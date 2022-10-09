---
date: "2013-05-29T17:36:05Z"
tags:
- planetkde
- nepomuk
title: More Nepomuk Performance Upgrades
layout: "@layouts/BlogPost.astro"
---

As you might have read, the 4.11 release of Nepomuk is [a lot faster](http://vhanda.in/blog/2013/04/merging-nepomuk-graphs) while doing writes and therefore indexing is going to much faster. However, read performance was nearly the same.

# Architecture

All application communicate to Nepomuk via the Storage Service which acts like a server. It is responsible for loading the ontologies, managing virtuoso and sending change notifications. The read communications happen over a local socket whereas the writes are sent over dbus.

![Original Architecture](/blog/images/2013/05/29/2013-05-before.png)

The Storage service communicates with virtuoso via odbc, which interally also uses a local socket. This architecture is quite similar to that of Akonadi.

# Cutting the middle man

I initially [started](https://commits.kde.org/soprano/f8747bc9d8fa916907d90b7f2bf84ffba923becb) [with](https://commits.kde.org/soprano/4437f5ab2d4448266e96fae981ba1c248727260c) [many](https://commits.kde.org/soprano/b8fb9056c201ba8ba9645c55d024a4d42b79a20a) [optimizations](https://commits.kde.org/soprano/dda25ad5818ef41832a141267f1b614a8dbc6b99) in Soprano where we use ODBC to communicate with virtuoso. These gave a good 30% increase in performance. However, the largest increase in performance was by removing the local socket communication between applications and the storage service.

![New Architecture](/blog/images/2013/05/29/2013-05-after.png)

All applications now directly communicate via virtuoso for reading data. The writes still go through the storage service so that we can do type checking and so that we can send the change notifications.

# Performance Increase

Removing the storage service from the middle results in a performance increase of 6-7x. For example - listing 50000 results goes down from ~20 seconds to about 2.5 seconds.

![Charts](/blog/images/2013/05/29/2013-05-charts.png)

Aditionally, since now the applications now directly communicate with the database, the CPU load of the Storage service also goes down a lot. It no longer has to serialize and deserialize data.
