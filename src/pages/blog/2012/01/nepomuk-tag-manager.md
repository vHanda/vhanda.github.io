---
pubDate: "2012-01-31T22:22:39Z"
tags:
- planetkde
- kde
- nepomuk
- tags
title: Nepomuk Tag Manager
layout: "@layouts/BlogPost.astro"
---

Welcome to Nepomuk Tag Week! Well, not really, since it's not an
official thing. I've just been working a lot with tags lately, and this
week I'm going to be spamming you with some tag related updates (One for
every day of the week, minus Monday)

I thought I'll start with something small - Tag Management.

![Sweet and simple][]

We've been badly needing a UI to allow the users to modify, merge and
delete their tags. You could always delete this using the conventional
"Add Tag" dialog, but this way you can do batch deletes.

I'm not much of a UI designer so the interface is quite bare. I'm hoping
that someone can come up with a beautiful mockup, which I can then
implement.

And with this I can close [BUG 258323][].

**Source Code:** [kde:scratch/vhanda/nepomuktagmanager][]

**Update -**

I've added a Filter bar, merged the "Rename Tag" and "Merge Tags"
button, and double clicking on a tag now opens it in the file browser.

![Version 2][]

  [Sweet and simple]: /blog/images/2012/01/31/nepomuk-tag-manager.png
  [BUG 258323]: https://bugs.kde.org/show_bug.cgi?id=258323
  [kde:scratch/vhanda/nepomuktagmanager]: http://quickgit.kde.org/?p=scratch%2Fvhanda%2Fnepomuktagmanager.git&a=summary
  [Version 2]: /blog/images/2012/01/31/nepomuk-tag-manager2.png
