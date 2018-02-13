# Weekly Programmer - Week 3
### From [Weekly Programmer Challenges](/weekly_programmer)

## Description
You own a nice tiny mini-market that sells candies to children. You need to know what change to give back to those little cute creatures and it happens you don't know basic math because when you were a child you were always eating candies and did not study very well. So you need some help from a little tool that tell you if you can.

## Challenge Description
Write a program that calculates change and outputs the minimum amount of coins needed to produce the required change.

Your program should receive as input the values of coins available to use, and the value of change needed as a decimal amount of dollars. If the exact amount is not satisfiable your program should round up to the next highest satisfiable value and output the coins needed for that

Your program should output the coins required to give the correct change.

For example:
```
Input:
.5 .25 .1 .05 .01
1.24
Output:
.5 .5 .5 .1 .1 .01 .01 .01 .01
```

## Challenge Inputs

```
1 .5 .25 .1 .05 .01
4.99
```

```
1 .5 .25 .1 .05
4.89
```

```
1 .5 .25 .1 .05 .01
.66
```

```
5 2 .5 .25 .1 .05
16.01
```