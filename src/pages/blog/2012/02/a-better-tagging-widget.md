---
pubDate: "2012-02-01T22:30:42Z"
tags:
- planetkde
- nepomuk
- tags
title: A better Tagging Widget
layout: "@layouts/BlogPost.astro"
---

A long long time ago, a very simple tagging widget was implemented. We
always though - "Eh! This is temporary. We'll come up with a better one
later." But that never happened.

![The old widget][]

There is a lot of code in Nepomuk. However most of it is backend stuff
which does absolutely marvelous things behind the scenes - Auto
duplicate merging, type checking with respect to the ontologies, caching
and lots more. We, however, lack good UIs.

So, if you're a UI designer looking for a challenge, look at Nepomuk. We
have a lot of data.

Anyway, enough promotion! Unlike [yesterday][], I won't be pointing you
towards the source (though it isn't that hard to find). I'll just be
showcasing some screenshots. You'll get to try out the tagging widget
and *whatever-is-in-store-for-tomorrow* on Friday.

![image][]

![image][1]

This was originally implemented with a QListView in flow mode with a
custom delegate for tags. Getting it to automatically resize was a pain,
and I was missing out on a lot of effects. Eventually, a couple of hours
back, someone at *\#qt* pointed me towards [Flow Layouts][].

I'm in the process of rewriting the old item delegate code, to a widget
based one. Minus minor variations it should look the same.

As last time, if someone can make a nice mockup, I'll be more than happy
to implement it :)

  [The old widget]: /blog/images/2012/02/01/tagging-widget-old.png
  [yesterday]: http://vhanda.in/blog/2012/01/nepomuk-tag-manager/
  [image]: /blog/images/2012/02/01/tagging-widget-new.jpeg
  [1]: /blog/images/2012/02/01/tagging-widget-new-autocomplete.png
  [Flow Layouts]: http://developer.qt.nokia.com/doc/qt-4.8/layouts-flowlayout.html
