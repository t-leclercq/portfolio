---
title: "[Reverse Engineering] IOLI 0x05"
date: "2022-04-15T17:08:00.000Z"
description: "From Dustri's « Defeating ioli with radare2 »"
categories: [reverse engineering]
comments: true
image:
  feature: https://raw.githubusercontent.com/t-leclercq/portfolio/main/content/assets/5.png
  credit: z4nzi
---

### Déjà-vu ?

Crackme0x05 is crackme0x04 with 2 variations :
`check` function is asking for the result to be **equal to 16 instead of 15**

The `check` function also checks for parity with the `parell` function :

![the parell function](https://raw.githubusercontent.com/t-leclercq/portfolio/main/content/assets/0x05.png)

I learned here that `var &= 1` equals to `var = var & 1`

[This ressource](http://graphics.stanford.edu/~seander/bithacks.html) is perfect for a deep dive into these operations.


Here is the function with `22222222` input (sums up to 16, so we're good) :

![the parell function](https://raw.githubusercontent.com/t-leclercq/portfolio/main/content/assets/0x05a.png)
