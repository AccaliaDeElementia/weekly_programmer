# Solution for [Week 2](Challenge)
## Author: [Perverted_Vixen](https://what.thedailywtf.com/user/Perverted_Vixen)

<a name="Javascript"></a>
## Language: Javascript

```
l = 1e7
c = new Array(l + 1).fill(1)
c[0] = 0
c[1] = 0
K = Math.ceil(Math.sqrt(l))
for (i = 2; i < k; i++) {
    if (c[i] !== 1) {
        continue;
    }
    for (j = 2 * i; j <= l; j += i) {
        c[j] = 0;
    }
}
console.log(c.reduce((a, v, n) => a + (v === 1 ? n : 0), 0))
```