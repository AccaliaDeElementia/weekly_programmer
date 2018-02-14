# Solution for [Week 1](Challenge)
## Author: [anotherusername](https://what.thedailywtf.com/user/anotherusername)

<a name="Javascript"></a>
## Language: Javascript

```javascript
function BaumSweet(n) {
  for (var a = [], i = 0; i <= n; ++ i) {
    for (var j = i; j && j % 4 == 0; j /= 4);
    a[i] = j ? j % 2 ? a[(j - 1) / 2] : 0 : 1;
  }
  console.log(a.join(', '));
  return a;
}
```

## Challenge Output for n = 20:
```
1, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0
```

<a name="QW-BASIC"></a>
## Language: GW-BASIC

```basic
10 INPUT"n";N
20 DIM A(N)
30 FOR I=0 TO N:J=I
40  WHILE J AND J MOD 4=0:J=J/4:WEND
50  IF J>0 THEN IF J MOD 2>0 THEN A(I)=A((J-1)\2) ELSE A(I)=0 ELSE A(I)=1
60  IF I=0 THEN PRINT USING"#";A(I); ELSE PRINT",";STR$(A(I));
70 NEXT I
```

## Challenge Output for n = 20:
```
1, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0
```

<a name="X86ASM"></a>
## Language: X86 Assembly

```asm
    XOR   BX,BX
    MOV   AH,02
    MOV   DL,6E              ;'n'
    INT   21
    MOV   DL,3F              ;'?'
    INT   21
    MOV   DL,20              ;' '
    INT   21
;_INPUT_LOOP
    MOV   AH,08
    INT   21                 ;Read character without echo
    CMP   AL,0D              ;Enter signifies end of input
    JZ    0136               ;_INPUT_DONE
    CMP   AL,30              ;'0'
    JB    0110               ;_INPUT_LOOP
    CMP   AL,39              ;'9'
    JA    0110               ;_INPUT_LOOP
    MOV   AH,02
    MOV   DL,AL
    INT   21                 ;Print accepted character
    MOV   CX,BX
    SHL   CX,1               ;Fast multiply by 10 (a << 1 + a << 3)
   ;SHL   BX,3               ;(unrecognized by debug - it's stupid)
    DB    C1,E3,03
    ADD   BX,CX
    AND   AX,000F
    ADD   BX,AX              ;Add typed char to accumulator in BX
    JMP   0110               ;_INPUT_LOOP
;_INPUT_DONE
    CMP   DL,20              ;If DL still contains ' ', nothing was entered
    JZ    0110               ;_INPUT_LOOP
    MOV   AH,02
    MOV   DL,0D              ;'\r'
    INT   21
    MOV   DL,0A              ;'\n'
    INT   21
    XOR   CX,CX              ;CX will count up from 0 to BX
    JMP   0153               ;_FIRST
;_MAIN_LOOP
    MOV   AH,02
    MOV   DL,2C              ;','
    INT   21
    MOV   DL,20              ;' '
    INT   21
;_FIRST
    MOV   AX,CX              ;AX contains the working value
    XOR   DL,DL              ;DL contains the result
;_SHIFT_LOOP
    CMP   AX,0
    JZ    016A               ;_DONE
    SHR   AX,1               ;Shift least-significant bit from AX into CF
    JB    0165               ;_BIT_1
    XOR   DL,01              ;DL = 1 if odd number of 0 bits have been scanned
    JMP   0157               ;_SHIFT_LOOP
;_BIT_1
    CMP   DL,0               ;DL = 0 if even number of 0 bits were scanned
    JZ    0157               ;_SHIFT_LOOP
;_DONE
    MOV   AH,02
    XOR   DL,31              ;'0' or '1'
    INT   21
    INC   CX                 ;Increment loop counter CX
    CMP   CX,BX              ;Check against upper loop limit
    JLE   0149               ;_MAIN_LOOP
    MOV   DL,0D              ;'\r'
    INT   21
    MOV   DL,0A              ;'\n'
    INT   21
    MOV   AX,4C00
    INT   21                 ;Terminate program and return to DOS
```

## Challenge Output for n = 20:
```
1, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0
```

<a name="DOSBATCH"></a>
## Language: DOS Batch

```batch
@echo off

set /p "max=n? "
for /f "tokens=* delims=0" %%a in ("%max%") do set "max=%%a"

set "out= "
set /a "n=0"
:main_loop
  set /a "a=%n%"
  
  :loop
  if /i %a% equ 0 goto :done
  
  set /a "m=%a% %% 4"
  if /i %m% equ 0 (set /a "a=%a% / 4" & goto :loop)
  
  set /a "m=%a% %% 2"
  if /i %m% equ 1 (set /a "a=(%a% - 1) / 2" & goto :loop)
  
  :done
  if /i %a% equ 0 (set "out=%out%, 1") else (set "out=%out%, 0")
set /a "n=%n% + 1"
if /i %n% leq %max% goto :main_loop

set "out=%out:~3%"
echo %out%
```

## Challenge Output for n = 20:
```
1, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0
```

<a name="Excel"></a>
## Language: Excel

```
"n? ",20,,"=IF(ISBLANK(OFFSET(A2,0,B1)),""Error - Extend the formula in cell A2 at least ""&B1&"" columns to the right (""&ADDRESS(2,B1+1)&"")"","""")"
"=IF(COLUMN()-1>$B$1,"""",IF(COLUMN()<3,""1"",MID(CHOOSE(VALUE(LEFT(OFFSET($A$2,0,INT((COLUMN()-1)/4)*2),1))*2+VALUE(LEFT(OFFSET($A$2,0,INT((COLUMN()-1)/4)*2+1),1))+1,""0000"",""1001"",""0100"",""1101""),MOD(COLUMN()-1,4)+1,1))&IF(COLUMN()-1<$B$1,"", "",""""))"
```