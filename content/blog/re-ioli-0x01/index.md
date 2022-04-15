---
title: "[Reverse Engineering] IOLI 0x01"
date: "2022-02-02T03:10:00.000Z"
description: "From Dustri's « Defeating ioli with radare2 »"
categories: [reverse engineering]
comments: true
image:
  feature: https://raw.githubusercontent.com/t-leclercq/portfolio/main/content/assets/1.png
  credit: z4nzi
---

![Second one...](https://raw.githubusercontent.com/t-leclercq/portfolio/main/content/assets/1.png)

### A bit of recon...

1. This can't be as easy as the 0x00... but why not testing again a good ol' `strings` :

```
❯ strings crackme0x01
/lib/ld-linux.so.2
__gmon_start__
libc.so.6
printf
scanf
_IO_stdin_used
__libc_start_main
GLIBC_2.0
PTRh
IOLI Crackme Level 0x01
Password: 
Invalid Password!
Password OK :)
GCC: (GNU) 3.4.6 (Gentoo 3.4.6-r2, ssp-3.4.6-1.0, pie-8.7.10)
[and so on...]
```

No clue.

2. In order to verify if this is a *countermeasure* for a specific `strings` search (which is a big word for this situation), i choose to double check with [rabin2](https://book.rada.re/tools/rabin2/strings.html). In order to do so, i fire up radare2, analyse the binary with `aaa` and execute the `rabin2 -z crackme0x01` command :

```
[0x08048330]> rabin2 -z ./crackme0x01
[Strings]
nth paddr      vaddr      len size section type  string
―――――――――――――――――――――――――――――――――――――――――――――――――――――――
0   0x00000528 0x08048528 24  25   .rodata ascii IOLI Crackme Level 0x01\n
1   0x00000541 0x08048541 10  11   .rodata ascii Password: 
2   0x0000054f 0x0804854f 18  19   .rodata ascii Invalid Password!\n
3   0x00000562 0x08048562 15  16   .rodata ascii Password OK :)\n
```

No clue, again.

Note to self (and to you guys) : Have i lost time ? No, double-checking is the best way to break assumptions based on a logic that is, most of the time, yours. In malware analysis, reverse engineering is a field where analysts have to deal with obfuscation based on langage, logic or [tools used by those analysts](https://www.appsealing.com/code-obfuscation/).

3. In the above step, i analyzed the binary using `aaa` which provides a pretty minimal yet sufficient analyse of the program's insructions. Let's have a quick look to the functions list using `afl` :

```
[0x08048330]> afl
0x08048330    1 33           entry0
0x080482fc    1 6            sym.imp.__libc_start_main
0x08048380    6 47           sym.__do_global_dtors_aux
0x080483b0    4 50           sym.frame_dummy
0x080484e0    4 35           sym.__do_global_ctors_aux
0x080484d0    1 5            sym.__libc_csu_fini
0x08048504    1 26           sym._fini
0x08048460    4 99           sym.__libc_csu_init
0x080484d5    1 4            sym.__i686.get_pc_thunk.bx
0x080483e4    4 113          main
0x080482d4    1 23           sym._init
0x08048354    3 33           fcn.08048354
0x0804830c    1 6            sym.imp.scanf
0x0804831c    1 6            sym.imp.printf
```

We can see that there is the `entry` function, and the `main` function.
The difference between those two is that an entry function is where the program header is loaded, where the stack is loaded and, with it, the needed registries in order to help the program's instructions to run properly.

Then, the first user-written function to be called is the `main` function.

If a developer wants to compile a C program that prints a dummy `printf` program, this person will just need to write a main method that calls the printf function.

TL;DR : `entry0` is more of an automatic method that is set by the compiler, but the user-related content is supposed to start inside the `main` method.

3. I place myself at the beginning of `main` using `s main`. This changes the position cursor like so :

```
[0x08048330]> s main
[0x080483e4]>
```
*s stands for seek*

Because the programs actually starts at `entry0`, the 1st address **[0x08048330]** is the beginning of `entry0` (lol, no sh*t).
**[0x08048330]**  is, then, the starting address of the `main` method.

4. Using `pdf` (for **p**rint **d**isassemble **f**unction) prints the actual assembler code of this main function we're in :

5. The first comparison instruction is this one :
```
0x0804842b      817dfc9a1400.  cmp dword [var_4h], 0x149a
```

The hard-coded part is on the right : **0x149a**

6. To decode a value, i use the `?` command inside r2 :

```
[0x080483e4]> ? 0x149a
int32   5274
uint32  5274
hex     0x149a
octal   012232
unit    5.2K
segment 0000:149a
string  "\x9a\x14"
fvalue  5274.0
float   0.000000f
double  0.000000
binary  0b0001010010011010
ternary 0t21020100
```

7. Seeing that the `var_4h` value is an **int**, i deduced that this is our password. 

8. Let's test it :

```
[0x080483e4]> exit
❯ ./crackme0x01
IOLI Crackme Level 0x01
Password: 5274        
Password OK :)
```