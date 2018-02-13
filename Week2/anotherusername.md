# Solution for [Week 1](Challenge)
## Author: [anotherusername](https://what.thedailywtf.com/user/anotherusername)

<a name="GWBASIC"></a>
## Language: GW-BASIC

```
10 DEFINT A-Z:INPUT"Max? ",MAX#:T!=TIMER
20 E=SQR(MAX#):N=E/LOG(E-4)*LOG(10):IF E<55 THEN N=28
30 DEF FNJ#(I#)=INT((I#-3)/2):DEF FNK#(I#)=2*I#+3
40 S=FNJ#(E):DIM P(N),Q(N),V(S):A=0:SUM#=0:IF MAX#>1 THEN SUM#=2
50 FOR I=0 TO S:IF V(I) THEN 80
60  P(A)=FNK#(I):SUM#=SUM#+P(A):Q(A)=FNJ#(3*P(A))
70  WHILE Q(A)<=S:V(Q(A))=1:Q(A)=Q(A)+P(A):WEND:Q(A)=Q(A)-S-1:A=A+1
80 NEXT:START#=0:A=A-1:ERASE V
90 M#=INT(FRE(0)/200-1)*100:IF M#<16000 THEN S=M# ELSE S=16000
100 START#=START#+I:IF FNK#(START#)>MAX# THEN 170
110 Z=S:IF FNK#(Z+START#)>MAX# THEN Z=FNJ#(MAX#)-START#
120 LOCATE,1:PRINT FNK#(START#)"-"FNK#(Z+START#);
130 DIM V(Z):FOR I=0 TO A
140  WHILE Q(I)<=Z:V(Q(I))=1:Q(I)=Q(I)+P(I):WEND:Q(I)=Q(I)-Z-1
150 NEXT:FOR I=0 TO Z:IF V(I)=0 THEN T#=FNK#(START#+I):SUM#=SUM#+T#
160 NEXT:ERASE V:GOTO 90
170 LOCATE,1:PRINT SUM#,,TAB(0)"Elapsed time:"(TIMER-T!)"sec"
```