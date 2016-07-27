---
layout: post
title: Bluetooth Discovery
date: '2009-11-14 23:08:26'
tags:
- bluetooth
---

I just rediscovered this blog I'd found a long time back. It's called *<a href="//steve.yegge.googlepages.com/blog-rants">Stevey's Drunken Blog Rants</a> *and it's a really old blog. By really old I mean 4-5 years old - h . I had originally found it when I was googling 'The Singleton Design Pattern' and reading up on why it's so bad and shouldn't be used. EVER! (Unless you're building a logging class.) <a href="http://stackoverflow.com/questions/228164/on-design-patterns-when-to-use-the-singleton">Here</a> is a really nice summary, in case anyone is interested.  Anyway, I really love his post about *<a href="http://steve.yegge.googlepages.com/you-should-write-blogs">Writing Blogs</a>*, particularly why everyone should write one, and that inspired me to start blogging again. Just emptying the contents of my head into a not-so-great W*ordpress* widget.

Coming to the problem. What problem? I just said I'll start writing, so what kind of problem can there be? Well there is one - *Categorization*. I hate it! I absolutely despise it. It forces me to write about a singular topic and not just go on and on about anything and everything. In a way I suppose that it's good most people don't do that. It helps search engines find stuff easily, but it hinders creativity. So for now I'm just going to write about whatever I feel like and later on - maybe - try to categorize it.
<h3>*The Bluetooth Part*</h3>
So, *Bluetooth Discovery, *that's what the title says, and that is what I'm going to talk about, in a way. A couple of days back, I have chatting with my friend about something (I don't really remember) when the topic of Bluetooth adapters came up and how they are quite cheap. I always thought they would be super expensive. Okay not super expensive, but atleast kinda. Little did I know they just cost around 100 bucks. (INR) So after being amazed by this discovery, I planned to buy one for my desktop, but my friend beat me to it and got me one. It as big as a small pen-drive. Really neat!

One of the main reasons I wanted one was, so that I would be able to transfer stuff from my mobile (N73) to my comp and vice versa without having to connect it via the USB cable, which my brilliant rabbit has chewed up a little bit so it doesn't always work. And now I can!

Once I was over the initial craze of being able to transfer stuff so easily. I started to think - "Hmmm .. what else could I do with Bluetooth". And that's when it hit me, that maybe ... maybe I could use my mobile as a remote to control the music on my Desktop. That way I could lie down on my bed. which is about 7 feet from my comp and change the music. Yes, I know, it's super lazy and I would maybe just use it for a couple of days or weeks. But it seems like a really interesting thing to do! And I'm sure many people would have done it before, but I don't care!

I remember seeing a you-tube video of something similar a couple of months back, where this guy would stream in movies form his comp to the mobile via Bluetooth. I'm going to try to implement something similar to that, except that I'll be streaming in my playlist.
<h4>**Programming Perspective**</h4>
From the looks of it I'll probably need two applications, one which run on my desktop and the second which will run on my mobile. Now what language should I use?

***Desktop Application -*** I can probably use any programming language I want - Python, Java, C or C++. I think I'll most probably use Python for now, as it's quite simple and I don't think I need a full blown application. Java is nice, but I don't have much experience with Java (maybe that's why I should use it.) Later on maybe I could use C++ and create a KDE Plasma widget. I can/should probably use the D-Bus IPC interface to access amarok/other-music-player.

***Mobile Application -*** I only have 2 options here - Java or Python. Personally I prefer Python over Java, but I don't really know what I'll use - so let's see.

Yea, well that's about it for now. Maybe tomorrow I'll actually start working on it, instead of just thinking what it could be!
