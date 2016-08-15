---
layout: post
title: Nepomuk KIO Upgrade
date: '2012-07-10 19:16:43'
tags:
- planetkde
- nepomuk
---

In case you haven't been running trunk, or haven't tried out the latest
release candidates (you really should), you should know that dolphin has
had a lot of major improvements since the last release. One of the
features I really love is better Nepomuk integration. This enhanced user
visibility lands up exposing bottlenecks, and performance problems.
That's exactly what happened with the nepomuk search kio slave.

![The Nepomuk Architecture][]

Dolphin exposes this neat sidebar which allows users to list out the
different kind of files - Documents, Images, Audio and Videos. A little
more than a month ago, I tried it out and was quite disappointed with
both the performance and the results. It was quite normal for dolphin to
popup a message saying that 'nepomuksearch' kio-slave had crashed.

Needless to say, RC2 fixes most of the issues and the performance is
quite good.

The Original Architecture
=========================

![The Nepomuk Architecture][1]

Step 1 - Remove threading
-------------------------

If you look closely you can see the large number of useless threads that
are spawned in the process of one query. The kioslave spawns a separate
thread in the kio\_nepomuksearch process, and then the *Query Service*
spawns another thread on which it actually runs the query and send the
results back via dbus.

The thread of the nepomuk kioslave is just running an event loop waiting
for it to get the results over DBus. This is especially useless, cause
kioslaves can be blocking.

Step 2 - Skip the middle man
----------------------------

The second big bottle neck is the transmission of the query results over
DBus via the queryservice, and the extra thread that is spawned cause of
that. You might just ask - "What the point of the query service
anyway?". Well it exists to cache query results and to provide query
updates, so it isn't a complete waste.

However, the extra overhead didn't seem worth it.

The Current Architecture
========================

![The Nepomuk Architecture][2]

The extra threading has been removed from the kioslave, and it now
directly communicates with the storage service to run the queries. It
seems a lot cleaner this way, and is a lot faster.

  [The Nepomuk Architecture]: /blog/images/2012/07/10/dolphin_sidebar.png
  [1]: /blog/images/2012/07/10/Nepomuk_KIO_old_Architecture.png
  [2]: /blog/images/2012/07/10/Nepomuk_KIO_new_Architecture.png