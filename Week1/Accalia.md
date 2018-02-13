# Solution for [Week 1](Challenge)
## Author: [Accalia](https://github.com/AccaliaDeElementia)

<a name="Javascript"></a>
## Language: Javascript

```
const baumSweet = (n) => /1(00)*0(1|$)/.test(n.toString(2)) ? 0 : 1;
const baumSweetSeq = n => new Array(n + 1).fill().map((_,n) => baumSweet(n));
console.log(baumSweetSeq(20).join(', '));
```

## Challenge Output for n = 20:
```
1, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0
```

## Author Comments
Did the challenge in Javascript, natch.