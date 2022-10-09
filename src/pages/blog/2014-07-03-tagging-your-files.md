---
date: "2014-07-03T15:43:07Z"
tags:
- planetkde
title: Tagging your Files
layout: "../../layouts/BlogPost.astro"
---

In 4.13, we moved away from a monolithic Nepomuk based system to a far more decentralized approach. Some parts of this are called Baloo, but to be honest, Baloo is not really responsible for managing your tags.

## The Nepomuk Days

Back in the era of Nepomuk, every time you would tag a file. 2 things would be done -

1. A tag would be created / fetched from the central Database
2. The tag would be linked with the file in the same database.

While this approach worked, a massive problem with it, was that all your precious tags were always stored in a central database. Modifying these tags needed this database (virtuoso) to be running, and more importantly, if anything were to happen to this database, all your tags would be lost.

This was one of the reasons why we developed backup and restore solution for Nepomuk. It was not to sure your file index, but rather these tags (and also the ratings and comments).

With Baloo, we are no longer responsible for storing the tags.

## So then where are the tags stored?
Internally, in the file system each file is typically identified by a unique number. This number then has many attributes associated with it. Some of the standard ones are - file name, permissions, user, group, etc.

Modern file Systems also allow applications to store their own custom attributes called extended attribute or xattr. It's a fairly common thing. In fact some popular applications such as chromium, curl and wget use them. Run the following command on a file downloaded through Chromium -

```
$ getfattr -d some-file-url
# file: some-file-url
user.xdg.origin.url="http://bugsfiles.kde.org/attachment.cgi?id=86198"
```

In KDE Platform, tags are similarly stored in these extended attributes. If you have tagged some file in a KDE Application, you can run the same command to try it out -

```
$ getfattr -d some-file
# file: some-file
user.xdg.tags="TagA,TagB"
```

## Why is this so great?

1. Your tags are never lost. No matter what you do to the file on any environment.
2. Baloo or another service has no play in tagging. You're simply talking to the file system.
3. We're using a [freedesktop standard](http://www.freedesktop.org/wiki/CommonExtendedAttributes) standard for comments. This same standard can be expanded for tags. So hopefully, we will be able to share tags among different Desktop Environments.

While this is awesome. It does however, come at a small price. We have no way of knowing which files have which tags unless we keep some kind of index relating the two together. This is where Baloo comes in. Baloo is just the search index. If you disable Baloo, then you can still safely tag files, you just cannot know which all files have been tagged by tag x.

Another small caveat is that the FAT file system does NOT support extended attributes. On those file systems we currently store the tags in a very simple sqlite db. However, with that we loose all the advantages listed above. It also means that the code to read and write tags becomes more complex.

We have been considering dropping tagging support completely in non supported file systems, however, that might just be my pipe dream.
