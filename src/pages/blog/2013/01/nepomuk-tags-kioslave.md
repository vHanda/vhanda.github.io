---
pubDate: "2013-01-31T18:36:55Z"
tags:
- planetkde
- nepomuk
title: Nepomuk Tags Kioslave
layout: "@layouts/BlogPost.astro"
---

Nepomuk has long required a convenient way of managing tags. I've
previously tried this with a simple [Tag Managing Application][], but
that wasn't something that we wanted to ship. For the KDE Workspace 4.10
release, we are releasing a Nepomuk Tags kioslave.

Listing Tags
============

The kioslave provides a very convenient way of listing all the tags. You
can even rename and delete tags, just like you would for any other
folder.

![image][]

Browsing Files
==============

Nepomuk has always provided users a way to browse tags, but it was only
one tag at a time. This seemed fairly limiting. Once could browse by
more tags, but then you would have had to write the [query][] yourself.

![image][1]

With this [kioslave][], you can finally browse the files based on the
tags, and then filter the search even more by selecting more tags.

Applying Tags
=============

The kioslave also supports adding of tags in bulk. Just drag and drop
(or copy) the files into the tagged folder, and the appropriate tags
will be applied.

  [Tag Managing Application]: http://vhanda.in/blog/2012/01/nepomuk-tag-manager/
  [image]: /blog/images/2013/01/31/nepomuk-tags-root.png
  [query]: http://userbase.kde.org/Nepomuk/kioslaves/search
  [1]: /blog/images/2013/01/31/nepomuk-tags-browsing.png
  [kioslave]: http://userbase.kde.org/Nepomuk/kioslaves/tags
