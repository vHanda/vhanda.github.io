---
pubDate: "2011-09-01T22:36:28Z"
tags:
- planetkde
- nepomuk
- notes
title: Nepomuk Notes v0.1
layout: "@layouts/BlogPost.astro"
---

About 5 months back I started jotting down notes about stuff that was going on. Some of them were personal, and some technical. For the personal stuff I had a separate folder where I would chronologically arrange my notes. A separate file for each day. The technical ones would be saved in other random places in variably getting lost. Thinking of a unique file name and file url is hard!

About a week after I started writing a lot of notes, I decided that I needed a proper way to categorize my notes. Nepomuk obviously came to mind. But I never started with the application.

More months went by, and I desperately wanted a more meaningful way to take Notes. I wanted my note taking application to understand when I was talking about a certain individual and what the note was about.

Eventually, it struck me - I need something as simple as [Yakuake](http://yakuake.kde.org/). A simple shorcut which opens up a text editor, lets me write my note, and then disappears.

So, after the [Desktop Summit](https://desktopsummit.org/), I took some time and wrote a very simple version of the note taker -

![Nepomuk Notes v0.1](/blog/images/2011/09/02/nnotesv0.1.png)

It's a nice change to actually use Nepomuk for creating something instead of working on Backend stuff all the time. Hopefully, as an application developer, I'll be able to better gauage the Nepomuk API and improve it as required.

The code is currently in a [scratch repo](http://quickgit.kde.org/?p=scratch%2Fvhanda%2Fnnotes.git&a=summary). Once I've added some proper features, I'll ask for it to be moved to playground/extragear.
