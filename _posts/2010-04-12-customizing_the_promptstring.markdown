---
layout: post
title: Customizing The PromptString
date: '2010-04-12 23:50:37'
tags:
- bash
---

This is something I've been meaning to do for  some time, and yesterday while debugging Nepomuk (KDE) I finally decided to get it over and done with. Why do I want to change the default prompt string? Well, there are several reasons.

Firstly, to conserve the screen width. I don't log into other systems via SSH or anything, and hence the hostname is irrelevant to me. It was time to remove it. And secondly, to improve readability. With massive amounts of text gushing out. It gets a little difficult to read it all. Highlighting certain parts of it helps a lot.

In the end it was a simple - (this goes in the **.bashrc** file)
`
PS1='[$(tput bold)]u:w $ [$(tput sgr0)]'
export PS1`

The reason I've used bold instead of some colour is because I tend to get bored easily. I usually change the appearance of my terminal at least once in 2 weeks. Bold seemed like a safe-bet.

Reference - <a href="http://www.cyberciti.biz/faq/bash-shell-change-the-color-of-my-shell-prompt-under-linux-or-unix/#tb">Bash shell change</a>
