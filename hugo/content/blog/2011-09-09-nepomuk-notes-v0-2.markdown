---
date: "2011-09-09T16:58:56Z"
tags:
- planetkde
- kde
- nepomuk
title: Nepomuk Notes v0.2
---

Another week has gone by and I've decided to release another version of
my favorite note taking application. Like [last time][] the code is
still in [my scratch repository][].

New Features
============

Tagging Support
---------------

I've added tagging support. So, now you can tag all your notes. I didn't
want to use the traditional tagging widget, and wanted something that
was more usable. So I ended up with a simple text box where you can type
your tags.

![Tags! Tags! Your tags go here!][]

![Guess which one I use more frequently?][]

Background Blur
---------------

Most of the notes I write are personal, and I don't want to be bothered
by the rest of the world when I'm in my typing phase. One option is to
kill both the plasma-desktop and kwin, but I didn't think the users
would like that, so I've settled with just blurring the background.

![image][]

Show/Hide Animation
-------------------

The current default for showing and hiding the notes window is Alt + K.
I've been told that it might conflict with certain menus, so maybe I
should use a better default. The shortcut is obviously configurable like
everything else in KDE.

With v0.1 there wasn't any animation and the window would just appear.
It seemed rather odd at times. I've added a simple fade in and fade out
animation when you try to show/hide the window.

![That's the animation duration!][]

I'm Still Looking For a Good Name
=================================

I'm growing kinda of fond of the name **notably** which was suggested by
[Sebas][]. Does anyone have any other suggestions? I want to avoid using
the term Nepomuk in the name, as that is just the technology I'm using
to make this note taker awesome. The users shouldn't really be bothered
with those details.

Possible Keywords - *Semantic* and *Connected*

Maybe next week when I've decided on a name, I'll move it to the
playground.

Note Browsing
=============

There is still no way for you to browse the notes you've written. For
now, you can use the awesome [SemNotes][] application. Both Nepomuk
Notes and [SemNotes][] use Nepomuk as a storage backend, and since we
use the same ontologies, the notes are completely compatible.

  [last time]: http://vhanda.in/blog/2011/09/nepomuk-notes-v0.1/
  [my scratch repository]: http://quickgit.kde.org/?p=scratch%2Fvhanda%2Fnnotes.git&a=summary
  [Tags! Tags! Your tags go here!]: /blog/images/2011/09/09/nnotes2_tags.png
  [Guess which one I use more frequently?]: /blog/images/2011/09/09/nnotes2_tags2.png
  [image]: /blog/images/2011/09/09/nnotes2_background.png
  [That's the animation duration!]: /blog/images/2011/09/09/nnotes2_duration.png
  [Sebas]: http://vhanda.in/blog/2011/09/nepomuk-notes-v0.1/#comment-300752830
  [SemNotes]: http://gitorious.org/semnotes
