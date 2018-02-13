# Solution for [Week 1](Challenge)
## Author: [Magus](https://what.thedailywtf.com/user/magus)

<a name="CSharp"></a>
## Language: C#

```
static void Main(string[] args)
{
	var end = int.Parse(args.First());
	Console.WriteLine(string.Join(", ", Enumerable.Range(0, end + 1).Select(BaumSweet)));
}

private static int BaumSweet(int origin)
{
	if (origin == 0)
		return 1;

	var binary =
		Convert
			.ToString(origin, 2)
			.Select(character => byte.Parse($"{character}"));

	var key = 0;
	var contiguified =
		binary
			.Zip(binary.Skip(1).Concat(new[] { (byte)1 }), (left, right) => (Key: left != right ? key++ : key, Bit: left))
			.Where(segment => segment.Bit == 0)
			.GroupBy(segment => segment.Key)
			.Select(group => group.Count());

	return contiguified.Any(sequenceLength => sequenceLength % 2 > 0) ? 0 : 1;
}
```

## Challenge Output for n = 20:
```
1, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0
```