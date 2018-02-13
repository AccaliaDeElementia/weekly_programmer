# Solution for [Week 3](README)
## Author: [masonwheeler](https://what.thedailywtf.com/user/masonwheeler)

<a name="Boo"></a>
## Language: Boo

```
namespace MakingChange

import System
import System.Linq.Enumerable

print "Input set of coins"
var coinsStr = gets()
print "Input change to make"
var changeStr = gets()
var coins =  coinsStr.Split((' ',), StringSplitOptions.RemoveEmptyEntries).Select(decimal.Parse).OrderByDescending({d | return d}).ToArray()
var change = decimal.Parse(changeStr)
while change > 0:
	var coin = coins.First({c | c <= change})
	print coin
	change -= coin

Console.ReadKey(true)
```