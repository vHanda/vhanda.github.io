---
layout: post
title: Extended Attributes Updates
date: '2014-08-20 11:14:29'
tags:
- planetkde
---

Recently [I blogged](http://vhanda.in/blog/2014/07/tagging-your-files/) about how we have been reducing the number of databases in Plasma. We have been doing this by using the file system to store additional information such as tags, ratings and comments.

Unfortunately, extended attributes are not supported on all file systems. The most notable ones are "FAT" and any Network File Share. With Plasma 5.1, Baloo will **not** support tags, ratings or comments in these file systems.

## Why was this done?

With the previous code base, we had to maintain multiple code paths - one for xattr supported systems and one for the other ones. This complicated the code, and more importantly, it provided a false sense of "feature completeness". We supported tags on these other file systems, but they were -

* Stored in a custom database
* Not portable
* Not user visible mechanism to backup or restore these tags
* The tags break horribly over Network file systems as the tags would be stored locally.

With these reasons, we decided to drop support for them completely. It's better to not support something than to support them this poorly.

## Advantages?

Not only does this greatly simplify our code base. It also allowed us to easily restructure our architecture so that we can react to xattr changes.

Before 5.1, when an application changed the tags, they would also need to update the Baloo specific index, which meant that the user could not, theoretically, use a non KDE application to write tags.

With 5.1, we now monitor for xattr changes. This means that all applications now no longer interact with Baloo when reading or writing tags. They directly interact with the file system. This makes it really fast, and simplifies the code.

The `baloo_file` process monitors the file system and updates its index whenever the tags change.


