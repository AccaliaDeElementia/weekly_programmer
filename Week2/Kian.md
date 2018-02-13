# Solution for [Week 2](Challenge)
## Author: [Kian](https://what.thedailywtf.com/user/kian)

<a name="CPlusPlus"></a>
## Language: C++

Brute Force solution:
```
// pvwpc2.cpp

#include <chrono>
#include <iostream>
#include <numeric>
#include <vector>

std::vector<long long> FindPrimes(long long max) {
	std::vector<long long> primes {2, 3, 5, 7};
	for (long long i {8}; i < max; ++i) {
		auto isPrime = true;
		for (auto p : primes) {
			if (i%p == 0) {
				isPrime = false;
			}
		}
		if (isPrime) {
			primes.push_back(i);
		}
	}
	return primes;
}

long long SumOfPrimes(long long max) {
	auto primes = FindPrimes(max);
	return std::accumulate(primes.begin(), primes.end(), (long long)0);
}

int main(int argc, char** argv) {
	if (argc != 2) return -1;
	auto max = std::atoll(argv[1]);
	
	auto start = std::chrono::system_clock::now();
	auto sum = SumOfPrimes(max);
	auto end = std::chrono::system_clock::now();
	std::cout << "Sum of primes to " << max << " : " << sum << "\nTime: " << std::chrono::duration<float>{end - start}.count() << " s \n";

	return 0;
}
```

Sieve Solution:
```
// pvwpc2.cpp

#include <chrono>
#include <iostream>
#include <numeric>
#include <vector>

std::vector<bool> Sieve(long long max) {
    std::vector<bool> sieve(max);
    long long p {2};
    while (p*p <= max) {
        for (auto it {(p*p) - 1}; it < max; it += p) {
            sieve[it] = true;
        }
        do {
            ++p;
        } while (sieve[p-1]);
    }
    return sieve;
}

long long SumOfPrimes(long long max) {
    auto sieve = Sieve(max);
    long long sum {2};
    for (long long i = 2; i < max; i += 2) {
        if (!sieve[i]) {
            sum += i+1;
        }
    }
    return sum;
}

int main(int argc, char** argv) {
    if (argc != 2) return -1;
    auto max = std::atoll(argv[1]);

    auto start = std::chrono::system_clock::now();
    auto sum = SumOfPrimes(max);
    auto end = std::chrono::system_clock::now();
    std::cout << "Sum of primes to " << max << " : " << sum << "\nTime: " << std::chrono::duration<float>{end - start}.count() << " s \n";

    return 0;
}
```