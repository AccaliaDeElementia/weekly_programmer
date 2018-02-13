# Solution for [Week 3](./)
## Author: [Perverted_Vixen](https://what.thedailywtf.com/user/Perverted_Vixen)

<a name="Javascript"></a>
## Language: Javascript

### Naive Solution

```
const calculateChangeNaive = (change, coins = [.5, .25, .1, .05, .01]) => {
    const fudge = 1e-7; // fudge factor for floating point math
    const makeChange = (change) => {
        const output = [];
        coins.sort((a, b) => b - a);
        coins.forEach((coin) => {
            // BUGBUG: this works correctly for standard values of coins, even with the penny removed
            // But for some sets of coins the math doesn't work out to allow this heuristical shortcut
            // Has to do with GCD of pairs of ordered coins or something.....
            while (change + fudge >= coin) {
                output.push(coin);
                change -= coin;
            }
        });
        if (change > fudge) {
            output.push(coins[coins.length - 1]);
        }
        return output;
    };
    let res = makeChange(change, coins);
    const sum = res.reduce((a, b) => a + b, 0);
    if (Math.abs(sum - change) > fudge) {
        res = makeChange(sum, coins);
    }
    return res;
};

const tests = [
    [4.99, [1, .5, .25, .1, .05, .01]],
    [4.89, [1, .5, .25, .1, .05]],
    [.66, [1, .5, .25, .1, .05, .01]],
    [16.01, [5, 2, .5, .25, .1, .05]],
    [.66, [.5, .25, .1, .05]],
    [.3, [.5, .25, .1]],
    [.25, [.5, .25, .1]]
];

const fn = calculateChangeNaive;
tests.forEach(([target, coins]) => {
    console.log('Input:');
    console.log(`Target: ${target}`);
    console.log(`Coins: ${coins.join(', ')}`);
    console.log(`Output: ${fn(target, coins).join(', ')}`);
    console.log();
});
```

### Better Solution

```
const calculateChange = (change, coins = [.5, .25, .1, .05, .01]) => {
    const factor = Math.pow(10, coins.concat(change)
        .map(coin => (`${coin}`).split('.').pop())
        .reduce((acc, val) => Math.max(val.length, acc), 0));
    change = Math.round(change * factor);
    coins = coins.map(coin => Math.round(coin * factor));

    // Initial search space
    const queue = coins.map((coin,i) => {
        return {
            remain: change - coin,
            change: [coin],
            last: coin
        };
    });

    // Don't forget about single coin solutions
    let solution = queue.filter(s => s.remain === 0).shift();

    // Remember the solution we've found where we went over by juuuust a bit
    // (covers the case where we can't make exact change and give more change than needed)
    let over = undefined;

    // Remember these for later. since we visit solutions from shortest to longest if we've been at
    // a remainder before we can stop looking on this path cause it's longer
    const remainders=[];

    // Breadth First Search through the solution space
    while (!solution && queue.length) {
        let c = queue.shift();
        // Build new candidates using coins strictly less than or equal in value to last added coin
        // Prevents excessive coverage of the search space
        coins.filter(coin => coin <= c.last).forEach(coin => {
            // Create the new candidate
            const newc = {
                remain: c.remain - coin,
                change: c.change.concat([coin]),
                last: coin
            };
            // Did we find exact solution?
            if (newc.remain === 0) {
                // SOLUTION GET!
                solution = newc;
            }
            // Are we not quite there yet?
            // Have we seen ths remainder yet?
            // No? This is the shortest way to get that remainder, mark it so if we get here the long
            // way we know to prune that search path
            else if (newc.remain > 0 && !remainders[newc.remain]) {
                // Continue down this search path
                remainders[newc.remain] = 1;
                queue.push(newc);
            }
            // Did we go over, and did we go over by less than we've seen before?
            else if (newc.remain < 0 && (!over || over.remain < newc.remain)) {
                // Well, we need to abandon this search path, but remember this one for later,
                // we might need it if we don't get exact solution
                over = newc;
            }
        });
    }
    return (solution || over || { change: [] }).change.map(coin => coin / factor);
};

const tests = [
    [4.99, [1, .5, .25, .1, .05, .01]],
    [4.89, [1, .5, .25, .1, .05]],
    [.66, [1, .5, .25, .1, .05, .01]],
    [16.01, [5, 2, .5, .25, .1, .05]],
    [.66, [.5, .25, .1, .05]],
    [.3, [.5, .25, .1]],
    [.25, [.5, .25, .1]]
];

const fn = calculateChange;
tests.forEach(([target, coins]) => {
    const result=fn(target, coins);
    console.log('Input:');
    console.log(`Target: ${target}`);
    console.log(`Coins: ${coins.join(', ')}`);
    console.log(`Output: ${result.join(', ')}`);
    console.log(`Total: ${(Math.round(100*result.reduce((a, b) => a + b, 0))/100).toFixed(2)}`);
    console.log();
});
```

### Another Approach

```
(target, coins) => {
    const factor = Math.pow(10, coins.concat(target)
        .map(x => /\d+(.(\d*))?/.exec(x)[2] || '')
        .reduce((a, v) => Math.max(v.length, a), 0));
    target = Math.round(target * factor);
    coins = coins.map(x => Math.round(x * factor));
    const values = [0];
    let i = 0;
    for (; i < target; i++) {
        if (i in values) {
            coins.forEach(coin => {
                const next = i + coin;
                if (!(next in values)) {
                    values[next] = coin
                }
            });
        }
    }
    i = target;
    while (!(i in values)) {
        i++;
    }
    const solution = [];
    for (; i > 0; i -= values[i]) {
        solution[i - 1] = values[i];
    }
    return solution.filter(value => value)
        .map(coin => coin / factor)
        .sort((a, b) => b - a)
}
```