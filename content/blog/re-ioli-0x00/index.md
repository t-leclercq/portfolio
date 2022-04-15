---
title: "[Reverse Engineering] IOLI 0x00"
date: "2022-02-01T03:10:00.000Z"
description: "From Dustri's « Defeating ioli with radare2 »"
categories: [reverse engineering]
comments: true
image:
  feature: https://raw.githubusercontent.com/t-leclercq/portfolio/main/content/assets/0.png
  credit: z4nzi
---

### A bit of recon...
1. First of all, seeing that IOLI 0x00 is the starting point of a learning series of linux binaries, i use the `file` command to check whether i can or cannot execute it *as is*. 

2. `crackme0x00: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 2.6.9, not stripped` is what i see. Let's dive into detail in this kind of result :

	 - ELF 32-bit LSB executable is **Executable and Linkable Format** which is an executable file format developped by Unix System Laboratories (thanks Wikipedia...). This clearly means we're able to run this in our terminal.

2. Now i can execute the 0x00 binary with a simple bash shortcut in my terminal :

	`./crackme0x00`

2. This prompts me with two strings : 
	- `IOLI Crackme Level 0x00`  
	- `Password:` 
Followed by a pause which indicates that an input is awaited from the user

3. I type a random `likjsdhfkdfsgsljhsdf` to test the program's reply

4. The reply is, and i'm not surprised, `Invalid Password!`.

### Now we start searching for strings

We're supposed to find a password but we don't know the length, the format... just the sequence : 
- first the program's name,
- then, the password is asked
- finally, a reply is shown

1. Starting of with a simple `strings` review of hard-coded strings, we will see if the password is hard-coded too based on the order of appearance of the strings.

```
❯ strings crackme0x00
/lib/ld-linux.so.2
__gmon_start__
libc.so.6
printf
strcmp
scanf
_IO_stdin_used
__libc_start_main
GLIBC_2.0
PTRh
IOLI Crackme Level 0x00
Password: 
250382
Invalid Password!
Password OK :)
GCC: (GNU) 3.4.6 (Gentoo 3.4.6-r2, ssp-3.4.6-1.0, pie-8.7.10)
[and so on...]
```

We clearly see that some interesting string (or **int**) is effectively hard-coded inside this binary.

2. Testing this (assumed) password 

```
❯ ./crackme0x00
IOLI Crackme Level 0x00
Password: 250382
Password OK :)
```