---
pubDate: "2012-07-05T16:16:43Z"
tags:
- planetkde
- nepomuk
title: The Nepomuk BOF
layout: "@layouts/BlogPost.astro"
---

For those of you who don't know, I'm currently at Tallinn, Estonia for
[Akademy 2012][].

Here at Akademy, yesterday was an amazing day for Nepomuk. Early morning
we had a [Nepomuk BOF][] titled '**Constructive Criticism - Prioritizing
Nepomuk development**', and it was really good. I wasn't sure if I
shouldn't have named it such, cause that might have ended in people just
coming and complaining, but surprisingly everyone was really nice and
constructive.

![Akademy 2012][]

During the start of the sprint, we decided to list down each of the
Nepomuk components along with their respective developers. Once that was
done, I just explained what we are currently working on, and then
'request for comments' period started.

The initial discussion was related to the PIM feeders and how they still
regularly cause high cpu spikes. I don't personally use PIM, and since
[Christian][] is that maintainer of that code base, he handled most of
the comments. The overall conclusion was that we need to schedule the
indexing at a better time. For this, we would need some help from Solid
for better idle detection.

After this we started with a discussion of the most user visible
application using Nepomuk -- Dolphin. For one it should only be showing
files, and the listing to the query results should be a lot faster.
Maybe even something along the lines of on-demand loading. And finally,
we talked about how it is not possible to search through both the file
name and content at the same time.

![The Nepomuk BOF WhiteBoard][]

With this discussion we obviously started discussing the current search
interfaces. We wanted a separate plasmoid just for searching (something
similar to Crystal), but someone mentioned that we already have 2 places
for searching in KDE - krunner and kickoff, and both of them do not show
the same results, which can be confusing. [Marco][], from Plasma,
mentioned that maybe krunner could be modified to show the search
results in a better way. Eventually, we concluded that this is an open
discussion which still needs a proper solution.

A couple of other issues that were discussed we better technical
documentation, ontologies, old data cleanup and migration. We also
discussed shipping meta data writeback, which is the process of writing
the Nepomuk Metadata back into files/Akonadi/some other representation.

We then discussed the state of Nepomuk and media players - Amarok,
Plasma Media Center, and Bangarang, and how there may be a need for a
central library for managing multimedia via Nepomuk.And finally went on
to discuss the current state of Digikam and Nepomuk integration which is
very old, and needs a lot of work.

And that was it. Well, not quite.

Later during the day, after the Randa sprint, we had an impromptu
meeting discussing the state of meta-contacts and its support in both
Telepathy and Akonadi. Fortunately, we had all interested parties
present - Telepathy and Akonadi developers, and the discussion went on
for quite some time.

![Meta Contact Whiteboard][]

My initial approach for solving meta-contacts using the ontologies had
some flaws. Having all the interested developers discuss it out and come
to a conclusion was amazing. It's difficult to get started with
implementing proper Nepomuk support for Telepathy if we all cannot
decide on how to use the ontologies.

We even went on to discuss semi-automatic contact merging and a lot of
stuff related to Telepathy. Overall, a really productive day!

  [Akademy 2012]: http://akademy2012.kde.org/
  [Nepomuk BOF]: http://community.kde.org/Akademy/2012/Wednesday
  [Akademy 2012]: /blog/images/2012/07/05/Akademy2012_imat.png
  [Christian]: http://cmollekopf.wordpress.com/
  [The Nepomuk BOF WhiteBoard]: /blog/images/2012/07/05/Akademy2012_NepomukBof.jpg
  [Marco]: http://www.notmart.org
  [Meta Contact Whiteboard]: /blog/images/2012/07/05/Akademy2012_MetaContacts.jpg
