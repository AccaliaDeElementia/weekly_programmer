# Solution for [Week 3](./)
## Author: [dkf](https://github.com/dkfellows)

<a name="TCL"></a>
## Language: TCL

```
package require Tcl 8.6

# Algorithm adapted from Wikipedia; code stealing, ahoy!
# NB: REQUIRES integer coins; takes finite time
proc change_making {coins target} {
    set m [lrepeat [expr {[llength $coins] + 1}] [lrepeat [expr {$target + 1}] 0]]
    set C [lrepeat [expr {[llength $coins] + 1}] [lrepeat [expr {$target + 1}] {}]]
    for {set i 0} {$i <= $target} {incr i} {
        lset m 0 $i $i
        lset C 0 $i "?"
    }

    for {set c 1} {$c <= [llength $coins]} {incr c} {
        set c1 [expr {$c - 1}]
        for {set r 1} {$r <= $target} {incr r} {
            if {[lindex $coins $c1] == $r} {
                lset m $c $r 1
                lset C $c $r [lindex $coins $c1]
            } elseif {[lindex $coins $c1] > $r} {
                lset m $c $r [lindex $m $c1 $r]
                lset C $c $r [lindex $C $c1 $r]
            } else {
                set rc [expr {$r - [lindex $coins $c1]}]
                set count [expr {
                    min([lindex $m $c1 $r], 1 + [lindex $m $c $rc])
                }]
                lset m $c $r $count
                if {$count != 1 + [lindex $m $c $rc]} {
                    lset C $c $r [lindex $C $c1 $r]
                } else {
                    lset C $c $r [list [lindex $coins $c1] {*}[lindex $C $c $rc]]
                }
            }
        }
    }

    set found [lindex $C end end]
    # If one of these markers was there, we failed
    if {"?" ni $found} {
        return $found
    }
    return {}
}

# Wrap to use fractional coins and search for imperfect solutions. The
# imperfect solutions are from a small range; can't be more than the smallest
# allowed coin out.
proc find_change {coins target {base 0.01}} {
    set least [lindex [lsort -real $coins] 0]
    set iCoins [lmap c $coins {
        expr {round($c / $base)}
    }]
    foreach c $coins ic $iCoins {dict set map $ic $c}

    for {set delta 0.0} {$delta < $least} {set delta [expr {$delta + $base}]} {
        set iTarget [expr {round(($target + $delta) / $base)}]
        set found [change_making $iCoins $iTarget]
        if {[llength $found]} break
    }

    return [lsort -decreasing -real [lmap c $found {dict get $map $c}]]
}

fconfigure stdout -buffering none
puts -nonewline "Coins: "; gets stdin coins
puts -nonewline "Target: "; gets stdin target
puts "Change: [find_change $coins $target]"
```