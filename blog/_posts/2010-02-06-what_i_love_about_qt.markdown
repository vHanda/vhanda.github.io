---
layout: post
title: What I love about Qt
date: '2010-02-06 18:53:08'
tags:
- kde
- boost
- qt
---

For the past week I've been learning about the <a href="http://en.wikipedia.org/wiki/Qt_(framework)">Qt Framework</a>, mainly cause I want to start contributing to KDE, instead of just bitching about what doesn't work, or how awesome it is! :-P

In case you're unaware, Qt is an application framework for developing cross-platform GUI applications. TrollTech started it in 1991, and Nokia bought it in June 2008. Qt applications can even be ported to Nokia's Symbian platform for mobiles. <a href="http://kde.org/">KDE</a>, started in 1996, has always been based on Qt. Here are some things I just love about Qt -

* **C++ 	- **It's based on C++, not C, 	C++. I love C++ in comparison to other languages. It has its 	flaws, but I prefer it over others for doing large tasks. Otherwise 	there is always Python, which again Qt supports.

* **Signals 	and Slots Mechanism –** This 	is one of the things that totally differentiates Qt, from the other 	frameworks, 
namely GTK+. Typically in other GUI frameworks 	implemented in C++, some form of callbacks are implemented. Which 	aren't always type safe, and have difficult to use, cluttered 	interfaces, but Qt's mechanism works wonders. The negative point 	about it is that it isn't pure C++. Qt implemented their own *Meta 	Object Compiler* which runs 	before the compiler, and implements the Signals &amp; Slots 	mechanism among other things. *<a href="http://www.boost.org/doc/libs/1_42_0/doc/html/signals.html">Honorary Mention - Boost Signals and Slots.</a>*

* **Objects 	– **One thing I like about 	Java, which isn't present in C++, is a supreme base class from which 	all other classes are derived. Qt implements this in C++ via <a href="http://doc.trolltech.com/4.6/qobject.html">QObject</a>, and adds a 	further tree hierarchy to all its objects. This helps in memory 	management (read below), and allows easy type conversions.

* **Memory 	Management – **One of the main 	
things people dislike in C++ (I don't - <a href="http://en.wikipedia.org/wiki/RAII">RAII</a>) is the need for memory 	management.  Some frameworks circumvent this by implementing 	their own memory management scheme, generally by overloading 	*operator new *and 	*operator delete. *Qt's 	method is even better. It does nothing, but deletes all the children 	of every object destroyed. Therefore the root QObject is generally 	allocated from the stack.

* **Java 	Based iterators – **STL 	is awesome. It really is, but it lacks in usability in certain ways. 	Namely deleting objects from a container. As iterators have no 	knowledge about the underlying implementation, they can't know how 	to delete an element. ( Checkout - <a href="http://www.cplusplus.com/reference/algorithm/remove/">std::remove</a> ) Java iterators however can. The 	best part is that they have both C++ and Java style iterators, which 	gives some features of Java, while still harnessing the powers of 	
the Standard Algorithms.

* **Copy-on-Write 	Mechanism – **The 	Qt programmers took <a href="http://doc.troll.no/4.6/implicit-sharing.html">the lazy approach</a> while copying object (which is 	awesome!) Most of the classes in the Qt Core module are only deep 	copied when they are being modified, which means I can easily pass 	them as arguments and use them without any worries about an 	additional overhead. Even their own container classes are 	implemented that way!

* **QString 	– **16-bit 	string support – Unicode. I know C++ provides this via *wchar *and 	*wstring*, but its ugly and isn't that well used. Apart from the 16-bit 	strings, it even provides many of the needed string searching 	functions - endsWith, beginsWith, and Yes! Regular Expressions as 	well - <a href="http://doc.troll.no/4.6/qregexp.html">QRegExp</a>. Now I should mention that most of this, if not all, is provided by <a href="http://www.boost.org/">boost</a>.
 
* **Everything 	C++ Should have had – **Another 	thing I like about Java and Python, which is missing in C++, is its 	huge library. There is DateTime, RegExp,  Networking, GUIs, URL 	handling, multi-threading, file-system monitoring, image handling, 	and many other things. All provided by Qt. Again there *is 	boost*, 	which provides everything stated and much more.

* **Container 	Classes – **I 	know this is a minor thing, but I love the overloaded operator &lt;&lt;. 	It's a lot simpler than calling *push_back.*</li> * **Foreach – **Support for the 	“foreach” keyword. Something C++0x should fix, but I 	have no idea when that's going to come out. Maybe in 2011?

I'm still exploring Qt, so there is a lot I don't know. But so far I'm really impressed. It really integrates everything together. I can totally see why KDE chose Qt.

Next week, after KDE 4.4 is released. I'll start going through the KDE code. :-)