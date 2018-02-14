# Solution for [Week 1](Challenge)
## Author: [cvi](https://what.thedailywtf.com/user/cvi)

<a name="CPlusPlus"></a>
## Language: C++

```c++
#include <cstdint>
#include <x86intrin.h>

inline
bool is_baum( std::uint64_t aValue )
{
	auto x = ~aValue;

	x ^= x << 1u;
	x ^= x << 2u;
	x ^= x << 4u;
	x ^= x << 8u;
	x ^= x << 16u;
	x ^= x << 32u;

	return !(x & aValue);
}


#include <cstdio>
#include <inttypes.h>

int main( int aArgc, char* aArgv[] )
{
	char dummy; 
	std::uint64_t upper = 20;

	if( 2 != aArgc || 1 != std::sscanf( aArgv[1], "%" SCNu64 "%c", &upper, &dummy ) )
	{
		std::fprintf( stderr, "Synopsis: %s <number>\n", aArgv[0] );
		return 1;
	}

	for( std::uint64_t i = 0; i <= upper; ++i )
		std::printf( "%d ", int(is_baum(i)) );

	return 0;
}
```

## Challenge Output for n = 20:
```
1, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0
```