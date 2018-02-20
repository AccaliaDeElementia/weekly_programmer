# Weekly Programmer - Week 4

### From [Weekly Programmer Challenges](/weekly_programmer)

## Description

Write a program that acts as a four function calculator that receives its input in [Reverse Polish Notation](https://en.wikipedia.org/wiki/Reverse_Polish_notation)

## Formal Inputs & Outputs

### Input Description

Valid inputs consist of the following tokens, each separated from the next token by whitespace:

* A floating point decimal number
* the `+` character, inicating the addition operation
* the `-` character, indicating the subtraction operation
* the `*` character, indicating the multiplication operation
* the `/` character, indicating the division operation
* the `=` character, indicating end of input

For example:

```text
15 7 1 1 + − ÷ 3 × 2 1 1 + + − =
```

### Output description

The result of the calculation, or if the stack is malformed an error message indicating what token was found and what class of token was expected.

```text
5
```

## Challenge Inputs

```text
15 7 1 1 + − ÷ 3 × 2 1 1 + + − =
.333 .333 .333 + + =
.1 .1 + .1 + =
2 2 * 5 5 * =
+ =
1   1   1   1   1   1   1   1   1   1
+ + + + + + + + + =
```