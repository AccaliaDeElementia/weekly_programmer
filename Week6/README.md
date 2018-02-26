# Weekly Programmer - Week 6

*From [Weekly Programmer Challenges](/weekly_programmer)*

## Description

Write a program that packs boxes as efficiently as possible.

Each box has a maximum weight it can safely pack, so you cannot exceed that weight in a box, but each box also costs money to ship so you want each shipment to consist of the fewest boxes possible.

## Formal Inputs & Outputs

### Input Description

The first line of input will be the sizes of boxes available to pack and ship. There is an unlimited quantity of boxes of each size available.

For Easy Mode: Use only the largest available box size given.
For Hard Mode: Consider all box sizes given and use the ones that give the smallest number of boxes, and that minimizes wasted space in boxes

Subsequent lines of input are the items to be packed. Each line will consist of a number representing the weight of the item in the same units as the box sizes and a human readable lable describing the item to be packed. These two parts are separated from each other by whitespace. Your program need not consider the item name as part of its calculations.

Example input:

```text
190 300 250
70 A Game of Thrones
76 A Clash of Kings
99 A Storm of Swords
75 A Feasts for Crows
105 A Dance With Dragons
```

### Output description

The boxes required to ship the order.

Easy Mode:

```text
2
```

Hard Mode

```text
190:   1
250:   1
300:   0
Total: 2
Wasted Weight: 15
```

## Challenge Inputs

```text
190 300 250
70 A Game of Thrones
76 A Clash of Kings
99 A Storm of Swords
75 A Feasts for Crows
105 A Dance With Dragons
```

```text
500
1309 Artamene
303 A la recherche du temps perdu
399 Mission Earth
```

```text
500 250 100 1000 50
96 b400786
69 b390773
174 b410413
189 b337528
80 b308576
194 b151872
190 b174310
157 b272731
45 b326576
112 b379689
177 b18459
122 b915759
138 b967342
96 b786519
184 b718074
75 b696975
192 b46366
168 b533904
45 b885475
186 b872991
63 b231207
162 b912709
123 b786720
7 b743805
120 b862301
54 b929784
89 b61876
168 b775890
87 b850242
60 b695331
0 b56157
139 b875241
78 b281324
122 b236962
1 b79403
68 b213353
103 b650997
97 b955752
177 b815100
139 b958679
43 b829736
163 b445471
94 b472821
167 b5429
57 b946679
13 b748794
146 b920913
17 b547056
33 b437091
12 b247401
120 b228908
178 b509018
98 b482352
152 b915322
14 b874214
71 b164605
11 b457140
35 b502201
5 b15232
49 b641136
166 b385360
183 b78285
199 b274935
195 b424221
79 b422570
150 b502699
41 b662132
63 b430898
111 b813368
100 b700970
157 b803925
140 b611243
25 b877197
136 b577201
94 b50211
56 b762270
120 b578094
21 b672002
9 b107630
156 b547721
186 b911854
71 b594375
32 b330202
3 b464002
36 b718293
44 b282975
130 b826246
77 b529800
117 b66381
89 b949447
133 b348326
178 b517646
184 b809038
105 b70260
182 b894577
123 b203409
79 b174217
159 b552286
40 b854638
78 b159990
139 b743008
1 b714402
153 b923819
107 b201001
48 b567066
138 b570537
100 b64396
139 b412215
132 b805036
121 b772401
120 b370907
51 b388905
77 b442295
152 b195720
46 b453542
```
