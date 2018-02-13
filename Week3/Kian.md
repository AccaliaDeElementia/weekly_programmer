# Solution for [Week 3](./)
## Author: [Kian](https://what.thedailywtf.com/user/kian)

<a name="CPlusPlus"></a>
## Language: C++
```
#include <algorithm>
#include <iostream>
#include <limits>
#include <map>
#include <sstream>
#include <vector>

int CountCoins(const std::map<int, int>& change) {
    auto count = 0;
    for (auto& c : change) {
        count += c.second;
    }
    return count;
}

int SumAmount(const std::map<int, int>& change) {
    auto sum = 0;
    for (auto& c : change) {
        sum += c.first * c.second;
    }
    return sum;
}

std::map<int, int> Coins(int amount, const int* coins, int numCoins) {
    static std::map<int, std::map<int, int>> memo;

    if (amount <= 0) return {};

    auto it = memo.find(amount);
    if (it != memo.end()) return it->second;

    auto candidate = 0;
    std::map<int, int> change;
    int minCoins {int(std::numeric_limits<int>::max())};
    auto minRemainder = coins[numCoins-1];
    for (auto i = 0; i < numCoins; ++i) {
        auto attempt = Coins(amount - coins[i], coins, numCoins);
        auto remainder = SumAmount(attempt) - (amount  - coins[i]);
        auto count = CountCoins(attempt);
        if (remainder < minRemainder
            || (remainder == minRemainder && count < minCoins)) {
            change = std::move(attempt);
            candidate = coins[i];
            minCoins = count;
            minRemainder = remainder;
        }
    }
    if (candidate != 0) {
        ++change[candidate];
    }
    memo[amount] = change;
    return change;
}

constexpr int ToInts(float amt, int mult) {
    return amt * mult + 0.5f;
}

constexpr float ToFloat(int amt, float div) {
    return amt / div;
}

constexpr int cents {100};

int main() {
    std::string line;
    std::getline(std::cin, line);
    std::stringstream ss {line};
    float coin;
    std::vector<int> coins;
    while (ss >> coin) {
        coins.push_back(ToInts(coin, cents));
    }
    std::sort(coins.begin(), coins.end(), std::greater<int>());
    float amt;
    std::cin >> amt;
    auto amount = ToInts(amt, cents);
    auto change = Coins(amount, coins.data(), coins.size());

    for (auto it = change.rbegin(); it != change.rend(); ++it) {
        for (auto n = 0; n < it->second; ++n) {
            std::cout << ToFloat(it->first, cents) << ' ';
        }
    }
    std::cout << '\n';

    return 0;
}
```

## Author Comments

I do a depth first search, starting with the highest denomination coins and building a map of results as I go. For any given amount, I try every combination and keep the one that gives the smallest amount of change. If two give the same change I keep the one that uses the fewest coins. When it's asked for zero or less it returns an empty set.
I might be able to optimize it a little, avoid some obviously wrong branches (for example, if using a particular coin makes me go over by more than the smallest denomination coin, that's obviously a dead end and I can skip it), but I didn't want to make the code more complicated. I doubt it makes much of a difference.

There might be some redundant work here, in that it searches for identical combinations of coins (0.25 + 0.5 vs 0.5 + 0.25), but as the memo builds up that work should be cut down quickly.