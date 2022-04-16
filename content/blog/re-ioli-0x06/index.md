---
title: "[Reverse Engineering] IOLI 0x06"
date: "2022-04-16T14:00:00.000Z"
description: "From Dustri's « Defeating ioli with radare2 »"
categories: [reverse engineering]
comments: true
image:
  feature: https://raw.githubusercontent.com/t-leclercq/portfolio/main/content/assets/06.png
  credit: z4nzi
---

### Déjà-vu ?

Based on operations inside 0x05 binary, we can recognize that we have :

`check` function is asking for the result to be **equal to 16**

`parell` function is asking for the result to be **even** and checks for the thirs argument of the `main` function

Here, i learned that `main` have two arguments : `arc` and `argv` as of C standards
Though, it can have a third argument named `envp` that reads the environement variables
A fourth one named `apple`, not as widely supported as the third, can be used on... well... Apple system. :)

I had some troubles understanding what was going on there because of ret2dec's had trouble interpreting envp argument of main function. I had to use r2ghidra's decompiler. This is probably due to a level two optimization of the compiler...

There, we see that the `dummy` function checks for a environment variable's name starting with `LOL` because `strncmp` gets the first 3 characters of the string `LOLO`

![the check for env](https://raw.githubusercontent.com/t-leclercq/portfolio/main/content/assets/0x06env.png)

Here is the function with `22222222` input (sums up to 16, so we're good) :

![two tries](https://raw.githubusercontent.com/t-leclercq/portfolio/main/content/assets/0x06a.png)
