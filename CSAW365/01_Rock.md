# Rock
CSAW365 -> Reversing -> Rock
## Tools
Tool | Description
------- | -------
[IDA](https://www.hex-rays.com/products/ida/support/download_freeware.shtml) | IDA is the Interactive DisAssembler: the world's smartest and most feature-full disassembler, used by software security specialists worldwide.

## Description

We run the code and see the error.

![First run](https://github.com/AlessandroMorelli96/Writeups/blob/master/CSAW365/images/01_01.png)

We open the file with IDA and see the strings presents in the file.

![Strings](https://github.com/AlessandroMorelli96/Writeups/blob/master/CSAW365/images/01_02.png)

We go to see where the string: **Too short or too long** is used.

![Function](https://github.com/AlessandroMorelli96/Writeups/blob/master/CSAW365/images/01_03.png)

If We read the function we will see that we need to submit a string of 30 char.

![30 Char](https://github.com/AlessandroMorelli96/Writeups/blob/master/CSAW365/images/01_04.png)

As you can see ther are 2 for loop that are going to change every char of our string and compare it with a string: **FLAG23456912365453475897834567**

First it does a **xor 0x50** and **add 0x14**

![1 for](https://github.com/AlessandroMorelli96/Writeups/blob/master/CSAW365/images/01_05.png)

Then a **xor 0x10** and **add 0x09**

![2 for](https://github.com/AlessandroMorelli96/Writeups/blob/master/CSAW365/images/01_06.png)

Now we have to do the same operation but from the bottom to the start and reverse, so the **xor** remain the same, but **add** became **sub**.

This is my script in python that resolve the challenge

```
import sys

flag = "FLAG23456912365453475897834567"

for c in flag:
    tmp = ord(c)
    tmp = tmp - 0x09
    tmp = tmp ^ 0x10
    tmp = tmp - 0x14
    tmp = tmp ^ 0x50
    print(chr(tmp), end="")
```

## FLAG
***IoDJuvwxy\tuvyxwxvwzx{\z{vwxyz***
