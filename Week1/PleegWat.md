# Solution for [Week 1](Index)
## Author: [PleegWat](https://what.thedailywtf.com/user/PleegWat)

<a name="C"></a>
## Language: C
```
#include <stdio.h>
#include <stdlib.h>

struct bits {
    struct bits * next;
    int ones;
    int zeroes;
};

struct bits * increment(struct bits * number)
{
    struct bits * next;

    if( ! number )
    {
        number = malloc( sizeof( *number ) );
        number->next = NULL;
        number->ones = 1;
        number->zeroes = 0;
        return number;
    }
    else if( number->zeroes == 0 )
    {
        if( ! number->next )
        {
            number->zeroes = number->ones;
            number->ones = 1;
            return number;
        }
        else if( number->next->zeroes > 1 )
        {
            number->next->zeroes--;
            number->zeroes = number->ones;
            number->ones = 1;
            return number;
        }
        else
        {
            next = number->next;
            next->ones++;
            next->zeroes = number->ones;
            free(number);
            return next;
        }
    }
    else if( number->zeroes == 1 )
    {
        number->ones++;
        number->zeroes = 0;
        return number;
    }
    else
    {
        number->zeroes--;
        next = number;
        number = malloc( sizeof( *number ) );
        number->next = next;
        number->ones = 1;
        number->zeroes = 0;
        return number;
    }
}

void baumsweet(int limit)
{
    struct bits * number = NULL;
    struct bits * i;

    while( limit-- >= 0 )
    {
        for( i = number ; i ; i = i->next )
        {
            if( i->zeroes & 1 )
            {
                break;
            }
        }
        printf( i ? "0\n" : "1\n" );
        number = increment( number );
    }
}

int main( int argc, char * argv[] )
{
    baumsweet( argv[1] ? atoi( argv[1] ) : 20 );
    return 0;
}
```

## Challenge Output for n = 20:
```
1, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0
```