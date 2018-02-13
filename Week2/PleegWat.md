# Solution for [Week 2](Challenge)
## Author: [PleegWat](https://what.thedailywtf.com/user/PleegWat)

<a name="Bash"></a>
## Language: Bash
```
$ time seq 2 10000000 | factor | awk 'BEGIN { sum=0 } NF == 2 { sum += $2 } END { print sum }'
```

<a name="C"></a>
## Language: C
```
/*
 * Perverted_Vixen's Sum-of-primes challenge
 * https://what.thedailywtf.com/topic/24558/weekly-programmer-challenges-by-professor-vixen
 */
#include <stdint.h>
#include <stdio.h>
#include <stdlib.h>

#define bitword(b)  ((b)>>7)
#define bitpos(b)   (((b)&0x7f)>>1)
#define bittest(b)  (sieve[bitword(b)] & (1UL << bitpos(b)))
#define bitset(b)   (sieve[bitword(b)] |= (1UL << bitpos(b)))

int main(int argc, char * argv[])
{
    uint64_t max = argv[1] ? atoll( argv[1] ) : (10 * 1000 * 1000);
    uint64_t * sieve = calloc( ((max >> 7) + 1), 8 );
    uint64_t i, j;
    uint64_t sum = 2;

    for( i = 3 ; i < max ; i += 2 )
    {
        if( ! bittest(i) )
        {
            sum += i;

            for( j = 3 * i ; j < max ; j += 2 * i )
            {
                bitset(j);
            }
        }
    }

    printf( "%lu\n", sum );
}
```