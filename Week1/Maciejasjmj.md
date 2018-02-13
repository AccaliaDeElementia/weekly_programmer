# Solution for [Week 1](Challenge)
## Author: [Maciejasjmj](https://what.thedailywtf.com/user/Maciejasjmj)

<a name="CSharp"></a>
## Language: C# (LINQ)

```
static void Main(string[] args) {
 const int n = 20;

 Enumerable.Range(0, n + 1)
  .Select(x => Enumerable.Range(0, sizeof(int) * 8 - 1)
   .Select(powerOfTwo => 1 << powerOfTwo)
   .Where(bitMask => bitMask <= x)
   .Select(bitMask => x & bitMask)
   .Select((maskingResult, index) => new {
    maskingResult,
    index
   })
  ).Select(bits => bits
   .Select(result => result.maskingResult > 0 || bits.Any(otherResult =>
     otherResult.index == result.index - 1 && otherResult.maskingResult == 0
    ) ? null : bits.Where(otherResult => otherResult.index >= result.index && otherResult.maskingResult == 0 && !bits.Any(yetOtherResult => yetOtherResult.index >= result.index && yetOtherResult.index < otherResult.index && yetOtherResult.maskingResult != 0))

   ).Any(re => re != null && re.Count() % 2 == 1) ? '0' : '1')
  .Select(ch => {
   Console.Write(ch + ",");
   return 0;
  })
  .ToList();
}
```

## Challenge Output for n = 20:
```
1, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0
```