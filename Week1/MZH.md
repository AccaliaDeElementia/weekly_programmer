# Solution for [Week 1](Challenge)
## Author: [MZH](https://what.thedailywtf.com/user/MZH)

<a name="Javascript"></a>
## Language: Javascript

```
#include <iostream>
#include <string>

int baum_sweet(unsigned n)
{
    if(n == 0)
    {
        return 1;
    }

    if(n & 1)
    {
        return baum_sweet(n >> 1); // ones at end don't affect answer
    }

    if(n & 2)
    {
        return 0; // n ends with 1 zero
    }

    return baum_sweet(n >> 2); // remove 2 zeros
}

int main(int argc, char** argv)
{
    if(argc > 1)
    {
        auto max_n = std::stoul(argv[1]);

        std::cout << baum_sweet(0);
        for(int i = 1; i <= max_n; ++i)
        {
            std::cout << ", " << baum_sweet(i);
        }
        std::cout << '\n';
    }

    return 0;
}
```

## Challenge Output for n = 20:
```
1, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0
```