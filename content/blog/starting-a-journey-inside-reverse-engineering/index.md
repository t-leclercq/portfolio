---
title: "[Reverse Engineering] Starting my journey into RE"
date: "2021-12-09T03:10:00.000Z"
description: "From basic web oriented CTF to RE challenges..."
categories: [reverse engineering]
comments: true
image:
  feature: https://raw.githubusercontent.com/t-leclercq/portfolio/main/content/assets/ASM.png?token=GHSAT0AAAAAABQQAU33U6ECYBOHRCPL3Z4AYSZNKAQ
  credit: z4nzi
---

![Debug session in gdb](https://raw.githubusercontent.com/t-leclercq/portfolio/main/content/assets/ASM.png?token=GHSAT0AAAAAABQQAU33U6ECYBOHRCPL3Z4AYSZNKAQ)

## I tripped on RE, now i'm obsessed

I'll make it quick : Initially, i'm more of a web dev so i'm used to higher level languages. A part of my studies has been focused of networking as well. The first CTF i did to challenge my skills... i stumble over a reverse engineering challenge. The goal was to patch a linux binary using a debugger and gathering ASM skills was clearly recommended. **I WAS LOST**

Now that i spent some time reading about ASM and x86 instructions, fiddling with C, testing, writing C, comparing ASM instructions from a simple loop binary, then from a hashing function... I'm a *bit* more aware of what's going in on inside this world.

### What drives me to progress while being a web dev ?

Cybersecurity has so many fields that i **have** to work on in order to get the bigger picture of what's really representing a flaw inside a production environment, that i started with the field i'm the least familiar with : low level languages.

### The plan

I gathered some useful ressources and started my journey with the following, in order of appearance :


![](https://us-central1-progress-markdown.cloudfunctions.net/progress/100)
- **[Max Kersten's Binary Analysis Course](https://maxkersten.nl/binary-analysis-course/)**
- **[The Official Radare2 Book](https://book.rada.re/)**
- **[pixis's Memory Handling](https://beta.hackndo.com/memory-allocation/)**
- **[pixis's How do stacks operate](https://beta.hackndo.com/stack-introduction/)**
- **[pixis's Buffer Overflow Theory and Example](https://beta.hackndo.com/buffer-overflow/)**
- **[pixis's ret2libc workaround, BoF Security activated](https://beta.hackndo.com/retour-a-la-libc/)**

![](https://us-central1-progress-markdown.cloudfunctions.net/progress/60)
- **[Dustri's Defeating ioli with radare2](https://dustri.org/b/defeating-ioli-with-radare2.html)**
*lol too easy this is a writeup* : **no**, not looking at the answer when trying to understand it myself has become a strict leitmotiv. I cannot say i understood if i'm guided with a path that's not mine.

![](https://us-central1-progress-markdown.cloudfunctions.net/progress/2)
- **[rev-pwn-hw-Hardware-Trojan](https://github.com/pwn2winctf/challenges-2020/tree/master/rev-pwn-hw-Hardware-Trojan)**
*This one has been in my bookmarks for a while, but i will someday achieve this kind of ecxercice. Yes, it's a **goal**.*

### So what now, u happy ?

Ok, enough talking : i'll start posting my own ioli writeups to save my work online and start the habit to note while i learn.
This is my conclusion.

