---
pubDate: "2013-03-21T22:36:55Z"
tags:
- planetkde
- nepomuk
title: Nepomuk - Simplifying the debugging process
layout: "@layouts/BlogPost.astro"
---

When something goes wrong in Nepomuk, its easy for us Nepomuk developers to track it down, but for other developers and users it can be quite hard. Even simple things like reporting which component is malfunctioning isn't completely obvious.

Over the last month, we have simplified some of the external details and added tools which will help us debug your problems so that we can fix things more easily. These all will be shipped with nepomuk-core in 4.11

# Nepomuk2::Service2

Nepomuk, like most modular architectures, has a number of different plugins or as we like to call them "services". Traditionally each service would be installed as a library that would be loaded by the `nepomukservicestub` process. When most users would try to provide debugging information they mostly just provide the process name - nepomukservicestub. This doesn't tell us much, since all the heavy lifting is done by the nepomuk services. The client libraries are mostly just light wrappers.

With the 4.10 release we have 3 major services -

* Storage Service
* File Watch Service
* File Indexing Service

Each can be started by calling the `nepomukservicestub` along with the service name. Eg - `nepomukservicestub nepomukfilewatch`.

Currently in master, we have moved away from this approach and each service now installs its own process. So you should no longer see any nepomukservicestubs. Instead you'll see a `nepomukstorage`, `nepomukfilewatch` and `nepomukfileindexer` process.

This greatly simplifies the debugging process as the users can easily report which process is problematic, and starting a service is just a matter of running the correct executable.

# Tools

Restarting Nepomuk and looking into the database has traditionally required some dbus commands. These commands were always apparent to us developers, but it's good to have some standard ways of managing Nepomuk.

## NepomukCtl

Thanks to [Gabriel](http://g-poesia.blogspot.in/2013/02/hello-planet.html), we now have `nepomukctl` which acts very similar to the `akonadictl`. It can be easily used to start, stop and restart Nepomuk or any individual service.

{{< highlight bash >}}
$ nepomukctl start
$ nepomukctl restart
$ nepomukctl restart fileindexer
{{< / highlight >}}

This is a great tool to check if Nepomuk is actually running.

## Nepomuk Show

In order to view the data inside the Nepomuk database one typically needs to issue a query. This requires the developers to know SPARQL. Typically users do not want to go into so much effort when they are debugging simple stuff.

Now `nepomukshow` can be used to easily view the resource information.

{{< highlight trig >}}
$ nepomukshow Politik.mp3

<nepomuk:/res/94e816c8-2466-4172-9bcd-0b9129cd17f8>
  rdf:type            nmm:MusicPiece
  rdf:type            nfo:FileDataObject
  rdf:type            nfo:Audio
  rdf:type            nie:InformationElement
  nao:created         2013-03-21T18:06:54Z
  nao:lastModified    2013-03-21T18:06:57Z
  nie:url             file:///home/vishesh/Music/Coldplay/Politik.mp3
  nie:mimeType        audio/mpeg
  nie:title           Politik
  nie:lastModified    2009-08-20T11:58:04Z
  nie:contentCreated  2002-03-21T18:06:54Z
  nie:created         2011-11-21T15:32:52Z
  nfo:averageBitrate  1.2800000000e+05
  nfo:sampleRate      4.4100000000e+04
  nfo:fileSize        5088374
  nfo:fileName        Politik.mp3
  nfo:channels        2
  nfo:duration        318
  nmm:performer       nepomuk:/res/8ffe10cc-c375-485f-8f00-b1d5b211ae7f
  nmm:trackNumber     1
  nmm:genre           20
  nmm:musicAlbum      nepomuk:/res/22c13ad9-f234-4e60-910a-9159b47cd290
  kext:indexingLevel  2

{{< / highlight >}}

This is a great tool to use to see what all information has been indexed about a file or if a file has been indexed at all. It can even be used to check if an email has been indexed, though the syntax is a little different.

{{< highlight trig >}}

$ nepomukshow 'akonadi:?item=39618'

<nepomuk:/res/b8ef2a3f-9112-4dfb-9071-4b1ce7544b1b>
  rdf:type            aneo:AkonadiDataObject
  rdf:type            nmo:Email
  nao:created         2012-12-11T10:29:20Z
  nao:hasSymbol       internet-mail
  nie:isPartOf        <akonadi:?collection=44>
  nie:byteSize        3998
  nie:url             <akonadi:?item=39618>
  nmo:isRead          1
  nmo:to              nepomuk:/res/cbee003e-7a36-4dd3-8978-c71c7c91d359
  aneo:akonadiItemId  39618
  nmo:messageSubject  Re: proposals marked as to be accepted in Melange now
  nmo:sentDate        2011-04-18T16:45:59Z
  nmo:from            nepomuk:/res/b900fd2e-dacf-4395-bb72-6c9ed78f5b71
  nmo:messageId       <BANLkTikYWvzvGCM4-N0cOw0NdVdO8BmOpg@mail.gmail.com>

{{< / highlight >}}

## NepomukCmd

Most developers already know of `nepomukcmd` which was an alias on top of `sopranocmd`. It could be used to query Nepomuk and contained many more soprano specific features which are now no longer applicable.

We are now shipping our own `nepomukcmd` tool, which currently only supports sparql queries. It does however support a neat `--inference` option which can be used to selectively enable and disable inferencing.

These tools right now are in a very early, simple but working state and could use some polishing, and extra features in the future. Contributing to these would be a great way to get involved in Nepomuk. Message me for more details.
