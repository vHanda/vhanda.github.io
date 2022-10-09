---
pubDate: "2011-03-12T07:33:42Z"
tags:
- planetkde
- kde
- search
- nepomuk
title: Nepomuk Audio Search
layout: "@layouts/BlogPost.astro"
---

This is something I showcased at my talk in <a href="http://conf.kde.in/" target="_blank">conf.kde.in</a>. I coded it in a couple of hours. It is a lot of fun to play around with! I can’t wait to index all my videos this way.

 youtube d41bmTSogA4

Impressive? :D

This was showcased at the beginning of the Nepomuk talk under the section “Nepomuk is not about searching, but here are some cool things that it can do!”
<h2>How does this work?</h2>
Although this looks fairly impressive, it’s quite a simple procedure. Nepomuk don’t use any fancy audio heuristic or voice recognition. It simply searches through the subtitles and plays the video at the appropriate place.

This entire project was supposed to be used with the Nepomuk Web Extractor, which aims to allow Nepomuk get additional information from the internet to annotate your files/contacts/events. But I haven’t had the time to implement a web extractor plugin, so you need to manually provide the subtitles.
<h2>The Source Code</h2>
The code can be found at my <a href="http://quickgit.kde.org/?p=scratch/vhanda/nepomuk-subtitle-search.git&amp;a=summary" target="_blank">git scratch pad</a>. It is a slightly unstable prototype that was created just to illustrate subtitle searching in Nepomuk. In order to load the subtitles you have to run

`nepomuk-subtitle-loader pathToVideoFile pathToAudioFile`

and you can run the search GUI with – `nepomuk-subtitle-search`.

I’m going to try my best to get this ready in time for 4.7. Ideally, this should be somehow integrated in Dolphin, but that is going to be tough. It’s high time users start seeing Nepomuk do some really cool things!
