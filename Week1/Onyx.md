# Solution for [Week 1](Challenge)
## Author: [Onyx](https://what.thedailywtf.com/user/Onyx)

<a name="QBASIC"></a>
## Language: QBASIC

```
DIM n AS INTEGER
REDIM bitArray(1)
INPUT n%
FOR i = 0 TO n
    CALL DecToBin(i, bitArray())
    PRINT CountOddGroups(bitArray());
NEXT

FUNCTION BitCount (num)
    IF num = 0 THEN
        BitCount = 1
    ELSE
        BitCount = INT(LOG(num) / LOG(2)) + 1
    END IF
END FUNCTION

SUB DecToBin (num, bitArray())
    IF num = 0 THEN
        bitArray(0) = 0
        EXIT SUB
    END IF
    DIM count AS INTEGER
    count = BitCount(num)
    REDIM bitArray(count)
    temp = num
    FOR c = 1 TO count
        bitArray(c) = temp MOD 2
        temp = temp \ 2
    NEXT
    EXIT SUB
END FUNCTION

FUNCTION CountOddGroups (bitArray())
    CountOddGroups = 1
    IF UBOUND(bitArray) = 1 AND bitArray(1) = 0 THEN EXIT FUNCTION

    DIM zeroCount AS INTEGER
    zeroCount = 0

    FOR o = 1 TO UBOUND(bitArray)
        IF bitArray(o) = 0 THEN
            zeroCount = zeroCount + 1
        ELSE
            IF zeroCount MOD 2 = 1 THEN
                CountOddGroups = 0
                EXIT FUNCTION
            ELSE
                zeroCount = 0
                CountOddGroups = 1
            END IF
        END IF
    NEXT
    EXIT FUNCTION
END FUNCTION
```

## Challenge Output for n = 20:
```
1, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0
```

<a name="Asterisk"></a>
## Language: Asterisk
```
exten => 1,1,Answer()
 same => n,Set(i=0)
 same => n,Set(zeroCount=0)
 same => n,Read(input,beep,8);
 same => n,While($[${i}<=${input}])
 same => n,Set(zeroCount=0)
 same => n,Set(currentNum=${i})
 same => n,While($[${currentNum} > 0])
 same => n,Set(zeroCount=${IF($[${currentNum} % 2 = 0] ? $[1 - ${zeroCount}] : ${zeroCount})})
 same => n,Set(currentNum=${IF($[${currentNum} % 2 = 1 && ${zeroCount} > 0] ? 0 : ${currentNum})})
 same => n,Set(currentNum=${MATH(${currentNum}/2,int)})
 same => n,EndWhile
 same => n,SayDigits($[1 - ${zeroCount}])
 same => n,Set(i=$[${i}+1])
 same => n,EndWhile
 same => n,Hangup()
```