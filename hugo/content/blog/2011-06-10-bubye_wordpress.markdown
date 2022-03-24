---
date: "2011-06-10T12:33:20Z"
tags:
- wordpress
- blog
- python
- blogofile
title: BuBye Wordpress
---

A couple of days ago, I finally jumped ship. I moved away from Wordpress. My blog looks a lot simpler now. I should probably state that I have shamelessly copied the CSS style from [Ant Zucaro](http://www.antzucaro.com/). I'll get around to modifying it to suit my needs.

My last attempt to move from Wordpress, was a spectacular failure. That migration is the reason there are no blog posts between July and November 2010. This happened because I chose [Jekyll](https://github.com/mojombo/jekyll/wiki), a ruby based static website generator. My ruby skills are next to non-existent. Trying to mess with code you can't read, along with being thrust into the world of Ruby with its own packaging mechanisms and what not, can confuse the crap out of some one. That coupled with my desire to write the css from scratch, ended in a monumental disaster.

![](/blog/images/2011/06/10/500px-Dr_Jekyll_and_Mr_Hyde_poster.png)

The language of my choice was Python, and I found quite a few contenders. [Hyde](http://ringce.com/hyde), which is the Python equivalent of Jekyll, was the first one that caught my eye. It's based on Django, which unfortunately still uses Python2. I really like Python 3. Eventually stumbled upon [Blogofile](http://blogofile.com/), which suited my needs perfectly.

It's extremely simple, and has most of the features anyone would want from a blog. It however did not offer different feeds for each tag. It took me some time, but I managed to hack my way and generate them. I still need to clean up the code, and push it upstream.

Now, hopefully, I'll start blogging more.
