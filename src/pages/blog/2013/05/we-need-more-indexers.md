---
pubDate: "2013-05-29T10:36:05Z"
tags:
- planetkde
- nepomuk
title: We need more Indexers
layout: "@layouts/BlogPost.astro"
---

With the 4.10 release of Nepomuk, we decided to [move away from Strigi](http://vhanda.in/blog/2012/11/nepomuk-without-strigi/) and write our own indexers. We support most of the commonly used formats. Also, the new code is faster and more importantly more maintainable and easier to contribute to. So far this decision has worked pretty well for us.

That being said - we still do not have enough indexers. Over the last week I managed to write simple ODF and Office2007 indexers, but we still need some more.

From the most commonly used file formats, we are still missing indexers for -

* Ebook file formats
* Office 97-2003 file formats (.doc)

If you have some time and would like to help out, [writing an indexer](http://techbase.kde.org/Projects/Nepomuk/IndexingPlugin) is a perfect way to get involved. Writing a plugin involves finding the appropriate library to parse the file format and extracting the plain text and basic metadata such as author, title, etc.

The MS Office 97-2003 formats are slightly harder because of the lack of a good high level library to parse the binary formats. However, we do have the [wv library](http://wvware.sourceforge.net/) which is a little low level, but can still be used.

It would be great to have these indexers for 4.11.

You can contact us the [nepomuk mailing list](https://mail.kde.org/mailman/listinfo/nepomuk) or on #nepomuk-kde
