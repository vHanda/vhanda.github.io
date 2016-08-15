---
layout: post
title: Better unit tests
date: '2012-07-26 21:51:03'
tags:
- planetkde
- testing
- nepomuk
---

A little while ago, before [Akademy][], I started implementing the
Shared Hash Memory table, so as to improve Nepomuk's architecture.
Architectural designs really interest me. I decided to be smart about
it, if I was going to refactor some of the main [classes][], there was a
very high chance I would break something. The code is quite complex.

I needed a way to make sure I was on the right track.

The obvious conclusion was that we need comprehensive unit tests for
Nepomuk. Unfortunately Nepomuk architecture is such that unit testing is
a little hard. Quite often one requires a full blown nepomuk storage
service along with a dbus session running, just in order to run simple
tests.

This is where my [Test Framework][] came into play.

Last week, the Test Library was finally merged into nepomuk-core (4.9
and master), and I wrote a set of comprehensive tests for the Resource
class, which is **the most widely used** class in Nepomuk.

The good or bad part (depending on your viewpoint) is that the tests
were so comprehensive that only about 20% of the tests actually passed.
Ad-hoc testing cannot spot all issues, and automated testing is able to
find more problems that slip past anyone.

A couple of days, and about 20 commits later, we have all the important
tests passing. We are making massive steps forward in improving test
quality, though there's always more that can be done

  [Akademy]: http://akademy2012.kde.org/
  [classes]: http://api.kde.org/4.x-api/kdelibs-apidocs/nepomuk-core/html/classNepomuk2_1_1Resource.html
  [Test Framework]: http://vhanda.in/blog/2012/03/nepomuk-test-framework/