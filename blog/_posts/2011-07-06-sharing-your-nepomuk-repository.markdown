---
layout: post
title: Sharing your Nepomuk Repository
date: '2011-07-06 18:18:10'
tags:
- planetkde
---

How many of you know that **Nepomuk** is an abbreviation? Oh! You knew that? Well, you have 5 seconds, can you tell me its full form? I doubt it. Unless your fingers are really fast, and you managed to open the [Wikipedia article](http://en.wikipedia.org/wiki/NEPOMUK_%28framework%29) of Nepomuk in less than 5 seconds.

![Don't we have a pretty logo?](/blog/images/2011/07/06/nepomuk.png)

The 'N' in Nepomuk stands for Networked. And that is exactly what I've been working on over the last week. So far it works quite well over a local Network.

## Metadata Sharing? ##

One of the most requested feature in Nepomuk is the ability to share the data present. Specially when you're dealing with real world data like Projects, Events, and People.

One of the most obvious use cases that I can think of is sharing of tags - Not only file tags, but maybe even photo tags. This should be possible the moment we export the tags from digikam into Nepomuk properly.

Right now, I'm able to query for entire Desktop from my laptop. I never realized that I have so many songs and movies.

## Example Code ##

If you've ever tried to query the Nepomuk Repository, you generally use the QueryServiceClient. I've tried to replicate a similar API.

{% highlight c++ %}
// Construct a simple query
Nepomuk::Query::LiteralTerm lt(QLatin1String("Maroon 5"));
Nepomuk::Query::Query q( lt );

// Query all the available repositories
foreach( const QUrl& repositoryUri, repoList ) {
    Nepomuk::NetworkQueryServiceClient *nqsc =
           new Nepomuk::NetworkQueryServiceClient( repositoryUri, this );

    connect( nqsc, SIGNAL(newEntries(QList<Nepomuk::Query::Result>)),
             this, SLOT(newEntries(QList<Nepomuk::Query::Result>)) );
    connect( nqsc, SIGNAL(resultCount(int)),
             this, SLOT(resultCount(int)) );

    nqsc->query( q );
}
{% endhighlight %}

## Current Architecture ##

Nepomuk Sharing is still in its very early stages. The [ontolgies](http://sourceforge.net/apps/trac/oscaf/ticket/100) aren't even finalized. So, all of what I tell, is susceptible to change. And probably will change.

I've currently only implemented sharing over a local network, so that is all I'm going to be talking about.

The process is broadly divided into 3 parts:

* Repository Identification
* Finding other Repositories
* Communicating with them

#### Repository Identification ####

We need some unique way of identifying each repository, for that we use a GUUID. Each Nepomuk Repository would contain a resource of type nso:Repository, which contains its UUID.

{% highlight trig %}

<nepomuk:/res/dbc5a9c6-050f-4d1e-b720-cd8b43c23b69>
        rdf:type                 nso:Repository ;
        nso:repositoryIdentifier {3ef85996-a231-4183-8ed0-140b067793d0} ;
        nso:belongsTo            nepomuk:/myself .

{% endhighlight %}

A Repository can belong to a certain person. So, you can query someone's laptop/phone/sever, or just specify that person, and let Nepomuk query all the available devices.

#### Finding other Repositories ####

I chose the simplest mechanism for finding existing repositories over a local Network - DNS Service Discovery. If you already know about it, then skip the next paragraph.

[ZeroConf](http://en.wikipedia.org/wiki/Zero_configuration_networking) is a set of techniques that provide 3 core technologies - Link Local addressing, multicast DNS ( hostname resolution without a DNS server ), and service discovery through DNS.

![The Avahi logo](/blog/images/2011/07/06/200px-Avahi-logo.svg.png)

Service Discovery or DNS-SD allows us to browse what all services are available over a network. Each machine broadcasts ( actually multicasts ) the services that it provides using simple DNS records. Avahi, which is a free implementation of the ZeroConf protocol, provides DNS-SD. The Nepomuk Metadata sharing service advertises a [DNS SRV](http://en.wikipedia.org/wiki/SRV_record) type **`_nepomuk._tcp`**. This is done using [KDE's DNSSD library](http://api.kde.org/4.x-api/kdelibs-apidocs/dnssd/html/index.html).

#### Communicating ####

For communicating with other repositories, I've implemented a simple HTTP server, which acts a slight variant of a [SPARQL endpoint](http://www.w3.org/TR/rdf-sparql-protocol/). Conventionally Sparql endpoints respond to requests of the form:

`GET /sparql/query=EncodedQuery HTTP/1.1`

In Nepomuk we encourage the use the Nepomuk Query API, which allows us to optimize the queries internally, and create them programmatically. The Nepomuk endpoint accepts requests of the form

`GET /nepomuk/query=EncodedNepomukQuery HTTP/1.1`

The query results are returned in a standard [application/sparql-results+xml](http://www.w3.org/TR/rdf-sparql-XMLres/) format.

## Trying it out ##

### Git repository ###

The code is available at [kde:/scratch/vhanda/nepomuk-metadata-sharing](http://quickgit.kde.org/?p=scratch%2Fvhanda%2Fnepomuk-metadata-sharing.git&a=summary). As of posting this the HEAD is at **`b98205a0600908fe0e8dba49ec8fb9e78edeef5b`**. You might want to use that version, as I give no guarantee that I won't completely change everything. This is still totally experimental.

You'll need to use the "nsoRepository" branch from the Shared Desktop Ontologies.

### Running ###

The standard `cmakekde` should do the trick.

In order to run the code you will need Avahi running, along with mDNS, so that you can resolve local addresses like `vdek.local`. Run, the service via

`nepomukservicestub nepomukmetadatasharing`

on each machine whose repository you want to share. You can run queries with the test app -

**`nepomuk-sharing-test`** `'hasTag:Fire AND hasTag:Water'`

or use the NetworkQueryServiceClient.

## The Future ##

Over the next couple of weeks, I'm hoping to implement some privacy controls, and allow the queries to be sent over XMPP via Telepathy.


