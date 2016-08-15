---
layout: post
title: Nepomuk and KDE 4.9.1
date: '2012-09-03 13:36:55'
tags:
- planetkde
- nepomuk
---

Last week 4.9.1 was tagged, and it should release any day now. A large
part of my time was spent bug fixing and stabilizing some of the most
user visible features. This blog post highlights some of the more user
visible changes.

File Watcher
============

Apart from minor fixes such as extra checks for [buffer overflows][],
and kernel [version checks][], we have two major improvements:

Memory Leak
-----------

The Nepomuk File monitoring service had a [serious memory leak][] in
4.9.0, which was unfortunately not caught in the beta or RC testing
period. Depending on the number of directories present, the file watcher
service would **consume a good 2 GB of your RAM**. Fortunately, many
distributions patched their own tarballs before releasing to the public.

Reindexing events
-----------------

Another bug was that lots of files were being unnecessarily reindexed
when they were opened in write mode even though no changes had been
made. With 4.9.1 we actually [check][] if the modification time of the
file has changed, even when the file notification system tells us
otherwise.

This should fix most of the problems of files unnecessarily being
reindexed.

File Indexer
============

The File Indexer has also gone through a number of fixes -

Do not check for new files on startup
-------------------------------------

One of the most annoying things I found about file indexing was the
initial check for new files on startup. Even though this check runs
silently in the background, it was very annoying. Specially for people
who run Nepomuk all the time.

![image][]

[Now][], we only check if the strigi version has changed, and only then
do we check for new files / check for any files that could not be
indexed the last time. If someone wants to forcibly check for new files,
they can do so with a dbus call -

`$ qdbus org.kde.nepomuk.services.nepomukfileindexer /nepomukfileindexer updateAllFolders false`

Or wait till 4.10, when I add an option to do so in the KCM.

  [buffer overflows]: https://projects.kde.org/projects/kde/kdelibs/nepomuk-core/repository/revisions/5609c4cdd8c7d938a9b3e99285b1044eea2fcf04
  [version checks]: https://projects.kde.org/projects/kde/kdelibs/nepomuk-core/repository/revisions/55761d35bb9e9ce863797b742c301d947dab61d0
  [serious memory leak]: https://bugs.kde.org/show_bug.cgi?id=304476
  [check]: https://projects.kde.org/projects/kde/kdelibs/nepomuk-core/repository/revisions/48d909c8aa4baca11e7bc1cf4ba5a23c1474fc22
  [image]: /blog/images/2012/09/03/scanning.png
  [Now]: https://projects.kde.org/projects/kde/kdelibs/nepomuk-core/repository/revisions/150a55d5eaabd8ad9a97112214fc2e008e9a1d11


Secondary Indexer
-----------------

This patch technically made it into 4.9.0, but I never got the chance to
blog about it. I've introduced a [SimpleIndexer][] which serves as
backup when the strigi indexers provide incorrect data. This way instead
of having no data about the file we at least have the basic information
such as filename, url and mimtype. I have plans for a more concrete
solution for 4.10, but that's for another blog post.

Nasty Deadlock
--------------

Another [annoying bug][] that was not caught with 4.9.0 was a deadlock
that rendered the file indexer service useless.

![image][1]

This only happened during the first run of Nepomuk, and led to some
[unfortunate publicity][]. In in the end it was a very simple fix once
the proper backtrace had been provided.

Strigi Analyzers
----------------

Another big change which probably has a large impact on indexing times
is the blacklisting of certain file indexing (strigi) plugins. Currently
by default, we only [blacklist the SHA1 hash generator][]. With it
blacklisted, we do not (hopefully) need to read the full contents of the
file. This results in a noticeable performance improvement for large
files.

Queries
=======

File Queries
------------

![image][2]

One of my favorite features of Dolphin is their Places Panel with all of
those defaults - Documents, Images, Audio and Video files.
Unfortunately, those defaults were normal Nepomuk queries and not file
queries. With 4.9.1, they have been changed to file queries which lets
them access a number of optimizations on our end, and allows them to
benefit from my [minor optimizations][] in file queries.

  [SimpleIndexer]: https://projects.kde.org/projects/kde/kdelibs/nepomuk-core/repository/revisions/414fd4c1c3c358aab70e1e10dd726ea2c1432e1f
  [annoying bug]: https://bugs.kde.org/show_bug.cgi?id=304982
  [1]: /blog/images/2012/09/03/nepomuk-deadlock.jpg
  [unfortunate publicity]: http://www.dedoimedo.com/computers/fedora-17-kde.html
  [blacklist the SHA1 hash generator]: https://bugs.kde.org/show_bug.cgi?id=303670
  [2]: /blog/images/2012/09/03/dolphin_places_panel.png
  [minor optimizations]: https://projects.kde.org/projects/kde/kdelibs/nepomuk-core/repository/revisions/328fbfd8a6fc66bf0b10bda7813b4827e3118d72

Query Updates
-------------

Most applications which currently use Nepomuk utilize the QueryService
in order to run queries. The Query Service can then provide updates on
those queries if there is some change in the results - It used to do so
by running **ALL open queries when any data in Nepomuk changes**. This
was not a good approach, as it obviously does not scale.

With 4.9.1, we are now using [some simple heuristics][] and the Resource
Watcher to only re-run queries when affected data changes. So indexing a
file will not result in the updating of all custom Nepomuk magic
folders.

Nepomuk Core Library
====================

And finally, the have been a number of [bug fixes][] and [crash fixes][]
in the NepomukCore library, but considering that they aren't really that
user visible, I'm not going to talk much about them.

Miscellaneous
=============

There have been a large number of improvements in the Nepomuk Testing
Suite, and more tests have been added. Most of the tests passed, but it
still serves as a good way of checking regressions.

Along with that there have been some [fixes][] with Removeable Media
handling.

Soprano
=======

Recently I blogged about certain major query optimizations, which would
only be public in 4.10. This major optimization was done by skipping a
large part of the Soprano code. After the blog post, we identified some
of the bottle necks, and optimized them. The most impressive result was
a reduction of one query from **6 seconds to 1 second**.

Again, this isn't really a part of KDE 4.9.1, but it will be a part of
Soprano 2.8.1. So when it releases, make sure you upgrade.

That's all for this release :)

  [some simple heuristics]: https://projects.kde.org/projects/kde/kdelibs/nepomuk-core/repository/revisions/ead226c9571a15da8d7a92810f5c4afd35bf9de8
  [bug fixes]: https://projects.kde.org/projects/kde/kdelibs/nepomuk-core/repository/revisions/e4d8cd1f76192dc798f2db09b9e19310d7c1d65f
  [crash fixes]: https://projects.kde.org/projects/kde/kdelibs/nepomuk-core/repository/revisions/7bef7c53d3b9a971c203ed4391bf19ac79f381f5
  [fixes]: https://projects.kde.org/projects/kde/kdelibs/nepomuk-core/repository/revisions/24caa3821aed71e590a3e55a76c6e4bc08f7d9d5
