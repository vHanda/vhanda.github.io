---
date: "2012-08-06T16:04:21Z"
tags:
- planetkde
- nepomuk
title: Nepomuk Without Files
layout: "../../layouts/BlogPost.astro"
---

Most people assume that if they switch off file indexing in Nepomuk,
then all the nepomuk file services will get disabled. This is however
not the case. Nepomuk consists of two services which are used to deal
with files -

-   Nepomuk File Watcher
-   Nepomuk File Indexer

The Nepomuk File Indexer is responsible for calling the [strigi
plugins][] to index the files, whereas the FileWatch service is a
general service that monitors file move, creation and deletion events.
Even when the *File Indexer* does not exist, files may have metadata
attached to them - Tags, Rating and Comments. We need the File Watcher
to update our database whenever the url of a file changes.

The File Watcher internally uses a kernel API for file monitoring -
inotify. This API, while quite easy to use, does not allow us to
recursively watch directories, and *more importantly*, does not provide
file move events unless we are watching both the source and the
destination directory.

We need file move events in order to track a file's url. This results in
us having to create inotify watches for every single directory in your
\$HOME folder. This causes a large disk load on startup and is the cause
of one of the [critical bugs][] in Nepomuk. And we have no solution,
until the kernel provides us with a better API.

Anyway, Nepomuk is being used in KDE PIM and Telepathy (development
version), and none of those use cases have anything to do with files. It
doesn't make sense to subjugate others to pay the price of the file
watcher, when they are not doing anything related to files. So, with
that in mind, please add the following lines to your nepomukserverrc, if
you do not care about files at all -

    [Service-nepomukfilewatch]
    autostart=false

Warning
=======

With the Nepomuk FileWatch Service disabled, you'll still be able to tag
and rate your files, but these annotations will be lost if you move or
rename the file.

  [strigi plugins]: https://projects.kde.org/projects/kdesupport/strigi/libstreamanalyzer
  [critical bugs]: https://bugs.kde.org/show_bug.cgi?id=233471
