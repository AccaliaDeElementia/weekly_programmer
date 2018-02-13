# Solution for [Week 2](Challenge)
## Author: [Jaloopa](https://what.thedailywtf.com/user/Jaloopa)

<a name="CSharp"></a>
## Language: C#

```
            var timer = new System.Diagnostics.Stopwatch();
            timer.Start();
            var maxToCheck = 2000000000;
            var candidates = new bool[maxToCheck];
            long sum = 0;
            for (int i = 2; i < maxToCheck; i++)
            {
                if (!candidates[i])
                {
                    sum += i;
                    for (long j = i; j < maxToCheck; j += i)
                    {
                        candidates[j] = true;
                    }
                }
            }
            timer.Stop();
            Console.WriteLine($"Sum of the first {maxToCheck} primes is {sum}. Elapsed time: {timer.Elapsed.TotalSeconds} seconds");
```