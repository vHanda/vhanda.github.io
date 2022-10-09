---
date: "2010-11-26T07:42:03Z"
tags:
- kde
- backup
- nepomuk
title: Nepomuk Backup
layout: "../../layouts/BlogPost.astro"
---

Roughly 2 weeks ago, just before the feature freeze, the Nepomuk backupsync service was pushed into the trunk.

A long time ago I posted a lot of features about the ‘planned’ Nepomuk Backup service. I did land up implementing most of those features, but ultimately disabled some of them for a more stable backup experience. I thought it would be better to have a stable backup tool, without all the frills, compared to unstable tool which may potentially mess up your data.

So, without further ado I present Nepomuk Backup -
<p style="text-align: center;"><a href="/blog/images/2010/11/26/kcm.jpeg"><img class="aligncenter size-full wp-image-333" title="Nepomuk Backup KCM" src="/blog/images/2010/11/26/kcm.jpeg" alt="" width="560" height="404" /></a></p>
<p style="text-align: center;">Nepomuk now provides automated backups.</p>

<h3>The Nepomuk Backup GUI</h3>
<a href="/blog/images/2010/11/26/intro.jpg"><img class="size-full wp-image-342 alignleft" title="Backup-Options" src="/blog/images/2010/11/26/intro.jpg" alt="" width="389" height="303" /></a>

<a href="/blog/images/2010/11/26/available-backups.jpg"><img class="size-full wp-image-343 alignright" title="available-backups" src="/blog/images/2010/11/26/available-backups.jpg" alt="" width="350" height="273" /></a>


The user interface can be invoked by the ‘nepomukbackup’ command, or can be called from the Nepomuk KCM.
<h4>**DBus interface**</h4>
The Backup GUI is completely independent of the backupsync service, as they communicate with each other through DBus. The main advantage of using DBus is that backups can easily be integrated into your existing backup solution. The command to perform a backup is -

`qdbus org.kde.nepomuk.services.nepomukbackupsync /backupmanager backup &lt;url&gt;`
<div>If a blank URL is given, the backup is performed in the default location which would be `$KDEDIR/share/apps/nepomuk/backupsync/backup`.</div>
<div>The backup could similarly be restored using DBus, but I would recommend using the gui for restoration as it allows you to resolve conflicts.</div>
<h3>Index everything</h3>
Honestly, do it! It’s a lot easier for you and for us. Plus you’ll have a much better Nepomuk experience if you index everything. With Nepomuk Backup, the more stuff you have indexed, the less conflicts you can expect when restoring a backup.
<h3>Nepomuk Sync</h3>
This entire post I’ve been calling it the backupsync service, and yet I’ve only been talking about backups. That’s cause there is currently no user interface for performing syncs. The infrastructure is there ( and it works! ) In the coming weeks once I’ll start working on the user-interface and stuff. Maybe I can release it in extragear? Either way, I will try my best to get Nepomuk Sync into 4.7

*<a href="/blog/images/2010/11/26/authors.jpeg"><img class="aligncenter size-full wp-image-365" title="authors" src="/blog/images/2010/11/26/authors.jpeg" alt="" width="590" height="127" /></a>*

*Now let the full scale testing commence :)*
