---
pubDate: "2013-04-16T17:40:00Z"
tags:
- planetkde
- nepomuk
title: Merging Nepomuk Graphs
layout: "@layouts/BlogPost.astro"
---

Since I've become the maintainer of Nepomuk we have put a strong emphasis on performance and stability. One of the core parts of Nepomuk are the high level operations that are exposed to the applications. These operations are typically used to insert and modify data into Nepomuk. Each of these operations is quite complex and involves a number of complicated queries.

For this 4.11 release we wanted to simplify that code and make it more efficient. This blog post delves in the technical details of what has changed, and then finally goes into how that affects the users.

# Graphs

It is often said that Nepomuk operates on triples in the format -

`<subject> <predicate> <object>`

While this is true, it skips over the fourth parameter which is the context or graph. Statements in Nepomuk are actually of the form -

`<subject> <predicate> <object> <graph>`

This fourth parameter allows us to store some additional information about the statement such as when it was added and who added it. The Nepomuk project has had a history of saving a lot of data without a clear usecase in mind. Graphs are a prime example of that.

Each statement does not have its own graph, rather a group of statements are clubbed together in one graph.

Before the 4.8 release, a new graph was created every 200 msecs. This graph just contained the creation date of the graph. This involved the insertion of 4 statements -

```
<graph> a nrl:InstanceBase .
<graph> nao:created “dateTime” .
<metaDataGraph> a nrl:GraphMetadata .
<metaDataGraph> nrl:coreGraphMetadataFor <graph> .
```

These 4 statements have never seen any use.

After the 4.8 release we introduced a set of central asynchronous APIs which performed a lot of the higher level functions so that application would not have to deal directly with statements.

This new API created a new graph each time a call was made to any of these higher level functions. The graphs that were now created were slightly more useful. They contained the following information

* creation date
* creating agent

The “Agent” in this case is the application that send the command for the data to be added. This extra “Agent” field allowed us some nice operations such as `removeDataByApplication` which allows applications to only remove the data they have added.

This functionality was already present for the file indexer in the pre-4.8 days. It was then generalized and made applicable to all applications.

The bottle neck is this plan is the creation of a new graph for each command. Even adding a simple property would result in a good 5-10 insert calls. This effectively kills our performance. For the 4.10 release I managed to combine a number of these insert calls and optimize the code, but we were still doing a large number of writes which served no purpose. We have never used the creation date of a graph in any way.

Additionally, we had to perform complex queries to make sure the data is always present in one graph, and check for empty graphs so that they could be removed. Overall, it was quite messy.

# New Graph Handling

With this 4.11 release I have simplified the concept of graphs. Now there are a limited number of graphs based on the number of Agents that push data into Nepomuk. Each Agent gets its own graph. This way we can still easily implement ‘removeDataByApplication’, and decrease the complexity of our code base.

This grossly simplifies the internals of the Nepomuk code base since no longer need to worry about all the complicated graph handling.

This big change is still in a `feature/mergeGraphs` branch. I'm still not completely ready to merge it into master.

# Benchmarks

These are initial benchmarks that were taken about a month ago. There is still scope for more optimizations. Especially if we combine more of our SPARQL calls. Also these benchmarks were run on a blank database. The difference should be a *lot* larger when there is some real world data.

| Function                           | 4.9    |   4.10  | 4.11  |
|:-----------------------------------|-------:|--------:|------:|
|addProperty()                       |130     | 79      | 32    |
|addProperty_sameData()              |60      | 34      | 32    |
|setProperty()                       |150     | 107     | 40    |
|setProperty_sameData()              |145     | 94      | 41    |
|storeResources()                    |85      | 52      | 21    |
|storeResources_email()              |300     | 85      | 77    |
|createResource()                    |26      | 14      | 8     |
|removeResources()                   |49      | 30      | 16    |
|removeDataByApplication()           |82      | 182     | 54    |
|removeDataByApplication_subResources|89      | 171     | 58    |
|removeAllDataByApplication()        |84      | 375     | 55    |

The numbers are in msecs. The functions are the higher level functions that all applications use to push any data into Nepomuk.

Whenever you add a tag in Nepomuk, the addProperty/setProperty methods are called. The file indexers and PIM feeder mostly use the `storeResources` function to push new data and `removeDataByApplication` to remove existing indexing data.

If you look at the results you'll notice a substantial increase with 4.10 except for the `removeDataByApplication` functions where they seem to have taken a severe hit. I'm not too sure why this has happened, the only change in that code base has been one bug fix which should have increased performance.

As noted above the number of graphs in Nepomuk are now limited and we no longer create superfluous graphs. However, all those extra graphs are still present and need to merged into a finite number. This can be a time consuming process.

Tomorrow, I'll go into the details of how we plan to counter that.
