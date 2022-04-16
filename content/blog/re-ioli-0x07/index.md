---
title: "[Reverse Engineering] IOLI 0x06"
date: "2022-04-16T15:08:00.000Z"
description: "From Dustri's « Defeating ioli with radare2 »"
categories: [reverse engineering]
comments: true
image:
  feature: https://raw.githubusercontent.com/t-leclercq/portfolio/main/content/assets/07.png
  credit: z4nzi
---

### Is this a joke ? (kinda..)

![the challenge in a nutshell](https://raw.githubusercontent.com/t-leclercq/portfolio/main/content/assets/0x06env.png)

No need to say that this was... easy ?

**BUT**, i learned a lesson : this challenge was the same as the previous, with an important exception.

There was a hidden `wtf` string inside the binary, which was never used.

I think that the intent was to say that in a malacious binary, some strings can be used to cause turmoil inside the analyst's head. Seeing a `wtf` line made me wonder a few things, instead of working on the actual binary.

Meh, the same old `22222222` with our environment variable worked. :D
