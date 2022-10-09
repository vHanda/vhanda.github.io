---
pubDate: "2014-04-17T17:09:15Z"
tags:
- planetkde
- nepomuk
- baloo
title: Desktop Search Configuration
layout: "@layouts/BlogPost.astro"
---

**Update:** *This blog post is quite old and does not reflect the current state of Desktop Search in KDE. If you're just looking to disable it, you can either run the `$ balooctl disable` command or go to System Settings -> Search -> File Search -> Disable.*

KDE SC 4.13 is finally out. As you may have heard this marks the release of Baloo. The bear is now out in the wild!

One of the many things that has changed is the "Desktop Search" configuration module. This blog post is about why the changes were made and what the rationale was behind it.

## The Configuration Module

![Main Nepomuk Configuration](/blog/images/2014/04/17/nepomuk_kcm1.png)

The old module was built on top of Nepomuk and it explicitly mentions that on the top. While "Nepomuk" was a decently known brand within the KDE Community, a normal user cannot be expected to know what it is.

It also presented a plethora of options in order to disable and enable Nepomuk, the File Indexer (used to say Strigi) and the PIM indexing. It also provides ways to control the indexing and labels to show what exactly is going on.

With the new module we have removed all of these options. The user should not need to know about the project called "Baloo", and indexing is an internal implementation detail in order to make searches faster. It doesn't need to be broadcasted.

Additionally now that we're so much faster, we can no longer afford to inform the user about each file. We often indexing hundreds of files per second. Plus, the files are indexed in multiple stages.

The old module's primary function was to control what is indexed. This was done in another tab -

![Nepomuk Indexing Configuration](/blog/images/2014/04/17/nepomuk_kcm2.png)

This allowed the user to set which types and mimetypes of files should be indexed, control the list of regular expressions which should be matched against the filename, and control the list of directories which should be indexed, and which should not.

Quite a few things.

In contrast, the new KCM is quite minimalistic.

![Baloo KCM](/blog/images/2014/04/17/baloo_kcm.png)

Here are some of the key things that we have changed -

* The KCM now uses a new search icon which clearly demonstrates that this is related to search.

* We now index your HOME directory by default and allow you to black list certain directories which you may not want to appear in the search results.

* The custom regular expression and mimetype filtering has been removed. We believe that it wasn't something an average user would know or care about. Also, we don't think anyone should need to micro-optimize the indexer to such a level.

* This current release does not fully support removable media. By default all removable media are not indexed. You can however remove them if the blacklist in order to index them.

* There is no explicit "Enable/Disable" button any more. We would like to promote the use of searching and feel that Baloo should never get in the users way. However, we are smart about it and IF you add your HOME directory to the list of "excluded folders", Baloo will switch itself off since it no longer has anything to index.

* Tags, Rating and Comments are no longer mentioned as they are not related to "Desktop Search". I'll talk about how they are now handled in another blog post.

This new KCM does remove a large number of options, but we considered those to potential optimizations from the Nepomuk days when we were not that performant. Baloo does still support most of these options, they just have sane defaults and are no longer exposed.
