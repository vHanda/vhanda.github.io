---
pubDate: "2010-01-14T04:05:29Z"
tags:
- linux
- emacs
title: Configuring Emacs
layout: "@layouts/BlogPost.astro"
---

I finally started to configure Emacs, specifically for C++ and Python. And let me tell you one thing - Emacs is NOT made for newbies or people who have used graphical user interfaces their entire lives. It will be a huge, and I mean huge, learning curve. Will it be worth it? Hell, I'm still waiting to find out. And, here is another thing - If you do not know how to type properly. Learn how to do so. Emacs key bindings are difficult enough without you being a two-finger typing who continuously looks at the keyboard. I mean it. Google some typing lessons, download some software, pray to the heavens or perform a ritualistic animal sacrifice. Whatever you do - Learn how to type properly.

My initial plan to get Emacs working as a Python IDE was to jump right in i.e. act like a script-kiddie. I don't really understand Emacs Lisp, so most of the code presented was copied blindly. Copying something without understanding does NOT go well with me. I have to understand what I'm copying, even if I don't understand it completely I should at least have a semi-concrete/vague idea of what it does. Unless you know Emacs Lisp, most of code seems like gibberish initially.

After an hour of mindless copying scripts and adding them to the *.emacs* file. I was on the verge of screaming and pulling my hair. Not only did it not make any sense, but it didn't really work. Code completion was a disaster, browsing files was a major pain in the ass, and basically everything was falling apart. Then, finally, after screaming "WHAT THE F\*CK IS THIS?" a couple of times. I switched off Emacs ( C-x C-c ) and went to make myself a snack.

Ah hour later, after I had calmed down, I decided to restart my efforts. The earlier approach obviously wasn't working, so I decided to go with a more systematic approach. I got a paper and pencil (Did I ever tell you about my fascination with mechanical pencils? I just don't like pens!) and wrote exactly what was bothering me, and started to get those things fixed.

**Appearance - ** Emacs22 looks horrible and the fonts are terrible! Call me shallow or whatever, but appearance matters to me. I'm not going look at some ugly half-assed interface created my people who inherently do not like graphical user interfaces. Changing the fonts seemed like  a daunting task, and a quick Google search revealed something called <a href="http://peadrop.com/blog/2007/01/06/pretty-emacs/">Pretty Emacs</a>, which I promptly installed. Emacs looks pretty good darn good now, and it uses the system fonts, which, did I mention are a pleasure to read? A couple of hours later I realized that I had Emacs22 installed while Emacs23 was available. I had no idea why I had chosen not to install it, so I promptly installed it, and to my surprise, this version of Emacs uses the system font. That makes this first point totally redundant.

