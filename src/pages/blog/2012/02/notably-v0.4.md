---
pubDate: "2012-02-08T18:22:43Z"
tags:
- planetkde
- nepomuk
- notably
title: Notably v0.4
layout: "@layouts/BlogPost.astro"
---

I meant to release a new version of Notably on Friday, but I got
sidetracked with some stuff. Plus, I've been spending a lot of time on
designing the UI for this release, which I think isn't a good idea.
Notably is still not quite mature, and I think right now features are
more important than polish.

Last week, I showcased some tagging UIs. They aren't yet ready to be
deployed in KDE, as they need to be polished quite a bit. Plus, there is
a lot scope for [collaboration][] when designing UIs.

Changes
=======

Revamped UI
-----------

I've gotten rid of most of the custom KWin code. I'd initially wanted my
application to look quite different, with a blurred background and fixed
size. But that would be locking the user into a fixed interface.

Notably now looks and behaves more like a KDE application. (No more
blurred background)

![image][]

Better Sidebar
--------------

Most of the code improvements have been in the sidebar, which now acts
as a proper menu and allows navigation.

![image][1]

Experimental Widgets
--------------------

Some brand new widgets;

### Tag Widget

I [showcased][collaboration] the new Tag Widget I was working on a
couple of days ago. Since then, I've improved the code to make it more
maintainable, unfortunately it still needs a lot of work.

![image][2]

### Tag Cloud

Creating a Tag Cloud turned out to be a greater challenge than I
expected. Right now it's implement with some basic HTML in a
QTextBrowser. I'm still experimenting with some custom layout code. Lets
see how it goes.

![image][3]

Tag Browsing
------------

You can browse your notes based on the tags they have been given. This
will eventually have to be expanded to allow multiple facets - like
tags, dates and so on. Implementing it on the Nepomuk side is fairly
simple, but I'm not sure about the interface.

![image][4]

After a couple of more releases when I've gotten most of the main
features down, I'll start on polishing it up and moving it to extragear
:)

**Source Code:** [kde:notably][]

  [collaboration]: http://disq.us/5cuoro
  [image]: /blog/images/2012/02/08/notably0.4-main.jpg
  [1]: /blog/images/2012/02/08/notably0.4-main-menu.jpg
  [2]: /blog/images/2012/02/08/notably0.4-tagwidget.jpg
  [3]: /blog/images/2012/02/08/notably0.4-tagcloud.jpg
  [4]: /blog/images/2012/02/08/notably0.4-hastag.jpg
  [kde:notably]: https://projects.kde.org/projects/playground/base/notably/repository/show?rev=v0.4
