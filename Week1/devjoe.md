# Solution for [Week 1](Challenge)
## Author: [devjoe](https://what.thedailywtf.com/user/devjoe)

<a name="Python2"></a>
## Language: Python 2.x

```python
import sys

BS=[]
def bs(n):
  if n<2:
    return 1 #avoid making a special case for 0 by combining with the special case for 1
  if n%2==1:
    return BS[n/2] #integer division ftw
  final_zeroes=0
  while n%2==0: #for even numbers, count up the length of the final block of zeroes
    n/=2
    final_zeroes+=1
  if final_zeroes%2==1:
    return 0
  return BS[n]

j=int(sys.argv[1])
for n in xrange(j+1):
  BS.append(bs(n))
print ", ".join(map(str,BS))
```

## Challenge Output for n = 20:
```
1, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0
```