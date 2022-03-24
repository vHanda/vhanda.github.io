---
date: "2014-10-13T16:37:16Z"
tags:
- planetkde
title: KRunner and Plasma 5
---

Let's talk us about KRunner and what has changed over the course of Plasma 5.

## KDE 4 and KRunner

During the KDE 4 series, KRunner was an important part of the KDE Workspace, and it was tied very closely to Plasma. The KRunner library was in fact a part of the Plasma library.

The entire UI was based on [QGraphicsScene](http://qt-project.org/doc/qt-4.8/qgraphicsscene.html). Considering that we were moving to QML for Plasma 5, the UI needed a complete rewrite.

## Milou

A little while ago, I started a small project called [Milou](http://vhanda.in/blog/2014/03/introducing-milou/). The idea was to create a UI more refined for searching. During that time kdelibs and therefore Plasma were frozen. Milou needed some extra features which krunner did not provide. Therefore, we implemented our own backend code separate from KRunner. It was a far far simpler implementation than KRunner and didn't even have a proper plugin framework. The idea was to cater specifically to Milou's needs.

## Plasma 5

With Plasma 5, the KRunner UI had to be ported to QML. Given that Milou was already based on QML, and a port to Qt 5 was in progress, it seemed prudent to use its UI instead of making one from the scratch.

Also, we were at a point where kdelibs had been split up into framworks and was no longer frozen.

KRunner was split up into 2 parts - [The KRunner Framework](http://inqlude.org/libraries/krunner.html), and KRunner the executable, which typically ran with Alt + F2. This executable is still part of the Plasma workspace.

![](/blog/images/2014/10/13/plasma_krunner.jpg)

### So what about Milou?

KRunner and Milou were effectively merged together. The features that Milou required were added to the KRunner Framework, and Milou's backend code was dropped.

With Plasma 5, we ship both KRunner and a Plasmoid called "Search".

![](/blog/images/2014/10/13/plasma_milou.jpg)

This plasmoid internally uses the same code as KRunner. The only missing part is the previews that Milou provided. They still need to be properly ported to Qt5. We will probably have them for Plasma 5.3. 

## Sprinter

One of the many reasons for splitting the KRunner code out from the plasma framework was that it would make it easier for the KRunner Framework to be replaced, if required.

During that time, [Sprinter](https://projects.kde.org/projects/playground/libs/sprinter) did seem like a possible alternative. My own views on this are different, but in general, it could have been considered an alternative to the KRunner Framework.

There were discussions and the conclusion was that for the Plasma 5.0 release Sprinter will not be adopted as it would involve a significant amount of work for very little or no advantage.

Since then Sprinter has not had [much development](https://projects.kde.org/projects/playground/libs/sprinter/repository). Given that the current KRunner Framework suits our needs, there are currently no plans to change.

None of this is, of course, set in stone.
