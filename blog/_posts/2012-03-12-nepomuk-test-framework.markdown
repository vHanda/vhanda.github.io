---
layout: post
title: Nepomuk Test Framework
date: '2012-03-12 18:17:13'
tags:
- planetkde
- nepomuk
---

Back in [October 2010][], I was trying to write automated tests for
Nepomuk Backup. That turned out to be a huge disaster, but the test
suite was still had some pretty good stuff in it.

Most of it was based on [George Goldberg][]'s [Telepathy Test Lib][].

Over the last month, I've finally moved it to [git][], cleaned it up and
started using it for real tests.

Why do we need a testing framework?
===================================

The Nepomuk Architecture is extremely decentralized. We have one central
storage service which handles virtuoso, and other different services for
monitoring files, indexing them, performing queries and so on.

These services or plugins often require other services to be running and
form a dependency chain. In order to properly test any them, we need the
dependencies to be satisfied. That's where it starts to get messy.
Specially cause Nepomuk primarily uses local sockets and DBus to
communicate.

Right now we have a [lot of tests][] that test individual classes from
the services, but nothing that tested if files are actually getting
reindexed after they are modified. Such things were always tested
manually.

Now, with this testing framework, we can launch a separate KDE and DBus
session and run tests.

Another reason why we really need this is that from KDE 4.8.1
[Nepomuk::Resource][] also uses DBus in order to write back any of its
changes. That effectively kills all of its current unit tests.

And we need unit tests!

Source Code
===========

The code is still in [my scratch repo][git], and there are very few
tests, but we're getting there. It has already helped me replicate a
nasty PIM Feeder bug, so Yaye! :)

  [October 2010]: http://websvn.kde.org/trunk/playground/base/nepomuk-kde/testlib/
  [George Goldberg]: http://grundleborg.wordpress.com/
  [Telepathy Test Lib]: https://projects.kde.org/projects/playground/network/telepathy/ktp-testlib
  [git]: http://quickgit.kde.org/index.php?p=scratch%2Fvhanda%2Fnepomuk-testlib.git&a=summary
  [lot of tests]: http://quickgit.kde.org/index.php?p=kde-runtime.git&a=blob&h=9b366ac3294c41d2bbe316f1c64c2fdb98eebf97&hb=feeadd5c4fed937864944e9d375b3441239c4e12&f=nepomuk%2Fservices%2Fstorage%2Ftest%2Fdatamanagementmodeltest.h
  [Nepomuk::Resource]: http://api.kde.org/4.x-api/kdelibs-apidocs/nepomuk/html/classNepomuk_1_1Resource.html
