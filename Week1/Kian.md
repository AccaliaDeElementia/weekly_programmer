# Solution for [Week 1](Index)
## Author: [Kian](https://what.thedailywtf.com/user/kian)

<a name="CPlusPlus"></a>
## Language: C++

```c++
// wpc1
#include <cstdlib>
#include <iostream>

int BaumSweetNumber(int i) {
	if (i == 0) return 0;
	auto run = 0;
	while (i > 0) {
		if ((i & 1) == 0) {
			++run;
		} else {
			if ((run%2) != 0) {
				return 0;
			} else {
				run = 0;
			}
		}
		i = i >> 1;
	}
	return 1;
}

int main(int argc, char** argv) {
	if (argc != 2) return -1;
	auto n = std::atoi(argv[1]);
	if (n < 0) return -1;
	
	for (auto i = 0; i <= n; ++i) {
		std::cout << BaumSweetNumber(i) << ' ';
	}
	std::cout << '\n';
	
	return 0;
}
```

## Challenge Output for n = 20:
```
    0 1 0 1 1 0 0 1 0 1 0 0 1 0 0 1 1 0 0 1 0
```

## Author Comments
Added some very basic error protection. Because of how the parsing works, if you don't pass a second parameter to the program (the first parameter is the name of the program) or you pass a negative number, you get no output and a -1 return code. If you pass something that is not a number, you get an output of zero. Something bigger than a 32 bit signed integer results in undefined behavior. I'm using a very primitive C function for parsing, so it doesn't even tell me if it was able to parse the number. The output is zero, the number, or undefined behavior.