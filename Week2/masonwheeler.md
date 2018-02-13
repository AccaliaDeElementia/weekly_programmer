# Solution for [Week 2](Challenge)
## Author: [masonwheeler](https://what.thedailywtf.com/user/masonwheeler)

<a name="Boo"></a>
## Language: Boo

```
namespace PrimeSum

import System
import System.Linq.Enumerable

def Sieve(length as int) as (bool):
	var result = Repeat(true, length).ToArray()
	result[0] = false
	result[1] = false
	k as int = Math.Floor(Math.Sqrt(length))
	for i in range(2, k):
		continue unless result[i]
		for j in range(2 * i, length, i):
			result[j] = false
	return result

var stopwatch = System.Diagnostics.Stopwatch()
stopwatch.Start()
var primes = Sieve(10000000)
var sum = primes.Select({value, i | (i if value else 0) cast long}).Sum()
stopwatch.Stop()
print sum
print "Generated in $(stopwatch.ElapsedMilliseconds) ms"
```

Challenge #1
```
namespace PrimeSumOptimized

import System
import Boo.Lang.Compiler.Ast
import Boo.Lang.Compiler.Steps

macro fastModule:
	var module = fastModule.GetAncestor[of Module]()
	AstAnnotations.MarkRawArrayIndexing(module)
	AstAnnotations.MarkUnchecked(module)


def CountingSieve(length as int) as long:
	WHEEL = (2, 4, 2, 4, 6, 2, 6, 4, 2, 4, 6, 6, 2, 6, 4,2, 6, 4, 6, 8, 4, 2, 4, 2, 4, 8,
             6, 4, 6, 2, 4, 6, 2, 6, 6, 4, 2, 4, 6, 2, 6, 4, 2, 4, 2, 10, 2, 10)
	result as long = 2 + 3 + 5 + 7
	var table = array(bool, length)
	table[0] = true
	table[1] = true
	k as int = Math.Floor(Math.Sqrt(length))
	var i = 11
	var wheelIndex = 0
	while i < k:
		if not table[i]:
			result += i
			for j in range(i * i, length, i):
				table[j] = true
		i += WHEEL[wheelIndex]
		++wheelIndex
		wheelIndex = 0 if wheelIndex == 48 //WHEEL.Length
	while i < length:
		result += i if not table[i]
		i += WHEEL[wheelIndex]
		++wheelIndex
		wheelIndex = 0 if wheelIndex == 48 //WHEEL.Length
	return result


fastModule
var stopwatch = System.Diagnostics.Stopwatch()
stopwatch.Start()
var result = CountingSieve(2145 * 1000 * 1000)
stopwatch.Stop()
print result
print "Generated in $(stopwatch.ElapsedMilliseconds) ms"
Console.ReadKey(true)
```

Challenge #2
```
namespace PrimeSumRemote

import System
import System.Linq.Enumerable
import System.Net
import HtmlAgilityPack

def GeneratePrimesUpTo(maximum as long) as long*:
	var stopwatch = System.Diagnostics.Stopwatch()
	stopwatch.Start()
	System.Net.ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12 | SecurityProtocolType.Tls11 | SecurityProtocolType.Tls
	var client = WebClient()
	var primesHtml = client.DownloadString('https://www2.cs.arizona.edu/icon/oddsends/primes.htm')
	var doc = HtmlDocument()
	doc.LoadHtml(primesHtml)
	var primeList = doc.DocumentNode.SelectSingleNode('//pre').InnerText
	var primes = primeList.Split((' ', '\r', '\n'), StringSplitOptions.RemoveEmptyEntries)
	print "Primes retrieved and parsed in $(stopwatch.ElapsedMilliseconds) ms"
	for primeStr in primes:
		var prime = long.Parse(primeStr)
		if prime <= maximum:
			yield prime
		else: break
	print "List built in $(stopwatch.ElapsedMilliseconds) ms"

MAX = 611953


print "Sum all primes up to what number? (Maximum of $MAX)"
var maximumStr = Console.ReadLine()
maximum as int
if not int.TryParse(maximumStr, maximum):
	print "'$maximumStr' is not a valid number."
elif maximum > MAX:
	print "$maximum is too large."
else:
	var result = GeneratePrimesUpTo(maximum).Sum()
	print "Sum of all primes up to $maximum is $result"
print "Press any key to continue . . . "
Console.ReadKey(true)
```