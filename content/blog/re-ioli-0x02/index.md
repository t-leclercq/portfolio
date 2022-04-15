---
title: "[Reverse Engineering] IOLI 0x02"
date: "2021-12-09T03:10:00.000Z"
description: "From Dustri's « Defeating ioli with radare2 »"
categories: [reverse engineering]
comments: true
image:
  feature: https://raw.githubusercontent.com/t-leclercq/portfolio/main/content/assets/2.png?token=GHSAT0AAAAAABQQAU33ABD3JP6OZPUGEBFUYSZOUPQ
  credit: z4nzi
---

![Second one...](https://raw.githubusercontent.com/t-leclercq/portfolio/main/content/assets/2.png?token=GHSAT0AAAAAABQQAU33ABD3JP6OZPUGEBFUYSZOUPQ)

### Started from the main (now we here)

After and `aaa` analyze, and `afl` to print all functions... nothing new, there's only a main function and basic print functions.

```
[0x08048330]> afl
0x08048330    1 33           entry0
0x080482fc    1 6            sym.imp.__libc_start_main
0x08048380    6 47           sym.__do_global_dtors_aux
0x080483b0    4 50           sym.frame_dummy
0x08048500    4 35           sym.__do_global_ctors_aux
0x080484f0    1 5            sym.__libc_csu_fini
0x08048524    1 26           sym._fini
0x08048480    4 99           sym.__libc_csu_init
0x080484f5    1 4            sym.__i686.get_pc_thunk.bx
0x080483e4    4 144          main
0x080482d4    1 23           sym._init
0x08048354    3 33           fcn.08048354
0x0804830c    1 6            sym.imp.scanf
0x0804831c    1 6            sym.imp.printf
```

1. Let's dive in with `s main` and `pdf` :

```
[0x080483e4]> pdf
            ; DATA XREF from entry0 @ 0x8048347
┌ 144: int main (int argc, char **argv, char **envp);
│           ; var uint32_t var_ch @ ebp-0xc
│           ; var signed int var_8h @ ebp-0x8
│           ; var int32_t var_4h @ ebp-0x4
│           ; var int32_t var_sp_4h @ esp+0x4
│           0x080483e4      55             push ebp
│           0x080483e5      89e5           mov ebp, esp
│           0x080483e7      83ec18         sub esp, 0x18
│           0x080483ea      83e4f0         and esp, 0xfffffff0
│           0x080483ed      b800000000     mov eax, 0
│           0x080483f2      83c00f         add eax, 0xf                ; 15
│           0x080483f5      83c00f         add eax, 0xf                ; 15
│           0x080483f8      c1e804         shr eax, 4
│           0x080483fb      c1e004         shl eax, 4
│           0x080483fe      29c4           sub esp, eax
│           0x08048400      c70424488504.  mov dword [esp], str.IOLI_Crackme_Level_0x02_n ; str.IOLI_Crackme_Level_0x02_n
│                                                                      ; [0x8048548:4]=0x494c4f49 ; "IOLI Crackme Level 0x02\n" ; const char *format
│           0x08048407      e810ffffff     call sym.imp.printf         ; int printf(const char *format)
│           0x0804840c      c70424618504.  mov dword [esp], str.Password:_ ; [0x8048561:4]=0x73736150 ; "Password: " ; const char *format
│           0x08048413      e804ffffff     call sym.imp.printf         ; int printf(const char *format)
│           0x08048418      8d45fc         lea eax, [var_4h]
│           0x0804841b      89442404       mov dword [var_sp_4h], eax
│           0x0804841f      c704246c8504.  mov dword [esp], 0x804856c  ; [0x804856c:4]=0x50006425 ; const char *format
│           0x08048426      e8e1feffff     call sym.imp.scanf          ; int scanf(const char *format)
│           0x0804842b      c745f85a0000.  mov dword [var_8h], 0x5a    ; 'Z' ; 90
│           0x08048432      c745f4ec0100.  mov dword [var_ch], 0x1ec   ; 492
│           0x08048439      8b55f4         mov edx, dword [var_ch]
│           0x0804843c      8d45f8         lea eax, [var_8h]
│           0x0804843f      0110           add dword [eax], edx
│           0x08048441      8b45f8         mov eax, dword [var_8h]
│           0x08048444      0faf45f8       imul eax, dword [var_8h]
│           0x08048448      8945f4         mov dword [var_ch], eax
│           0x0804844b      8b45fc         mov eax, dword [var_4h]
│           0x0804844e      3b45f4         cmp eax, dword [var_ch]
│       ┌─< 0x08048451      750e           jne 0x8048461
```

We see 3 interesting things right above :

- user input gets stored in **var_4h**
- there are 2 hard-coded values : **90** inside **var_8h** and **492** inside **var_ch**
- there are algorithmic instructions such as `add` and `imul` this time

2. Let's follow the assembly starting from `[0x0804842b]` :

```
0x08048418      lea eax, [var_4h]	    ; input gets stored in var_4h
0x0804841b      mov dword [var_sp_4h], eax  ;
0x0804841f      mov dword [esp], 0x804856c  ; 
0x08048426      call sym.imp.scanf          ; 
0x0804842b      mov dword [var_8h], 0x5a    ; 90 is stored in var_8h
0x08048432      mov dword [var_ch], 0x1ec   ; 492 is stored in var_ch
0x08048439      mov edx, dword [var_ch]     ; var_ch value is displaced in edx
0x0804843c      lea eax, [var_8h]	    ; value of var_8h is loaded inside eax
0x0804843f      add dword [eax], edx        ; edx is added to the value of eax, so eax is 492 + 92 so eax = 582
0x08048441      mov eax, dword [var_8h]     ; the value of var_8h is moved inside eax
0x08048444      imul eax, dword [var_8h]    ; eax is multiplied with the value of var_8h (there are the same at this point) so 582²
0x08048448      mov dword [var_ch], eax     ; assigning eax content to var_ch value, var_ch = 582×582 = 338724
0x0804844b      mov eax, dword [var_4h]     ; assigning var_4h value to eax
0x0804844e      cmp eax, dword [var_ch]     ; comparing eax (user input) to var_ch (338724)
0x08048451      jne 0x8048461               ; if not equal, badguy scenario
```

3. Sure ? Let's give it a shot : 

```
❯ ./crackme0x02
IOLI Crackme Level 0x02
Password: 338724
Password OK :)
```