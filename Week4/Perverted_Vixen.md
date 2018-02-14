# Solution for [Week 3](./)
## Author: [Perverted_Vixen](https://what.thedailywtf.com/user/Perverted_Vixen)

<a name="Javascript"></a>
## Language: Javascript

```
const recurr = (str) => {
    const map = {};
    let first = null;
    let position = -1;
    let char = null;
    let gap = -1;
    `${str}`.split('').forEach((c, i) => {
        if (map[c]) {
            if (!first) {
                first = c;
                position = map[c];
            }
            if (i - map[c] > gap) {
                char = c;
                gap = i - map[c];
            }
        }
        map[c] = i;
    });
    return { first, position, char, gap, str };
};

[
    'the quick brown fox jumps over the lazy dog',
    'IKEUNFUVFV',
    'PXLJOUDJVZGQHLBHGXIW',
    '*l1J?)yn%R[}9~1"=k7]9;0[$',
    156248267293222,
    '0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz'
].map(recurr)
.map(r=>`${r.str} => \`${r.first}\`@${r.position} (\`${r.char}\`[${r.gap}])`)
    .forEach(result => console.log(result));
```