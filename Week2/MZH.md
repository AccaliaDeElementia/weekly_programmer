# Solution for [Week 2](Challenge)
## Author: [MZH](https://what.thedailywtf.com/user/MZH)

<a name="C++"></a>
## Language: C++

```
#include <iostream>
#include <type_traits>

template<unsigned n, unsigned divisor>
struct is_prime_helper
{
    enum
    {
        value = ((n % divisor != 0) && is_prime_helper<n, divisor - 1>::value)
    };
};

template<unsigned n>
struct is_prime_helper<n, 1>
{
    enum
    {
        value = 1
    };
};

template<unsigned n>
struct is_prime
{
    enum
    {
        value = is_prime_helper<n, n/2>::value
    };
};

template<unsigned m, unsigned n>
struct sum_of_primes : std::enable_if<m <= n>
{
    enum
    {
        value = (is_prime<m>::value ? m : 0) + sum_of_primes<m+1,n>::value
    };
};

template<unsigned n>
struct sum_of_primes<n, n>
{
    enum
    {
        value = 0
    };
};

template<unsigned n>
struct sum_of_primes_up_to
{
    enum 
    {
        value = sum_of_primes<2, n>::value
    };
};

int main()
{
    std::cout << sum_of_primes_up_to<334>::value << std::endl;

    return 0;
}
```