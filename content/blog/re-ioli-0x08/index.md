---
title: "[Reverse Engineering] IOLI 0x08"
date: "2022-04-16T23:56:00.000Z"
description: "From Dustri's « Defeating ioli with radare2 »"
categories: [reverse engineering]
comments: true
image:
  feature: https://raw.githubusercontent.com/t-leclercq/portfolio/main/content/assets/08.png
  credit: z4nzi
---

### Thanks to Radare2 Book ;)

After a few moments analysing, and testing the same response as `22222222`, i figured out that... there wasn't any differences between the 0x07 and 0x08 behaviours.

In short, i wasn't sure there was any difference... but then Radare2 book pointed out what's a stripped binary and non-stripped binary.

The difference was just that 0x08 is a non-stripped 0x07.

![the challenge in a nutshell](https://raw.githubusercontent.com/t-leclercq/portfolio/main/content/assets/08.png)