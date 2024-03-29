---
pubDate: "2015-10-10T16:57:00Z"
tags:
- planetkde
- baloo
title: Baloo 5.15
layout: "@layouts/BlogPost.astro"
---

We have a new release of [Baloo](https://community.kde.org/Baloo). For those of you who don't know about it - It's a file indexing and searching solution for Linux. It's quite fast, and shipped by default in KDE Plasma.

Baloo is part of KDE Frameworks and has a fast 1 month release cycle. So, I'm probably going to be spamming you about the big changes in Baloo each month.

The [5.15](https://www.kde.org/announcements/kde-frameworks-5.15.0.php) release focused a lot on tooling and bug fixing.

One of our main tools `balooctl` got a few additional commands -

* `balooctl config` - For Managing your configuration. Before this our options were to either directly edit the config file or use the KCM shipped with Plasma.
* `balooctl check` - Forcibly check for updates. There are cases when we fail to pick up on file system events or baloo just isn't running. Executing this command makes baloo recheck if every file has been indexed.

Additionally, a large number of bugs have been fixed. The largest is fixing the corruption issue with the Baloo index. Internally, Baloo uses [LMDB](http://symas.com/mdb/) which provides us with a very efficient BTree. The Btree file is memory mapped and we get direct access to the kernel page table. This is dangerous but also makes us very efficient as we can avoid copies. One of the way of avoiding copies was to never copy words into memory and use [`QByteArray::fromRawData`](http://doc.qt.io/qt-5/qbytearray.html#fromRawData). This worked quite well, however since `QByteArray`, like all Qt containers, is [copy-on-write](http://doc.qt.io/qt-5/implicit-sharing.html), the original QByteArray would be copied and used even after the original had been destroyed.

For now we have [fixed this](https://git.reviewboard.kde.org/r/125362/) by copying the memory each time. It is a little bit slower, but much safer, and this fixes a large number of crashes. Unfortunately, this also means that the previous index needs to be discarded.

With Baloo 5.15, all your files will be reindexed from scratch. It's unavoidable.
