---
title: "[Reverse Engineering] IOLI 0x04"
date: "2022-04-15T16:21:00.000Z"
description: "From Dustri's « Defeating ioli with radare2 »"
categories: [reverse engineering]
comments: true
image:
  feature: https://raw.githubusercontent.com/t-leclercq/portfolio/main/content/assets/3.png
  credit: z4nzi
---

### Let's dive in a bit quicker

Crackme0x04 is following the steps of it's predecessor, nothing new : ioli's a progression.
The `main` function takes the user's input into and passes it to a `check` function.

Here is what it looks like (in pseudo code):

```
#include <stdint.h>
 
int32_t check (char * s) {
    char * var_dh;
    uint32_t var_ch;
    uint32_t var_8h;
    int32_t var_4h;
    char * format;
    int32_t var_sp_8h;
    var_8h = 0;
    var_ch = 0;
    do {
        eax = s;
        eax = strlen (eax);
        if (var_ch >= eax) {
            goto label_0;
        }
        eax = var_ch;
        eax += s;
        eax = *(eax);
        var_dh = al;
        eax = &var_4h;
        eax = &var_dh;
        sscanf (eax, eax, 0x8048638);
        edx = var_4h;
        eax = &var_8h;
        *(eax) += edx;
        if (var_8h == 0xf) {
            printf ("Password OK!\n");
            exit (0);
        }
        eax = &var_ch;
        *(eax)++;
    } while (1);
label_0:
    printf ("Password Incorrect!\n");
    return eax;
}
```

Do i see a pattern ? Yup, clearly
The input is passed into a loop that adds each character to a collector `n` times. 
`n` being the total lenght of the input.
Then, with `var_8h == 0xf`, `check` is checking if the sum (inside the collector) is equal to `15`.

So in short, if i pass `123456` to the `check` function, here is what happens :

- 1 is added to a collector, so the collector is equal to 1
- 2 is added to a collector, so the collector is equal to 3
- 3 is added to a collector, so the collector is equal to 6
- 4 is added to a collector, so the collector is equal to 10
- 5 is added to a collector, so the collector is equal to 15
- 6 is added to a collector, so the collector is equal to 21

Then comparing 21 with 15 returns false, which prompts us a `Wrong password` message.

You got it, the answer can be whatever, when adding each numbers, is equal to 15 :

![0x04 answer](https://raw.githubusercontent.com/t-leclercq/portfolio/main/content/assets/0x04a.png)