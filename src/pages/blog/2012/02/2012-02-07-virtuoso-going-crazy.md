---
date: "2012-02-07T16:29:55Z"
tags:
- planetkde
- nepomuk
- virtuoso
title: Virtuoso going crazy?
layout: "@layouts/BlogPost.astro"
---

There have been cases of virtuoso going a little crazy and consuming a
lot of CPU cycles. It's extremely frustrating. However, it's ever more
annoying when you have no idea what's wrong.

Most of bug reports we get just say that virtuoso is consuming too much
CPU, and that isn't the least bit helpful. So, here is a short guide to
figure out what query is causing virtuoso to go crazy.

Listing Queries
===============

Nepomuk contains a query service which is used to cache queries and to
execute them asynchronously. We can use it at any point to figure out
which all queries are being executed.

    $ qdbus org.kde.nepomuk.services.nepomukqueryservice
    /
    /nepomukqueryservice
    /nepomukqueryservice/query1
    /nepomukqueryservice/query4
    /servicecontrol

Each of the `/nepomukqueryservice/query[n]` represents one query.

Getting the SPARQL Query
========================

    $ qdbus org.kde.nepomuk.services.nepomukqueryservice
    /nepomukqueryservice/query4 queryString

And you'll get something like this -

    select distinct ?r ?v2 where { { ?r a
    <http://www.semanticdesktop.org/ontologies/2007/11/01/pimo#Note> . ?r
    <http://www.semanticdesktop.org/ontologies/2007/08/15/nao#created> ?v2 . }
    . ?r <http://www.semanticdesktop.org/ontologies/2007/08/15/nao#userVisible>
    ?v1 . FILTER(?v1>0) . } ORDER BY DESC ( ?v2 )

This query is extrememly important cause without it finding the cause is
nearly impossible.

Killing queries
===============

    $ qdbus org.kde.nepomuk.services.nepomukqueryservice /nepomukqueryservice/query4 close

This will end the query

When/If you find virtuoso consuming too much cpu, list out all the
queries and close each of them one by one. The moment virtuoso gets
better, you'll have your culprit.

That's the query you should post in the bug report.
