---
date: "2012-01-21T15:33:12Z"
tags:
- planetkde
- gtalk
- nepomuk
- chat
title: Chat logs in Nepomuk
---

Prototyping is fun. You don't need to care about proper libraries. Your
code can be absolutely horrible, cause "Hey! It's just a prototype!"

Yesterday, I started the process of importing my entire gTalk chat
history into Nepomuk. It turned out to be a lot simpler that I thought
it would be.

Step 1: Get the chat logs
=========================

GMail fortunately allows you to export your chat logs via SMTP. They
don't implement the traditional [XMPP-0136][] for fetching offline
messages. But at least, unlike Facebook, they provide a mechanism.

I landed up using [getmail][] for importing all chat logs.

**getmailrc** :

    [retriever]
    type = SimpleIMAPSSLRetriever
    server = imap.gmail.com
    mailboxes = ("[Gmail]/Chats",)

    username = *****@gmail.com
    password = ********

    [destination]
    type = Maildir
    path = ~/Chats/

I originally wanted to use [offlineimap][] but they seem to have a
problem fetching the Chats in GMail.

![There were a lot of logs!][]

Step 2: Write a parser
======================

The chat logs are presented in a custom xml format encapsulated in the
email. The content was in the traditional [quoted-printable][] format,
as most emails are. Writing a parser didn't take too long. Plus, with
the new Nepomuk Datamanagement APIs, pushing them into Nepomuk was even
simpler.

![The API is fun to use!][]

Ideally, this should be implemented as a strigi analyzer, so that it
becomes a part of Nepomuk's Indexing framwork. But hey! It's a
prototype!

What's the point of having your chat logs in Nepomuk
====================================================

Well, for one, the Telepathians can use this to show chat logs. We'll
obviously need a better way of importing the chat logs. Manually calling
`nepomuk-chat-feeder` obviously isn't an option. So we'll need to find a
proper way of fetching chat logs.

The second, more personal, use is that I finally have a usable dataset
to determine important people in my life - based on the chat frequency
and timings. AFAIK Facebook internally uses a combination of likes,
comments, chat history and stalking to determine how important a person
is to you, and accordingly place them higher in the auto-completion list
and chat sidebar.

This obviously has many other applications like altering the chat list
based on the people you converse with when you're doing one activity.

**Source Code:** [kde:scratch/vhanda/nepomuk-gtalk-chatlogs][]

  [XMPP-0136]: http://xmpp.org/extensions/xep-0136.html
  [getmail]: http://pyropus.ca/software/getmail/
  [offlineimap]: https://github.com/nicolas33/offlineimap
  [There were a lot of logs!]: /blog/images/2012/01/21/chat-log-emails.jpeg
  [quoted-printable]: http://en.wikipedia.org/wiki/Quoted-printable
  [The API is fun to use!]: /blog/images/2012/01/21/pushing-chat-logs.jpeg
  [kde:scratch/vhanda/nepomuk-gtalk-chatlogs]: http://quickgit.kde.org/?p=scratch%2Fvhanda%2Fnepomuk-gtalk-chatlogs.git&a=summary