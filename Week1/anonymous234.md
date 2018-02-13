# Solution for [Week 1](Index)
## Author: [anonymous234](https://what.thedailywtf.com/user/anonymous234)

<a name="Pseudocode"></a>
## Language: Pseudocode

```
Variable definitions:
N = input number
n = current number
b = current bit
odd = 1 if we've seen an odd number of 0s, 0 otherwise


input N
push n := 0
loop:
    [N n]
    goto do_number 
    exit if n == N
    n += 1
end loop

function do_number: 
    [n]
    push odd := 0 [n odd]
    flip
    loop: //one bit per iteration
        [odd n]
        push n%2 [odd n bit]
        if bit == 0:
            flip [n odd]
            odd = !odd
        else: //bit = 1
           flip [n odd]
           if odd = 1: print 0, return
        [n odd]
        flip
        n /= 2
        if n == 0: print 1, return
    end loop
```

## Author Comments
![is_pseudocode_a_language.png](https://what.thedailywtf.com/assets/uploads/files/1517059396581-f5efc6ec-1b78-486d-b372-655b988a1844-image.png)

I wanted to write a version in Befunge. So I wrote a version in pseudocode first, then got tremendously bored when I had to "compile" it into the real language and just left it there 🤷. It's not like it would add much anyway.

I guess it actually works for any stack-based language. [Square brackets] are documentation, they indicate the state of the stack at that point (right is top, stack might have other stuff at the bottom as long as it's not relevant).