---
date: "2013-01-30T11:36:55Z"
tags:
- planetkde
- nepomuk
title: Nepomuk Cleaner
layout: "@layouts/BlogPost.astro"
---

Nepomuk has a unique problem of maintaining an RDF store. Unlike
traditional SQL based stores, RDF offers a very loose schema, which is a
HUGE advantage. Unfortunately all of the current RDF stores do not
support any form of schema enforcement. It's up to the client code to
make sure that the data being pushed is valid.

This has resulted in a number of problems such as strings being stored
where an integer should go.

With the KDE Workspace 4.7 release, we started employing our own form of
schema enforcement in the Nepomuk Storage Service, but the old incorrect
data still remains. Also, as Nepomuk has evolved as a project, we have
found better ways to store data. Since the schemas are so loose, we
could easily store both the old and the new data without any problems on
the database level. This obviously results in more complex client code
which has to handle both legacy and new data.

For this release, we decided to clean up the code to a certain extent
and stop supporting some of the legacy data. We also decided to ship a
very basic application called the "**Nepomuk Cleaner**".

![image][]

This application is responsible to port any legacy data, clear up
incorrect data, and merge duplicate data. We recommend that all users
run it at least once. It will result in a performance upgrade of all
areas of Nepomuk, including a significant impact in the indexing speed
of emails.

With the 4.11 release, we're planning to improve the interface, add more
cleaning jobs, and make running this application mandatory. That way we
can safely remove all the legacy code paths.

  [image]: /blog/images/2013/01/30/nepomuk-cleaner.png
