# Weekly Programmer - Week 4
### From [Weekly Programmer Challenges](/weekly_programmer)

## Description
Write a program that outputs the first recurring character in a string.

## Formal Inputs & Outputs
### Input Description
A string of alphabetical characters. Example:

```
ABCDEBC
```

### Output description
The first recurring character from the input. From the above example:
```
B
```

Alternatively provide additional info in the output, such as this example that providdes the solution to the basic problem and the bonus as well.
```
PXLJOUDJVZGQHLBHGXIW => `J`@3 (`X`[16])
```
This shows the first repeatching character is `J` at index 3 (zero based because that's the only correct way) and the character with the longest recurrence is `X` with a gap of 16 characters

## Challenge Inputs

```
the quick brown fox jumps over the lazy dog
IKEUNFUVFV
PXLJOUDJVZGQHLBHGXIW
*l1J?)yn%R[}9~1"=k7]9;0[$
156248267293222
0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz
```

## Bonus Problem
Instead of outputting the first recurring character output the character
with the largest gap between an occurrence and next occurrence.

## Challenge Solutions

* In Javascript
    * By [Perverted_Vixen](Perverted_Vixen)
