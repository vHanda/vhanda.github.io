---
date: "2014-10-10T14:34:44Z"
tags:
- planetkde
title: Search Folders in Plasma 5.1
---

Everyone loves Search Folders. These are custom folders which can show you files based on a search result.

In the KDE4 days, Nepomuk provided some of this, and we even had a `tags://` protocol. Now, with Plasma 5.1, we're pushing the first incarnation of "Search Folders" built on top of Baloo.

Here are some quick examples -

    baloosearch://?query=Random Words
    baloosearch://?query=tag:Death
    baloosearch://?query=tag:Death OR artist:Coldplay
    baloosearch://?query=width:100 AND tag:Sunset
    
The query syntax supports simple property names which you would expect along with any values. These can of course be combined with AND and OR along with parenthesis.

In its current form this is just for power users who can type the full syntax, but eventually we'll be exposing this in a better way where the whole `baloosearch://?query=` part is hidden away.