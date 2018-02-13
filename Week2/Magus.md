# Solution for [Week 2](Challenge)
## Author: [Magus](https://what.thedailywtf.com/user/magus)

<a name="CSharp"></a>
## Language: C#

```
static void Main(string[] args)
{
	var end = int.Parse(args.First());
	Console.WriteLine($"Sum of primes is {Sundaram2(end)}.");
}

private static BigInteger Sundaram2(int max)
{
	var end = max / 2;
	var set = Enumerable.Range(0, end);
	var primes =
		new[] { 2UL }
			.Concat(set.SelectMany(i =>
				Enumerable
					.Range(i, end)
					.Select(j => (ulong)i + (ulong)j + 2UL * (ulong)i * (ulong)j)))
			.Distinct();

	BigInteger sum = 0;
	for(int k = 0; k < max; k += 10000)
	{
		var segment = primes.Skip(k).Take(10000);
		sum += segment.Aggregate((a, b) => a + b);
	}
	return sum;
}
```