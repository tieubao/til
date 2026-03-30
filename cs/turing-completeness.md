---
title: "Turing completeness"
date: 2026-03-30
captured: 2026-03-30T16:14:43.354Z
tags: ["computation", "theory", "fundamentals"]
source: "Claude.ai chat"
---
## Definition

A system is **Turing complete** if it can simulate a Turing machine, meaning it can compute anything that is theoretically computable, given enough time and memory. It is the bar for "general-purpose computation."

The concept comes from Alan Turing's 1936 thought experiment: an imaginary machine with an infinite tape (memory cells), a read/write head that moves left or right, and a table of state rules (the "program"). The machine loops: read the current cell, check the rules, write a value, move the head, repeat. Turing proved this absurdly simple setup can solve any problem that any computer can solve.

![Diagram showing the 3 parts of a Turing machine: infinite tape, read/write head, and state rules](assets/cs/turing-machine-parts.svg)

## The three ingredients

A system needs all three of these to be Turing complete:

1. **Conditional branching** (if/else, pattern matching)
2. **Looping or recursion** (ability to repeat)
3. **Arbitrary/unbounded memory** (read and write without a fixed limit)

If any one of those is missing, the system can only handle a subset of computable problems.

## Examples

![Comparison chart: Turing complete systems vs NOT Turing complete systems](assets/cs/turing-complete-examples.svg)

| Turing complete | NOT Turing complete |
|---|---|
| Python, JavaScript, C, Java | HTML, CSS, JSON, regex |
| Excel (with formulas) | A basic calculator |
| Conway's Game of Life | SQL (without extensions) |

The left column has all three ingredients. The right column is missing at least one (HTML can't loop, a calculator can't branch, pure SQL can't recurse arbitrarily).

## The catch

Turing completeness says nothing about speed or practicality. You could write an operating system in Conway's Game of Life. You absolutely should not.

It also does not mean infinite power. There are problems (like the halting problem) that no Turing-complete system can solve. Turing complete is the ceiling for computation, and it turns out to be surprisingly easy to reach.