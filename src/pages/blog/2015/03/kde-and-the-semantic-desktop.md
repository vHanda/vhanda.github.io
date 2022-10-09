---
pubDate: "2015-03-13T17:11:22Z"
tags:
- planetkde
- nepomuk
- baloo
title: KDE and The Semantic Desktop
layout: "@layouts/BlogPost.astro"
---

During the KDE4 years the Semantic Desktop was one of the main pillars of KDE. Nepomuk was a massive, all encompassing, and integrated with many different part of KDE. However few people know what The Semantic Desktop was all about, and where KDE is heading.

## History

The Semantic Desktop as it was originally envisioned comprised of both the technology and the philosophy behind The Semantic Web.

The Semantic Web is built on top of [RDF](http://en.wikipedia.org/wiki/Resource_Description_Framework) and Graphs. This is a special way of storing data which focuses more on understanding what the data represents. This was primarily done by carefully annotating what everything means, starting with the definition of a resource, a property, a class, a thing, etc.

This process of all data being stored as RDF, having a central store, with applications respecting the store and following the ontologies was central to the idea of the Semantic Desktop.

The Semantic Desktop cannot exist without RDF. It is, for all intents and purposes, what the term "semantic" implies.


#### What did Nepomuk actually provide?

It's important to look at the core features a technology provided from a pure user point of view. After all, we're developing software for users. They are the end goal.

With KDE SC 4.12, which was the last release with Nepomuk. It provided Desktop Search. This included File Search, Emails, and Contacts.

It provided a way for users to store tags, ratings and comments.

It also provided a rich, but complex platform for applications to be built on. I use the term "complex" because storing the information was quite mind boggling at times.

Due the complexity and high costs very few applications actually embraced this idea. Having a huge central store was limiting, and using RDF just made it harder. Some of the notable applications were - Amarok, Bangarang, Rekonq, and KGet. However, Nepomuk was almost always optional, and not part of the core feature set.

Telepathy and Akonadi also tried to use it via a library called KPeople which tried to aggregate person information. It wasn't a fun exercise.

#### Where are are now?

Since almost all applications using Nepomuk, used it optionally, it could be easily removed. Where it could not be just dropped, a specific technology to tackle that particular problem was employed.

##### Baloo

[![](/blog/images/2015/03/13/8278933920_b685553977_z.jpg)
](https://www.flickr.com/photos/tambako/8278933920)

The largest part of this move away from Nepomuk, was the creation of Baloo. This project was often sold under the misnomer of being KDE's new Semantic Search engine. I often feel that the description, while containing a ton of buzz words, really does stray away from what it really meant to be Semantic.

No researcher would call Baloo semantic.

While Baloo did take some time figuring out if it wants to handle all desktop search or just file search. It's goals were always centered around search.

In Plasma 5, The Baloo project is just a **file indexing and searching solution**. Nothing more.

##### Other Applications

As stated, Nepomuk and RDF, were just one of the many ways one could implement these features.

File tags, ratings and comments are stored in the file system as extended attributes.

KGet is soon going to be storing the download origin in the extended attributes of the file, just like chromium, wget and curl.

Rekonq is continuing to store bookmarks using KBookmarks, we are thus not duplicating the information and avoiding the problem of the data never being in sync.

A new video player is being worked on which revolves around fetching information from the web and remembering what you watched. These were some of the core features from Bangarang.

Amarok continutes to work just fine. Nepomuk was after all one of its many backends.

KPeople now stores person information in a custom database. The developers are much happier with this approach.

Akonadi's search functionality was completely split, and though it was called the `akonadi_baloo_indexer`. It really just shared the name, the database was completely independent. Now, with the KF5 Akonadi, searching will become even more tightly integrated that process will disappear forever. Akonadi also stores tags in their own database, which has nothing to do with Baloo or Nepomuk.

**tldr:**
The Semantic Desktop as it was envisioned is no more.
