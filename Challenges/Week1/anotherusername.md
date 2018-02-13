# Solution for [Week 1](Index)
## Author: [Magus](https://what.thedailywtf.com/user/magus)

<a name="Javascript"></a>
## Language: Javascript

```
function BaumSweet(n) {
  for (var a = [], i = 0; i <= n; ++ i) {
    for (var j = i; j && j % 4 == 0; j /= 4);
    a[i] = j ? j % 2 ? a[(j - 1) / 2] : 0 : 1;
  }
  console.log(a.join(', '));
  return a;
}
```

## Challenge Output for n = 20:
```
1, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0
```