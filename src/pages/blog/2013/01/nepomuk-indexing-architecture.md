---
date: "2013-01-29T18:36:55Z"
tags:
- planetkde
- nepomuk
title: Nepomuk Indexing Architecture
layout: "@layouts/BlogPost.astro"
---

Over the last 6 months, while working for [Blue Systems][], Nepomuk has
undergone a number of changes. The most public and noticeable change has
been the major refactoring of the file indexer. One large part of this
has been the [migration from Strigi][]. The other large part is the
introduction of 2 phase indexing.

With 4.9 and earlier, the file indexing service used to just have one
queue, whose speed could be controlled. This queue was filled on startup
by comparing the mtime of the file with the one stored in the database.
This would involve scanning through all the indexed folders. Once the
scan was complete it would also listen to the file watcher to be
notified when a file is modified or created.

This architecture had some shortcomings -

-   Indexing each file is a time consuming process, and it involves
    extracting and pushing large amounts of data in Nepomuk.
-   Since this process was slow and we did not want to annoy the users,
    artificial delays were introduced which were changed based on if the
    user is idle.
-   The entire indexing process was suspended when on battery
-   Faulty files which cannot be indexed do not have any information
    stored, and could not even be searched by filename

With this new release, we have split the indexing into 2 parts - Basic
Indexing and File Indexing. The basic indexing just extracts the stat
information and mimetype of the file. Whereas, the file indexing
actually extracts data from the file.

This basic indexing is always enabled, and is very fast. It can process
around 10-20 files per second. Also, it consumes very little cpu.
Extracting this basic information first allows us to search on the basis
of type, file and enabled the timeline kioslave to work properly.

The file indexing is the relatively heavy process that is only run when
the user is idle, [by default][].

This two phase architecture allows us to still index all the files,
while providing a relatively light burden to the user. It also allows us
to provide finer control than a simple on/off switch. For example - Now
when on battery, file indexing is disabled, but the simple indexing
still continues.

This new approach will also allow us to provide more user feedback in
future releases, such as an indexing progress bar.

Summary
=======

The new architecture is much faster and more resilient to abnormal files
and faulty plugins. It tries to save the basic information first, so
that one can easily answer simple queries. The full file information is
stored later, when the user is idle.

  [Blue Systems]: http://www.blue-systems.com/
  [migration from Strigi]: http://vhanda.in/blog/2012/11/nepomuk-without-strigi/
  [by default]: http://userbase.kde.org/Nepomuk/FileIndexer#Changing_the_default_behavior
