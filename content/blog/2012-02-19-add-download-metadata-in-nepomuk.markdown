---
date: "2012-02-19T18:47:21Z"
tags:
- planetkde
- nepomuk
title: Add Download Metadata in Nepomuk
---

I land up using `wget` a lot. I know there are better alternatives, but
`wget`'s simplicity has won me over. Plus, with applications like
firefox, I'm not always sure I'll be able to continue the download.
That's important when I'm downloading big files, but for small files, it
really doesn't matter.

A couple of days back, Martin and I were chatting about storing the
metadata of a downloaded file in Nepomuk. I knew it wouldn't be hard.
The [ontology][] is already in place, so it was just a matter of pushing
the data into Nepomuk.

I estimated that it would take us around half an hour to code a simple
prototype. Yesterday we finally decided to do it.

It took around an hour. :)

So, we present to you -

Nepomuk Add Download Metadata (NADM)
====================================

Yes the name is weird. In fact deciding on the name was the hardest
part. The Nepomuk code was just around 30 lines -

![image][]

NADM is a simple executable which when presented with the file and
download url will attach the corresponding metadata. So, people who
prefer commands can just call it after `wget`, or write a script to do
it automatically.

For the people who use Firfox, they can head on to [Martin's blog][] and
look at the [cool stuff he implemented][].

**Source Code:** [kde:scratch/vhanda/nepomuk-add-download-metadata][]

  [ontology]: http://oscaf.sourceforge.net/ndo.html
  [image]: /blog/images/2012/02/19/nepomuk-download-code.jpg
  [Martin's blog]: http://martys.typepad.com/
  [cool stuff he implemented]: http://martys.typepad.com/blog/2012/02/so-you-want-to-keep-the-url-of-downloaded-file-eh.html
  [kde:scratch/vhanda/nepomuk-add-download-metadata]: http://quickgit.kde.org/index.php?p=scratch%2Fvhanda%2Fnepomuk-add-download-metadata.git&a=summary