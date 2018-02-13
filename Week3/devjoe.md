# Solution for [Week 3](./)
## Author: [Jaloopa](https://what.thedailywtf.com/user/Jaloopa)

<a name="Python2"></a>
## Language: Python 2.x

```
import sys
from decimal import *

coins=sys.stdin.readline().split()
target=Decimal(sys.stdin.readline())

chg={}
chg[Decimal('0')]=tuple()
todo=[Decimal('0')]

while target not in chg and target>min(todo):
  newdo=[]
  for amt in todo:
    for coin in coins:
      newamt=amt+Decimal(coin)
      if newamt not in chg:
        chg[newamt]=chg[amt]+(coin,)
        newdo.append(newamt)
  todo=newdo

if target not in chg:
  target=min((amtx for amtx in chg if amtx>target))
print " ".join(chg[target])
```

## Author Comments
There's some laziness here, but it's laziness that avoids a bunch of pitfalls in this problem. There are some pathological cases where this will break down, but most of them involve *way* more change than anybody is going to want to count out. (The problem happens when the dictionary gets more than about 11 million entries because of a limitation in Python. This either requires a solution which needs a number of coins which is large enough to generate 11 million different amounts in the coinage system, or, for amounts you can't make exactly, the largest number of coins needed to make any amount less than the target, + 1 coin, can make over 11 million different amounts.)

* Use the Python Decimal module to avoid all manner of rounding problems, and keep the original display strings rather than let str(Decimal(...)) stick on leading zeroes.
* Use a breadth-first search to find all the amounts we can make from n coins. Keep track of the amounts that required n coins, and keep increasing the number of coins until we either find our target, or there are no more amounts that need n coins less than our target.
* If we didn't find our target, change the target to the smallest amount we found greater than our target.
* Print the solution for the target.