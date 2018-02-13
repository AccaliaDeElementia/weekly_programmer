# Solution for [Week 3](./)
## Author: [anotherusername](https://what.thedailywtf.com/user/anotherusername)

<a name="Javascript"></a>
## Language: Javascript

```
function makeChange(coins, amount) {
  coins = [amount, ...coins];
  var d = coins.reduce((n, c) => Math.max(n, String(c).replace(/^\d*\.?/, '').length), 0);
  [amount, ...coins] = coins.map(c => Number(Number(c).toFixed(d).replace('.', '')));
  for (var t = Math.pow(10, d), solution = false, c = [0], s = [0], i = 0; i < amount; ++ i) {
    if (i in s) for (var n, j = 0; j < coins.length; ++ j) {
      n = i + coins[j];
      if (!(n in s) || s[n] > s[i]) {
        c[n] = coins[j];
        s[n] = s[i] + 1;
      }
    }
  }
  
  // find next best solution if exact change can't be created
  for (var m = s.length; !(i in s) && i <= m; ++ i);
  
  // hop backward through the array to retrace the steps to the solution
  if (i in s) for (solution = Array(s[i]); i > 0; i -= c[i]) solution[s[i] - 1] = c[i];
  return solution ? solution.map(c => Number((c / t).toFixed(d))) : solution;
}
```

## Author Comments
This one basically creates a sparse array and then fills it in with all the possible values that can be created. At each amount, two values are stored (two separate arrays, to keep it simple): the last coin that was used to get the amount, and the total number of coins that were used to get the amount.

Starting from the beginning (0 coins, 0 value), it steps upward, and at each value in the array (that exists -- i.e., it can be created using the coins), it tries adding the value of each possible coin; the resulting new value is checked to see if this is the best (or only) path to that value. If so, it's either not been reached yet or we've reached it in fewer total coins than we did before, so store the coin used and the total coins used to reach it.

Once we've reached the target amount, there are two possibilities. If we were able to exactly make that amount of change, at that location we'll find the last coin that was used and the number of coins it took. Otherwise, that location will be empty, i.e. not possible; in this case, iterate upward until we find the first amount that is possible.

Now that we've found an amount that we can make, and it satisfies the original conditions, we need to recreate the path that we used to reach it. This is quite simple: at each value, we've stored the last coin that was used; if we subtract the value of this coin, it gives us the previous value. There, we will have the last coin that we used to reach that value, so we subtract its value, and so on, moving back until at last we reach index 0.

Quite interestingly, it came up with a different (but also correct) result for the challenge input which took my first algorithm quite a long time. As the result is correct, and the problem didn't specify that one of them should be preferred, I'm not inclined to try to tweak the one or the other to get their outputs to exactly match.