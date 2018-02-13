# Solution for [Week 1](Challenge)
## Author: [dkf](https://github.com/dkfellows)

<a name="TCL"></a>
## Language: TCL


```
proc bsn {n} {
    # One special case
    if {$n == 0} {return 1}
    foreach seq [regexp -all -inline 0* [format %llb $n]] {
	if {[string length $seq] & 1} {
	    return 0
	}
    }
    return 1
}

proc bs n {
    set a {}
    for {set i 0} {$i <= $n} {incr i} {
	lappend a [bsn $i]
    }
    return [join $a ", "]
}

puts [bs [lindex $argv 0]]
```

## Challenge Output for n = 20:
```
$ tclsh8.6 /tmp/baumsweet.tcl 20
1, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0
```

## Author Comments
I wrote it in Tcl 'cos I like doing it in that.

All with plenty of BS!