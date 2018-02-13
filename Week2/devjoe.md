# Solution for [Week 2](Challenge)
## Author: [Jaloopa](https://what.thedailywtf.com/user/Jaloopa)

<a name="Python2"></a>
## Language: Python 2.x

```
limit=10000000
plimit=int(limit**0.5)
sm=0
arr=[True]*limit
arr[0]=False
arr[1]=False
cur=1
while cur<plimit:
  cur+=1
  if arr[cur]:
    for j in xrange(cur*cur,limit,cur):
      arr[j]=False
print sum(j for j in xrange(limit) if arr[j])
```