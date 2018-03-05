# Weekly Programmer - Week 7

*From [Weekly Programmer Challenges](/weekly_programmer)*

## Description

Write a program that calculates how long a lightbulb has been on.

Our lightbulb has two buttons, one that turns the light on, and one that turns the light off. Pressing the on button while the light is on has no effect, and pressing the off button while the light is off has no effect. Given a log of button presses for a day calculate how ling the light is on, and indicate the final state of the light at the end of the day.

## Formal Inputs & Outputs

### Input Description

Inputs will be multiline. Each line will staret with `0` or `1` where `0` indicates turning the light off and `1` turning it on. There will then be some amount of whitespace. Finally there will be a timestamp in HH:MM format.

Additional considerations:

* Lines that do not begin with a `0` or `1` followed by whitespace should be ignored.
* There may be text that follows the timestamp, it should be ignored.
* If encountered a line that consists entirely of hyphen `-` characters indicates end of input and any further input is to be ignored.

The timestamps are not necessarily in sorted order. Multiple conflicting button presses at the same time is an error condition. Inputs that do not include an entry for 00:00 to set the initial state of the light is an error condition.

Example input:

```text
# SAT 2018-03-03
1 06:45
1 09:30
1 19:00
0 00:00 This is the initial condition
0 09:15
0 17:10
0 22:20
--------------------------------------
Lorem ipsum dolor sit amet, consectetur adipiscing elit.
Nam lacinia vel eros eu fringilla.
1 22:30
Fusce porttitor laoreet aliquet.
```

### Output description

The amount of time the light was on, and the final state of the light at end of day.

Example output:

```text
The light was on for: 12:30
The light ended: Off
```

## Challenge Inputs

```text
1 06:45
1 09:30
1 19:00
0 00:00 This is the initial condition
0 09:15
0 17:10
0 22:20
```

```text
1 00:00
```

```text
0 00:00
1 09:00
1 09:00
0 10:00
```

```text
0 00:00
1 09:00
0 09:00
1 10:00
```

```text
0 00:00
1 09:00
1 09:10
1 09:30
1 09:55
0 10:00
0 10:01
0 10:10
0 10:50
```
