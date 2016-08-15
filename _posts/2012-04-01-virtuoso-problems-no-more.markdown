---
layout: post
title: Virtuoso problems no more
date: '2012-04-01 12:16:43'
tags:
- planetkde
- nepomuk
- virtuoso
---

The Nepomuk team has been working really hard to fix the problems with
`virtuoso` consuming too much memory and often just going bat crazy. And
now finally we've figured it out.

It wasn't the obvious solution, but we think it's going to work out very
well. From now on `virtuoso` won't be shown in `ksysguard`.

![April Fools!][]

Since we won't be able to see virtuoso using up our memory and CPU, it
obviously won't be doing it. Now, I get that this logic is a little
brain-dead, cause in Linux we have other ways of monitoring our
processes as well.

Fortunately, [Trueg][] is in the process in patching up `top`, and I'm
going to be contacting the kernel people to see if we can remove
virtuoso PID from `/proc`.

Problem solved! :)

  [April Fools!]: /blog/images/2012/04/01/ksysguard-virtuoso.jpg
  [Trueg]: http://trueg.wordpress.com/


