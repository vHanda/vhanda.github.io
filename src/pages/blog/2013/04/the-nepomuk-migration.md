---
pubDate: "2013-04-19T16:36:55Z"
tags:
- planetkde
- nepomuk
title: The Nepomuk Migration
layout: "@layouts/BlogPost.astro"
---

A couple of days ago I [talked](http://vhanda.in/blog/2013/04/merging-nepomuk-graphs/) about how we have been clearing up some unwanted data in Nepomuk for the 4.11 release - mainly graphs. This change comes with a increased performance of over **100%** in many cases, and makes the codebase simpler, and easier to maintain.

Unfortunately, it comes at a cost.

The graphs in the old database need to be merged to a small number. This operation is a very time consuming process cause merging graphs is equivalent to slowly removing your entire database and reinserting it.

Given that all users will *have* to go through this migration. We decided to add some additional methods of migrating.

![Nepomuk Migration](/blog/images/2013/04/19/nepomukmigrator.png)

1. **Backup Tags and Rating** - The user can choose to only backup their file tags and ratings, remove the entire database, and then restore the ratings. The graph merging process is only performed on the tags and ratings when creating the backup.

    This process is very fast and is the recommended way.

    ![Nepomuk Backup Migration](/blog/images/2013/04/19/nepomukmigrator_backup.png)

    Once this has been performed, all your files and emails will need to be indexed again, which is actually a good thing cause historically a lot of the indexed data has been quite inferior.

2. **Migrate the existing Data** - We can obviously go through the slow process of migrating all of the graphs. This can however easily take a couple of hours for medium sized databases (2.5 gb). I would not recommend this unless you have some really important data that you added on your own that option (1) does not cover.

3. **Start Afresh** - Just remove the existing database and start with a fresh Nepomuk installation.

So, far the user is given a choice the first time Nepomuk runs in 4.11. It's a little ugly, but that can be fixed.

I'm hoping that this will not be too much of a pain and that the users can just click through the wizard using the default option (1). For medium sized databases this entire process gets done in just a couple of minutes.

On the positive side, because of this migration I had a chance to fix and test Nepomuk Backup - Just backing up the Tags and Ratings works very well right now. Backing up the full Nepomuk system still needs to be tested.

I recommend developers to checkout the `feature/mergeGraphs` branch and try out the migration and just use that branch of Nepomuk for a while. When I'm confident about the migration and the internal changes, I'll merge it into master.

Happy testing!
