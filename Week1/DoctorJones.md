# Solution for [Week 1](Challenge)
## Author: [DoctorJones](https://what.thedailywtf.com/user/DoctorJones)

<a name="CSharp"></a>
## Language: C#

```
void Main()
{        
    string.Join(", ", GetBaumSweet(0, 20)).Dump();
}

public IEnumerable<int> GetBaumSweet(int start, int end)
{
    return from i in Enumerable.Range(0, end + 1)
           let hasOddLengthOfZeroes = GetConsecutiveZeroes(i).Any(c => (c % 2) == 1)
           select hasOddLengthOfZeroes ? 0 : 1;
}

const int LEAST_SIGNIFICANT_BIT_MASK = 1;

IEnumerable<int> GetConsecutiveZeroes(int i)
{    
    while (i > 0)
    {
        int consecutiveZeroes = -1;
        int leastSignificantBit;

        do
        {
            // get the least significant bit            
            leastSignificantBit = i & LEAST_SIGNIFICANT_BIT_MASK;
            // shift to the next bit
            i = i >> 1;
            consecutiveZeroes++;
        } while (leastSignificantBit == 0);
        
        yield return consecutiveZeroes;
    }
}
```

## Challenge Output for n = 20:
```
1, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0
```