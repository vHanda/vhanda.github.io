---
date: "2012-11-07T21:36:55Z"
tags:
- planetkde
- nepomuk
title: Nepomuk without Strigi
layout: "@layouts/BlogPost.astro"
---

Strigi has always been a large part of Nepomuk. In fact a lot of users
still do not understand the difference between the two. It's quite
common to see bug reports saying mentioning "Strigi/Nepomuk". Lots of
blog posts do the same.

Strigi consists of a number of different parts. In Nepomuk we just used
to use libstreams and libstreamanalyzer. These were pure C++ libraries.
The great thing about Strigi is that it is based on streams, instead of
files. So one can theoretically even extract metadata from the album
image embedded inside an audio files. It's very powerful. Unfortunately,
everything comes at a price, and this increased "awesomeness" comes with
increased complexity. Additionally with it being a pure C++ ( no Qt or
KDE ) library, contributing is harder.

For 4.10, We decided to take a very drastic change and move away from
Strigi. There are a large number of [reasons][] for doing so. Apart from
the technical ones there was also an economic one - A large code base
like Strigi is difficult to maintain and comes with a lot of added
complexity.

Our own solution is based only on files (not streams) thereby making it
a lot simpler. It directly uses the Nepomuk and KDE libraries, thereby
making integration very simple. Integrating Strigi in Nepomuk required a
lot of code.

This new file indexer currently resides in the nepomuk-core repository
and does not have a public interface. I'm currently still debating if it
should be public for 4.10. Write about if it gets a public interface,
one can theoretically write plugins in other languages.

So far we have 5 indexers -

-   Image File - Based on Exiv2
-   Video Files - Based of ffmpeg (We might move to gstreamer)
-   Audio Files - Taglib
-   PDF Files - Poppler
-   Plain Text files

Writing file indexers for Nepomuk is now very simple. In fact these 5
indexers combined are just 500 lines. Here is the important part of the
plain text extractor -

    QTextStream ts( &file );
    QString contents = ts.readAll();

    int characters = contents.length();
    int lines = contents.count( QChar('\n') );
    int words = contents.count( QRegExp("\\b\\w+\\b") );

    SimpleResource fileRes( resUri );
    fileRes.addType( NFO::PlainTextDocument() );
    fileRes.addProperty( NIE::plainTextContent(), contents );
    fileRes.addProperty( NFO::wordCount(), words );
    fileRes.addProperty( NFO::lineCount(), lines );
    fileRes.addProperty( NFO::characterCount(), characters );

  [reasons]: http://mail.kde.org/pipermail/nepomuk/2012-September/003167.html


The current file indexers cover most of the commonly used files, but
they still need to be polished. So, if you're interested in contributing
to Nepomuk, here is your chance.

I've managed to [catalog][] some of the different files that I know we
support. Our current indexers support many more formats, they just need
to be properly tested.

If you're interested in helping, you can start by running nepomuk-core,
and manually indexing the different file formats and updating this
[page][catalog]. If you're a developer, feel free to checkout
nepomuk-core, and start writing extractors. I've written a simple
[guide][].

Btw, all of this Nepomuk awesomeness is powered by Blue Systems!

  [catalog]: http://community.kde.org/Projects/Nepomuk/FileIndexing
  [guide]: http://techbase.kde.org/Projects/Nepomuk/IndexingPlugin