**The retarded scroll bar on the left -** I know most Emacs users don't keep a scroll bar as they believe using a mouse is against their religion, but I like interfaces, and I do use the mouse! `(set-scroll-bar-mode 'right)` This should be included by default. As far as I can remember scroll bars have always been on the right.

**Line numbering** - All basic editors, including *gedit* and *kwrite*, provide line numbering. The first solution I found was `M-x line-number-mode`, but that just switched off the L sign at bottom panel. The Emacs wiki pages contained so much info, that I was again swarming in a never-ending mist of information. Questions like okay there is this *.el* file, but where do I put it? Do I have to or is it included by default? If I need to include it should I use the package manager or just download it?The two options I finally found were - `setnu_mode` &amp; `linum_mode`. The former was buggy, and awkward while the latter was exactly what I wanted. Now, the question came of how to enable this by default? I discovered that every mode has a global option which can be set in the .emacs file.

`(global-linum-mode t)`

**Copy Paste -** I'm familiar with Emacs's concept of yanking and killing text, along with the kill-ring-buffer, but it should integrate itself with the Operating System's copy-paste buffer. While typing my blog I used to write the entire blog post, save it, open it with kWrite and then copy it to the Wordpress new post Page.
`(setq x-select-enable-clipboard t)`

**Code browsing **- Using `C-x b` and `C-x C-b` for managing files is not my idea of convenience. When you have been using Graphical User interfaces for so long, you need to be able to see the directories. Fortunately Emacs Code browser came to my rescue. Installing it was a mere - `sudo aptitude install cedet ecb` Then came the hard part. Learning how to use it.

The normal `C-x o` only seem to work when you're on one of the auxiliary windows. Jumping from the editor to the auxiliary windows requires a lengthy `C-c . g &lt;e/d/h&gt;`.

**Package management or manual download -** This was another reason for my confusion. I just don't know which philosophy to follow. A mixture of both, maybe? I've finally settled on manual downloads as it's easier to keep track of all the plugins download. Plus, the repositories don't contain all the plugins.

I don't believe in customizing software from the start. I believe you should initially learn to use it using its default behavior and after you're comfortable with it then start customizing it. Emacs doesn't work that way. In order to get virtually anything to work. You need to customize it, and that involves editing a *.**emacs* file, which isn't provided by default, in case you're wondering. For simple customizations it isn't required that you know Emacs Lisp, but it's a lot easier if you do.

I had already installed certain Plugins using the Ubuntu repositories, and the repositories aren't exactly up to date. For example - The Emacs Code Browser package is at version 2.40, which was released in May 2009, as of today the repository provides ver 2.32. So, I removed all the installed packages, and decided to install them manually.

I decided to keep the install directory .emacs.d/ as it already existed, unpacked ecb, ya-snippet <a href="http://www.youtube.com/watch?v=76Ygeg9miao">(Check out the amazing video)</a>, and cedet. This required minor configuring of my .emacs file, but by then I was getting used to it.

**Code Completion** - Getting this to mark was what had initially cause me to go a little berserk, but this time I had already installed all the required packages or so I thought. The Emacs wiki page has an auto-completion section. I though auto completion was a synonym for code completion, and headed over there to install it. Unfortunately, that page is a little out of date, the commands are conflicting at times, and it didn't really work. A little googling revealed a newer version, which had none of the earlier hassles.  But, this wasn't what I was looking for. It doesn't parse the header files and provide appropriate suggestions. It merely looks at code in the current/all buffer and suggests symbols from there. Ugh!

Eventually, I read the entire CEDET and Semantic guide, and realized that Semantic provided code-completion. The `global-semantic-idle-completion-mode` is nice, and so it the `semantic-speedbar` option. Neither of them are parsing the standard header files (for C++) or the imports from Python, but they still do parse a lot of info. So, for now I'm content. I wonder if there is a way to link this up with auto-complete.

**Control and Alt key -** I know it's suggested that you swap the Caps Lock and control, and that is what I had initially done, but it started to get annoying. My left pinky was constantly alternating between the TAB, CAPS LOCK, and SHIFT key. Something had to be done! I finally configured Caps lock into Alt (or Meta), Alt into Control. This works perfectly, and my pinky isn't getting that strained. Yet. <a href="http://pastebin.com/f46a96843">Here</a> is the script I used.

**Undo and Redo -** The default key combination for undo is `C-x u` or `C-_`. Both of which are kinda cumbersome for an operation you perform quite frequently. Then, I accidentally discovered `C-/` also acts as undo. :-) Unfortunately there is no simple redo mechanism. The current redo mechanism is to undo the "undoes". This can be done using `C-x` followed by the undo command, which is `C-/` for me. Still, it's kinda confusing switching back and forth between redo and undo. A better option is to install the redo package, and map it to C-Shift-/. <a href="http://www.emacswiki.org/emacs/RedoMode">Here</a> is a guide.

This is all I've done for now. As I continue making changes to Emacs, I'll keep posting. Maybe this will help some lost newbie who feels as confused as I do. :-

Here is my *.**emacs* file -

{{< highlight lisp >}}
; Load Directory
(add-to-list 'load-path "~/.emacs.d/")
(add-to-list 'load-path "~/.emacs.d/auto-complete-1.0")

;ya-snippet
(add-to-list 'load-path "~/.emacs.d/plugins")
(require 'yasnippet-bundle)

; Clipboard
(setq x-select-enable-clipboard t)

;(require 'ido)
;(ido-mode t)`

; Appearance
(set-scroll-bar-mode 'right)
(if (fboundp 'tool-bar-mode) (tool-bar-mode -1))

; Auto complete
(require 'auto-complete)
(require 'auto-complete-config)
(global-auto-complete-mode t)`

; Line numbers
(require 'linum)
(global-linum-mode t)

; Cedet
(load-file "~/.emacs.d/cedet-1.0pre6/common/cedet.el")
(global-ede-mode 1)                ; Enable the Project management system
(semantic-load-enable-code-helpers); Enable prototype help, smart completion
(global-srecode-minor-mode 1)      ; Enable template insertion menu`

; Ecb
(add-to-list 'load-path "~/.emacs.d/ecb-2.40/")
(require 'ecb)`

; Misc
(delete-selection-mode t)    ; Delete selected text
(setq make-backup-files nil) ; stop creating those backup~ files
(setq auto-save-default nil) ; stop creating those #autosave# files

{{< / highlight >}}
