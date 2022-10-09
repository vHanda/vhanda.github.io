---
date: "2009-12-14T04:03:52Z"
title: Animation Managing Techniques
layout: "@layouts/BlogPost.astro"
---

I've never really created a full fledged platform game. I've tried to, once, and I worked on it for about 6 months and I managed to create something resembling a game, if you count two characters running around many platforms with the ability to jump and bump into one another. Hehe, but then that was a long time back, and I think I've improved since then. At least I hope so!

Now that I'm entering the Assemblee competition, I'm giving platformers another chance, and this time I'm trying not to make my plans too grandiose. Lets see how it turns out. I have about a month to go. :-)

One problem I encountered last time was managing the animations. I don't mean basic Sprites, those are relatively easy, a bunch of images are shown consecutively to form an animation. I mean when should which animation be played, usually in accordance to the Player's, Agent's or Object's state.

I though there would be loads of information available on this topic, and a simple Google search would reveal many articles and maybe even a couple of technical papers. Unfortunately, that wasn't the case, and I only found one article. Or maybe I'm just searching for the wrong stuff :- Either way, this is a blog post about the different Animation Managing Technique I know about, and their pros and cons. I usually refer to animations as Sprites (frame based), but that doesn't necessarily mean so. I think skeletal animation would work perfectly well.

**1. Variable Based Approach ** - This is when you have a number of variables each depicting some area of the Object's state. It's somewhat like an ad-hoc state machine, having all its flaws and, more or less, none of its strengths. So, why would anyone use this? Beats me .. I did, but then that was because I didn't know any better.

![](/blog/images/2009/12/14/variable1.jpeg)

Look at the image for a better idea. When I had implemented this I didn't think of using vectors, and it was pretty much a huge fiasco. Most of the states mentioned would be taken care of by the physics engine / velocity vector, and some of them are extremely game centric.

The benefit of this method is that it is quite easy to implement, and seems quite intuitive. The problem is that since multiple variables can be active at one point of time, the code jumbles into a huge mess of checks and if-else statements. And the entire logic is lost in translation.

**2. State Based Approach -** This is the more refined way of using the "Variable Based Approach". The basic idea is that you have a Finite State Machine, where every state has a distinct animation. My states were generally implemented via classes, and have two independent rendering and updating (some people prefer to call it tick) functions. Each state maintains its own logic for changing states. Some people prefer to implements their FSMs using a combination of *enums *and *switch statements*. Do whatever works for you.

**Pros -**

* Easy to implement.
* Logic is clearly defined by each state.
* Separates the internal logic and animation beautifully.

**Cons -**

* The code gets somewhat lengthy.
* Not really that extensible and reusable.
* Add extra states required changing the previous states logic.

I usually implement every state with its own class, derived from a generic State class.

**3. Data driven Approach - ** This is my  chosen method of implementing my Animation Manager, and I think it's quite amazing. Instead of explaining how it works. I'm going to point you to the article I found regarding it -&gt; <a href="http://www.gamedev.net/reference/snippets/features/dataAnimMngr/">link</a>. I suggest you read the <a href="http://www.gamedev.net/community/forums/topic.asp?key=featart&amp;uid=2704&amp;forum_id=35&amp;Topic_Title=A+Data-driven+Animation+Manager">forum post</a> regarding its discussion as well.

The coding process was fairly simple and it took me just about an hour to implement the basic animation Engine. I still have to incorporate at least the basic optimizations.

**Pros -**
* State change logic is read from a file. So, it's totally re-usable.
* Code length is relatively small. Approx 200 lines of code including file handling.
* It's been used in commercial games. So, I know for a fact there are no great hidden dangers.

I don't really know any cons of this method, yet. Overall I think this approach is quite amazing, and I should mention it can be, apparently, used for skeletal animation systems.

Are they any other methods anyone uses for implementing Animation Mangers?
