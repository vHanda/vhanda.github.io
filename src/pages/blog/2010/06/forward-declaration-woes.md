---
pubDate: "2010-06-07T04:42:04Z"
tags:
- c++
title: Forward Declaration Woes
layout: "@layouts/BlogPost.astro"
---

I'm ashamed to admit it, but I've never really understood forward declaration in C++. I know it's used to speed up compile times, and it helps me fix cyclic dependency issues, but I've never really understood it. To this date, my approach of forward declaration has been - "Forward declare most of the things, and then include header files to fix compiler warnings." I know that's not the best approach, and I should have read about it earlier, but ...

Here are the cases where you can't use forward declaration -
<ul>
	<li>**Base Classes - **When deriving a class Der, from a base class Base. Base cannot be forward declared</li>
	<li>**Member Variables -** If a class A is used  as a member variable then it can't be forward declared (References and pointers do not count)</li>
</ul>
I knew about the Base Class condition, but not about the other one. These conditions exist because the compiler must be able to find the total size of the class from the its definition. The compiler knows the size of pointers (and references.)

Another issue that had been bugging me was the use of forward declaration with templates. After reading and experimenting this is what I figured out.
<ul>
	<li>The syntax for forward declaring templates is NOT -- `template &lt;typename T&gt; className&lt;T&gt;;` It is -- `template &lt;typename T&gt; className;`</li>
	<li>Templates must be forward declared with all their parameters even if some of them are optional</li>
	<li>The template parameters follow the same forward declaration rules as another other member variables if the template is being used as a member variable. If the template variable is a pointer or a reference, you can safely forward declare its parameters.</li>
</ul>
A couple of other minor details -
<ul>
	<li>You can't forward declare typedefs or enumerations</li>
	<li>Don't ever try to forward declare any class from the *std namespace*. For one it's not allowed, and two, many commonly used classes, like std::string, are actually typedefs for other classes.</li>
</ul>
Maybe this blog post will help some poor bloke like me, who never bothered to fully understand forward declaration. :-/
