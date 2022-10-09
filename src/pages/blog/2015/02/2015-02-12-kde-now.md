---
date: "2015-02-12T21:27:48Z"
tags:
- planetkde
- kde-now
title: KDE ... NOW
layout: "@layouts/BlogPost.astro"
---

Last week I got decently annoyed by having to lookup my flight information before FOSDEM. Also, you always forget when your flight is afterwards.

Google started "Google Now" which aims to provide useful information about current events through your Android phone. But that does involve it going through your email.

With that in mind, as a weekend project, I started to write a replacement. Initially, I started with just scrapping the information from the emails. I assumed they'll just be some 100 popular airlines. However, after writing the first couple of extractors I stumbled on a widely used [schema](https://developers.google.com/gmail/markup/reference/flight-reservation).

This is mostly just a weekend hack, but it does seem like an interesting project to continue. Specially given the large number of information we can extract.

![](/blog/images/2015/02/12/flightinformation.jpeg)

A **Flight Information** plasmoid, which shows information about future flights.

## How it works

We have a `kdenowd` daemon which goes through your IMAP server, fetches yours emails, and tries to extract Flight Information from the emails. A lot of airlines seem to use this standard schema in their emails, but for those which do not, I started writing scrappers. We will eventually need to use a combination of both.

The Akonadi-Qt5 release isn't quite ready now. So we're directly creating our own IMAP connection. This is not ideal, as writing an IMAP client is not trivial, but as a starting point, this seems to work well.

### Trying it out

The code is available in my scratch repository - [kde:scratch/vhanda/kde-now](http://quickgit.kde.org/?p=scratch%2Fvhanda%2Fkde-now.git). You'll need to run the `kdenowd` process with the correct IMAP Server information, and then add the plasmoid.
