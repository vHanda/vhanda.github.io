---
date: "2012-08-21T20:03:59Z"
tags:
- planetkde
- nepomuk
title: Faster Nepomuk Queries
---

Nepomuk has a very decentralized architecture where the different
components exist as different processes. They are all variants of the
same executable - `nepomukservicestub`. This servicestub loads
appropriate service plugin. The main reason for doing this was
stability. If one of the components crashes, then it doesn't take all
the other components with it.

Unfortunately this architecture doesn't hold very well when the
different components need to communicate with one another. In that case
they need to use complex methods such as dbus or local sockets. Another
problem is the increased memory consumption cause each process has its
own internal cache (Nepomuk stuff) and other KDE specific stuff.

![image][]

If you [ignore file handling][] in Nepomuk, we have two main services -

-   Storage Service
-   Query Service.

The Storage Service is responsible for managing the ontologies,
initializing virtuoso, and other data management functions. The
QueryService exists for caching queries and running them in a separate
thread.

Now the Query Service obviously need to access the virtuoso database,
and for that it needs to go through the storage service. This
communication happens through a local socket. The same socket which all
other applications use to access Nepomuk.

Last week, I finally merged the query service into the storage service.

![image][1]

I was aiming for a small memory decrease, and a slight performance
upgrade on the queries. Boy, was I wrong! The additional local socket
seems to have been a huge bottleneck.

Here are some benchmarks listing about 12,500 resources.

![image][2]

There are still many more performance upgrades that can be done, but
this seemed like a good place to start :)

  [image]: /blog/images/2012/08/21/query-storage-separate.png
  [ignore file handling]: http://www.vhanda.in/blog/2012/08/nepomuk-without-files/
  [1]: /blog/images/2012/08/21/query-storage-merged.png
  [2]: /blog/images/2012/08/21/queryservice-benchmarks.png