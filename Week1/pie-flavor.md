# Solution for [Week 1](Challenge)
## Author: [pie-flavor](https://github.com/pie-flavor)

<a name="APL"></a>
## Language: APL

```
{⍵=0:1⋄1,~{1∊2|∊⍴¨1~¨⍨⍵⊂⍵}¨{⍵⊤⍨∊2⍴⍨1+⌊2⍟⍵}¨⍳⍵}
```

## Challenge Output for n = 20:
```
1, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0
```