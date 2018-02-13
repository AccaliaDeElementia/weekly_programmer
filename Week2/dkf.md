# Solution for [Week 2](Challenge)
## Author: [dkf](https://github.com/dkfellows)

<a name="TCL"></a>
## Language: TCL


```
package require Tcl 8.6

proc primes_seq {} {
    yield [info coroutine]
    set q 2
    for {set q 2} true {incr q} {
	if {![info exist d($q)]} {
	    yield $q
	    set d([expr {$q*$q}]) [list $q]
	} else {
	    foreach p $d($q) {
		lappend d([expr {$p+$q}]) $p
	    }
	    unset d($q)
	}
    }
}
coroutine primes primes_seq

proc sum_to {gen n} {
    set total 0
    while true {
	set i [$gen]
	if {$i >= $n} {
	    return $total
	}
	incr total $i
    }
}

puts [sum_to primes [lindex $argv 0]]
```