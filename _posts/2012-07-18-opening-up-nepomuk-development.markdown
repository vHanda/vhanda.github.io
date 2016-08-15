---
layout: post
title: Opening up Nepomuk Development
date: '2012-07-18 13:27:55'
tags:
- planetkde
- nepomuk
---

The Nepomuk development process has been fairly closed to outsiders.
This was never a conscious decision made by the Nepomuk Team, we just
never took the effort to open up the development, and make it more
appealing to new comers.

For most of the last 2 years, the development model has been
[Sebastian][] and I working on our own personal list of things, and
occasionally sending private emails to each other or communicating via
IRC.

I'm trying to change all of this. The aim is to make Nepomuk development
more open and friendlier to new developers. Getting developer feedback
at the [Nepomuk BOF][] at Akademy was the first step.

Compiling Nepomuk
=================

One of the largest hurdles in contributing to Nepomuk was that the code
resided in kdelibs. Fortunately, with 4.9, we have our own repository -
[nepomuk-core][], where most of the Nepomuk related code has moved.

For most of Nepomuk development, you just need to compile nepomuk-core.
However for some stuff like the kioslaves, controller, and kcm, you'll
still need [kde-runtime][].

Nepomuk Tasks
=============

Over the last week, I've created a separate mailing list for [Nepomuk
Bug Coordination][], and I've been adding simple tasks to bugzilla
(mostly from my personal TODO list).

The idea is to make the Nepomuk development process more public. Anyone
can see the current list of tasks that we are working on, and which
developer is working on which task (Assigned field)

Right now there are just about 20 tasks at Bugzilla. I'm in the process
of splitting down the big tasks, like [Fix Nepomuk Backup][], into
smaller chunks which individual developers can handle.

I've listed some simple tasks which do not require any knowledge of RDF,
SPARQL or the ontologies.

Beginner Junior Jobs
--------------------

-   [Port QueryService to ResultIterator][]
-   [Port nepomuksearch kioslave to Result Iterator][]
-   [Add Nepomuk/Vocabulary header][]


User Interface
--------------

-   [Nepomuk Controller Alignment][]
-   [Only show Indexing Options if the FileIndexer is enabled][]
-   [Split "Folders to Index" and "Include Exclude Filters"][]
-   [Discard "Query Base Folder listing" from the KCM][]
-   [Add a button to check for new files][]

Scripting
---------

-   [Proper Nepomuk2 namespace][]

Git skills
----------

-   [nepomuk-widgets repository][]

API Design
----------

-   [API for controlling list of indexed files][]

Medium Level tasks
------------------

-   [Disable SHA1 Analyzer][]
-   [Index new files when on Battery][]
-   [Port to QDbusConnection::Call][]
-   [Merge kded module into the QueryService][]

But where are you guys?
=======================

If you're interested come talk to us -

-   **IRC:** \#nepomuk-kde
-   **Mailing List:** [<nepomuk@kde.org>][]

Get involved!!

Also, if you have any suggestions on how we can make Nepomuk development
more appealing or be more open about what we are working on, please let
us know.

  [Sebastian]: http://trueg.wordpress.com/
  [Nepomuk BOF]: http://vhanda.in/blog/2012/06/the-nepomuk-bof/
  [nepomuk-core]: https://projects.kde.org/projects/kde/kdelibs/nepomuk-core
  [kde-runtime]: https://projects.kde.org/projects/kde/kde-runtime
  [Nepomuk Bug Coordination]: https://mail.kde.org/mailman/listinfo/nepomuk-bugs
  [Fix Nepomuk Backup]: https://bugs.kde.org/show_bug.cgi?id=303726
  [Port QueryService to ResultIterator]: https://bugs.kde.org/show_bug.cgi?id=303383
  [Port nepomuksearch kioslave to Result Iterator]: https://bugs.kde.org/show_bug.cgi?id=303382
  [Add Nepomuk/Vocabulary header]: https://bugs.kde.org/show_bug.cgi?id=303667
  [Nepomuk Controller Alignment]: https://bugs.kde.org/show_bug.cgi?id=303664
  [Only show Indexing Options if the FileIndexer is enabled]: https://bugs.kde.org/show_bug.cgi?id=303662
  [Split "Folders to Index" and "Include Exclude Filters"]: https://bugs.kde.org/show_bug.cgi?id=303661
  [Discard "Query Base Folder listing" from the KCM]: https://bugs.kde.org/show_bug.cgi?id=303660
  [Add a button to check for new files]: https://bugs.kde.org/show_bug.cgi?id=303658
  [Proper Nepomuk2 namespace]: https://bugs.kde.org/show_bug.cgi?id=303651
  [nepomuk-widgets repository]: https://bugs.kde.org/show_bug.cgi?id=303700
  [API for controlling list of indexed files]: https://bugs.kde.org/show_bug.cgi?id=303653
  [Disable SHA1 Analyzer]: https://bugs.kde.org/show_bug.cgi?id=303670
  [Index new files when on Battery]: https://bugs.kde.org/show_bug.cgi?id=303369
  [Port to QDbusConnection::Call]: https://bugs.kde.org/show_bug.cgi?id=303665
  [Merge kded module into the QueryService]: https://bugs.kde.org/show_bug.cgi?id=303666
  [<nepomuk@kde.org>]: https://mail.kde.org/mailman/listinfo/nepomuk