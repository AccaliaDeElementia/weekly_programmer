# Solution for [Week 1](Challenge)
## Author: [pie-flavor](https://github.com/pie-flavor)

<a name="APL"></a>
## Language: APL

```
{+/(2=+⌿0=(⍳⍵)∘.|⍳⍵)/⍳⍵}
```

## Author Explanation
So `⍳⍵` gets us the first `⍵` integers. `∘.`, the outer product, is used in conjunction with the remainder operator (`|`) to give a big array of all possible remainders. Any elements that are equal to 0, are therefore multiples of that number. So add `0=`, and now we have a big boolean array, where 1 represents an exact multiple and 0 doesn't. `+⌿` adds all these rows together, leaving us with a 1-dimensional array of the total number of factors of each number. The prime test is pretty obvious - any number with only two factors. So adding a `2=` gives us a boolean array representing the indices of the prime numbers. Lastly, `/` against the original `⍳⍵` is used to translate this array of indexed booleans into the numbers themselves, and that's the Sieve. Then a `+/` is tacked on to sum them all together.