---
pubDate: "2011-09-01T12:22:45Z"
tags:
- scripts
- youtube
- flash
- html5
title: HTML5 Videos
layout: "@layouts/BlogPost.astro"
---

Flash is annoying. Specially in Linux where it goes bat crazy at times and starts gobbling up your CPU. That's one of the reasons why I really think HTML 5 Video tag is the way forward.

YouTube has had an [HTML5 beta](http://www.youtube.com/html5) for quite some time. Unfortunately, I don't like viewing videos on the YouTube player. I like the feel of my favorite media player - VLC. The great thing about the Flash videos was that they *used* to be cached in `/tmp/Fl*`. And then, Adobe changed their Flash cache directory.

Fortunately, I found this script somewhere -

{{< highlight bash >}}
#!/bin/sh
args=("$@")

args=`echo $args | sed 's/[/]$//'`

pids=`eval pgrep -f flashplayer`
for pid in $pids; do
    lsoutput=$(lsof -p $pid | grep '/tmp/Flash[^ ]*')

    IFS=$'\n'
    for line in $lsoutput; do
        lsout1=`echo $line | awk '{print "/proc/" $2 "/fd/" $4}' | sed 's/[rwu]$//'`
        lsout2=`echo $line | awk '{print $9}' | awk -F '/' '{print $3}'`

        if [ -n "$args" ];then
            if [ -d $args ]; then
                echo "Copying $lsout2 to $args/"
                eval "cp $lsout1 $args/$lsout2.flv"
            else
                echo "The directory \"$args\" doesn't exist"
                break
            fi
        else
            echo "Copying $lsout2"
            eval "cp $lsout1 $lsout2.flv"
        fi
    done
done
{{< / highlight >}}

After switching to the HTML 5 Beta, I needed a new script.

{{< highlight bash >}}
#!/bin/sh
#
# A Script that runs all WebM files present in the FireFox cache with vlc
# media player.
#
# Author: Vishesh Handa <me@vhanda.in>

CACHEDIR="$HOME/.mozilla/firefox/*/Cache/"

files=`find $CACHEDIR -mtime -1 -size +1M -regex '[^_]*' \
       -exec file -F ' ' {} \; | grep WebM | awk '{ print $1}'`

for f in $files; do
    echo $f
    vlc $f &> /dev/null
done
{{< / highlight >}}

Have fun!
