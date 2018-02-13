# Weekly Programmer - Week 1

## Description
In mathematics, the Baum–Sweet sequence is an infinite automatic sequence of 0s and 1s defined by the rule:

* b_n = 1 if the binary representation of n contains no block of consecutive 0s of odd length;
* b_n = 0 otherwise;
for n >= 0.

For example, b_4 = 1 because the binary representation of 4 is 100, which only contains one block of consecutive 0s of length 2; whereas b_5 = 0 because the binary representation of 5 is 101, which contains a block of consecutive 0s of length 1. When n is 19611206, b_n is 0 because:

```
19611206 = 1001010110011111001000110 base 2
            00 0 0  00     00 000  0 runs of 0s
               ^ ^            ^^^    odd length sequence
```

Because we find an odd length sequence of 0s, b_n is 0.

## Challenge Description
Your challenge today is to write a program that generates the Baum-Sweet sequence from 0 to some number n. For example, given "20" your program would emit:

`1, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0`

## Challenge Solutions

* In APL
    * By [pie-flavor](pie-flavor)
* In Asterisk
    * By [Onyx](Onyx#Asterisk)
* In C++
    * By [Kian](Kian)
    * By [cvi](cvi)
    * By [MZH](MZH)
* In C#
    * By [Magis](Magus)
    * By [DoctorJones](DoctorJones)
    * By [Maciejasjmj](Maciejasjmj) using LINQ
* In DOS Batch
    * By [anotherusername](anotherusername#DOSBATCH)
* In Excel
    * By [anotherusername](anotherusername#Excel)
* In GW-BASIC
    * By [anotherusername](anotherusername#GW-BASIC)
* In Javascript
    * By [Accalia](Accalia)
    * By [anotherusername](anotherusername#Javascript)
* In Python 2.x
    * By [devjoe](devjoe)
* In QBASIC
    * By [Onyx](Onyx)
* In TCL
    * By [dkf](dkf)
* In TSQL
    * By [Vault_Dweller](Vault_Dweller)
* In X*^ Assembly
    * By [anotherusername](anotherusername#X86ASM)
