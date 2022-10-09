---
pubDate: "2010-02-16T00:28:51Z"
tags:
- kde
- linux
- bugs
title: Fixing a bug :KDE
layout: "@layouts/BlogPost.astro"
---

EDIT: I'd just like to say that I did NOT solve the bug, and I introduced another one in the process. But it was a good learning experience. :)

A couple of weeks back I started learning about Qt, and then about KDE. I thought it was time I start contributing, specially since I can. KDE consists of million lines of code, and getting acquainted with such a huge system is a terribly daunting task. Fortunately, they have pretty good documentation.

One of the best ways to acquaint yourself with the huge code base is to, allegedly, fix a bug. I've never really fix a bug before, so it seemed like a task which would need loads of experience, proper understanding of the code, and many other things. Turns out, it isn't that way at all. Bugs are of all types, some are simple some are sweet.

[This post](http://forum.kde.org/viewtopic.php?f=76&amp;t=16626&amp;start=0) is what got me started. Ksnapshot seemed like a simple enough program, and it is ... kinda. This blog post is about how I fixed [**this bug**](https://bugs.kde.org/show_bug.cgi?id=204628), and the thought process behind it.

**Getting the code -** This was fairly simple, and after reading the [SVN guide at KDE](http://techbase.kde.org/Getting_Started/Sources/Using_Subversion_with_KDE), I even understand the structure of the code base. I generally tend to use Bazaar to mange my code, and I've heard a lot about git. Svn is a step below, so it really wasn't a problem for me.

**Compiling the code -** The only small problem I encountered was when I  tried to build *ksnapshot* instead of *kdegraphics*. And on trying to compile *kdegraphics *I ran into loads of *N**epomuk* and *ontology* dependency problems. I fixed this by removing *okular* and *gwenview* and then running the entire *cmake *process.

**Setting up the Environment –** Many of the [KDE techbase articles](http://techbase.kde.org/Getting_Started/Sources/Using_Subversion_with_KDE) are about setting up the entire KDE code-base for development. This consists of either creating a new user (generally kde-devel) or using scripts. I preferred the new user approach, and download the entire source code. But in the end, all of that didn't really matter. It was just an inconvenience. So, I guess you should go with the recommended approach. I didn't really understand any of the scripts and they didn't do squat for me. For a small project like *ksnapshot* this wasn't really required.

Now, came the actual part – **Understanding the code.** The usual problem I have with reading source code is that I don't really know where to start. Every open source project is different and almost all of them are constructed differently. I don't really have much experience reading source code – Gimp, VLC, and a couple others. All of them were largely C programs. Qt and KDE just make love C++ more and more. :-) Anyway, the ideal place I found to start in KDE apps is main.cpp, and follow the included files from there. Fortunately, *ksnapshot* isn't that large just 14 files (C++ and headers) and despite what I thought earlier. You don't need to understand the entire source code to fix one bug.

I was using KDE's inbuilt text editor Kate for browsing the source code, with its Documents tab it was quite nifty, though it lacks stuff like *go to declaration* and *type of variable.* Nevertheless it was a simple solution. One that didn't require me to configure anything. Just jump in and start reading.

![kate](/blog/images/2010/02/16/kate1.jpeg)

I usually keep a pencil and some paper in front of me that way I can draw diagrams (loosely based on UML ) and jot down points to remember. Some editors have different mechanisms to add comments, but I somehow prefer a more traditional pencil and paper.

Before attempting to solve bugs in KDE, I went through some of the [tutorials](http://techbase.kde.org/Development/Tutorials) and some parts of the API <a href="http://api.kde.org/">(Here is the ultimate reference)</a> Look at <a href="https://bugs.kde.org/show_bug.cgi?id=204628">this page</a> for a walk through about what the bug was about. No point repeating it over here.

Now I'm just going to catalogue the information I gathered -
* **main.cpp** – Fairly standard. The only out of the norm thing (for me) was the 	inclusion of custom Command Line arguments, but I presume that is 	the norm.

* **ksnapshotobject – **Now, this was one odd class. It initially seemed like it should have been a *namespace* as none of the public function actually modify any of 	the variables. This is not quite apparent as they weren't declared *const*. Maybe they should have been static member functions? Overall 	this class contained 6 data members and 3 additions protected member 	functions. They were fairly easy to understand. `autoincFilename` has been a subject of bug reports earlier.

* **ksnapshot** – This class was derived from `ksnapshotobject` and 	`kdialog`. Multiple inheritance! I don't really have any experience with multiple inheritance and generally tend to avoid it (For no apparent reason!) But over here it seems to be fine. This is class is the heart of the application, and the largest.

* **Rest of the files** – Stuff like `regiongrabber`, `windowgrabber` and `snapshottimer` didn't really seem relevant to the bug I was trying 	to fix so, I really didn't them bother with them much.

Coming to the actual bug solving. After finding save functions in `ksnapshotobject` I decided to head over to them and understand what they do. (Keep the API documentation open!) The first two overloaded versions of `save` were fairly obvious and both seemed to call `saveEqual`. Here is where it got complicated. I didn't really know much about MIME types, apart from the basics, so when confronted by *<a href="http://api.kde.org/4.x-api/kdelibs-apidocs/kdecore/html/classKMimeType.html#a98fc76604e462c4209ff96270977116a">KmimeType::findByUrl</a> *even after reading the documentation I was fairly confused. `KMimeType` is derived from `KserviceType`, which is inherited by `KsyscocaEntry`, which made me further read a lot about the <a href="http://techbase.kde.org/Development/Architecture/KDE3/System_Configuration_Cache">System Configuration Cache</a>. None of this was really helping in solving the bug, though it did make me understand KDE
better, which I suppose was the point!

After that I decided the skip the specifics and get the general idea. The saveEqual function just saves the file with additional checks. So, saving wasn't really the problem. I headed over to open slots in *ksnapshot.cpp*. And viola! I had found the problem – Both the slots (the first one doesn't seem to be used anywhere – Don't take my word for it!) were opening a local file. This made me read a lot about KstandardDirs :-)  They should be opening a local file if the file wasn't already saved, otherwise they should open the saved file. Logically.

![orig code](/blog/images/2010/02/16/orig-code.jpeg)

Then came the actual coding process. Fixing the bug. My thought was process was something along the line that I should check whether the file has already been saved (bool variable maybe?) and then subsequently open it or a temp file. I really didn't want to introduce new variables as this wasn't my code. Another thing I noticed was that *fileOpen* (the main variable in consideration) was a *QString* and later on it was being ***implicitly*** typecasted into a KUrl. I generally tend to avoid implicit type conversion, and the code stuck struck me as some what wrong. I decided to change it to a KUrl (a lot safer) and add a check. As this change was required in two functions, in an attempt to avoid code duplication, I encapsulated it in a function.

![code change](/blog/images/2010/02/16/code-change.jpeg)

I tested it out. Fixed a couple of typos and I was done. :-) After than I proceeded to <a href="http://techbase.kde.org/Contribute/Send_Patches">create a patch</a>. I tend to subconsciously indent code as I'm reading it, so the patch I created (svn diff) had those changes as well. Not something I wanted. I landed up reverting the code to the original, and *just* making the changes to fix the bug. Still some code indentation got transferred to the patch. Anyone knows how to fix this?

After that I added the patch to the original bug report ([link](https://bugs.kde.org/show_bug.cgi?id=204628)) and crossed my fingers and waited. After a couple of days I got tired of waiting and landed up mailing the author. (Am I too impatient?) Hopefully, my contribution will be added to *ksnapshot*.

The entire process was quite enlightening, and was a great learning experience. I learned a lot about KDE and Qt. I think fixing bugs is an amazing way to get acquainted with a project. Specially one with such a huge code base. :-)
