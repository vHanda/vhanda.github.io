---
date: "2010-06-24T00:02:24Z"
tags:
- planetkde
- kde
- gsoc
- nepomuk
title: 'Nepomuk: Backup & Sync'
layout: "../../layouts/BlogPost.astro"
---

﻿How time flies.. a month of the official coding period is already over, and I'm nowhere even close to saying that this part works perfectly. Most of my code is still in the *requires exhaustive testing* stages, but I can say that the core parts have been implemented.

In case you've forgotten, or didn't read the title, I'm implementing Metadata Backup &amp; Sync. Well I'm supposed to be implementing sharing as well, but ignore that for now. The core parts are done and I've started working on the user interface. Creating interfaces is something I've never been good at. I guess it's more of an acquired trait. Very few people would be very good interface designers from the start.

The screenshot should say it all.

<img class="aligncenter" title="BackupSync screenshot" src="/blog/images/2010/06/24/backupsync.jpg" alt="Rudimentary Interface" width="326" height="302" />

Syncing and Backup both have a common base, and work on the same principle. While syncing maps the metadata with another machine's metadata, backup maps it to a file (Zip Archive). So, when I say the core has been implemented, I means all of the ground work has been done.

When designing backups, integrability was one of my prime concerns. I wanted people to be able to integrate metadata backup with their existing backup solutions with minimal effort. Providing *just* a graphical interface just wouldn't cut it. An interface which could be seamlessly integrated with other programs was required. The most obvious solution I could think of was DBus. Performing a backup is as simple as calling a method (with no arguments) from DBus.

Another factor that was of major concern was performance. Backing up metadata needs to be fast. If it isn't, backups will get side-tracked for more important tasks. And when a catastrophe does occur (and it will), you'll lose important data. Currently, backing up even slightly large databases ( with over 10000 statements, worth backing up ) takes less than a couple of seconds. And that's just for the first backup. Subsequent backups would only store the changes, and are nearly instantaneous.

Every time you perform a backup, a checkpoint is created. So if you ever want to revert your database to be previous state ( maybe you synced some data which you didn't want to ) you can revert the Nepomuk database to any of the checkpoints. Reverting changes is non-destructive and you can even revert a revert.

Syncing works on the concept of sync files. When synchronizing with another machine, that system must create a sync file and send it to your machine. The creation of sync files is the same as creating a backup and it's performance is nearly analogous. I haven't yet implemented a mechanism to transfer the sync files, because that isn't my prime concern. Maybe later, if I have time.

Restoring a backup or performing a sync isn't that fast. Mainly because apart from finding the required files, folder, contacts, and other stuff. It even needs to decide which statements should be removed and which should be added. This is done by analyzing the time stamps. This entire process may require a certain amount of user-interaction, mainly to help determine the location of certain files. One way to reduce the amount of user-interaction required is to index most of your data. It helps a LOT!

I'll try to keep posting updates as the project evolves. Till then if you have any suggestions about how I can better integrate backup or sync with any existing solution, please let me know.

BTW, I'll be attending Akademy 2010!

<img class="aligncenter" title="Attending Akademy" src="/blog/images/2010/06/24/igta2010.png" alt="Yes! I'll be there :)" width="380" height="200" />
