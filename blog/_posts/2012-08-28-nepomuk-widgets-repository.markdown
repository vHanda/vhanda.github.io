---
layout: post
title: Nepomuk Widgets Repository
date: '2012-08-28 11:36:18'
tags:
- planetkde
- nepomuk
---

With KDE 4.9, we introduced a new repository called [nepomuk-core][].
This contained a combination of [kdelibs/nepomuk][] and
[kde-runtime/nepomuk][]. It was created because of the API freeze
present in kdelibs. Considering that most of the client libraries are
thin wrappers over the runtime components, it made sense to combine them
in one repository..

In order to be compatibile with kdelibs, the new library is installed
with the Nepomuk2 namespace.

Now with KDE 4.10 we are going to have another new repository
[nepomuk-widgets][]. This repository contains the remaining GUI parts of
[kdelibs/nepomuk][] that were not moved to `nepomuk-core`.

Port your Applications
======================

With this repo, we have all of the earlier functionality covered. **With
4.10, the Nepomuk libraries will be deprecated**. So, port your
applications to `Nepomuk2`. I've updated [the wiki][] with a short
script that should do take care of most of the changes. You will have to
update your CMake files on your own.

Advantages
==========

The [kdelibs/nepomuk][] libraries are in a critical bug-fix state only.
That being said, some of the most important classes over there do not
have any kind of tests. With `Nepomuk2`, we have decent test coverage,
and active development. Plus, you get access to a number of new
asynchronous APIs.

  [nepomuk-core]: https://projects.kde.org/projects/kde/kdelibs/nepomuk-core
  [kdelibs/nepomuk]: https://projects.kde.org/projects/kde/kdelibs/repository/revisions/master/show/nepomuk
  [kde-runtime/nepomuk]: https://projects.kde.org/projects/kde/kde-runtime/repository/revisions/master/show/nepomuk
  [nepomuk-widgets]: https://projects.kde.org/projects/kde/kdelibs/nepomuk-widgets
  [the wiki]: http://techbase.kde.org/Projects/Nepomuk/Nepomuk2Port