---
title: "What polynomial time actually means"
date: 2026-05-17
captured: 2026-05-17T12:25:37.097Z
tags: ["complexity-theory", "cs", "math"]
source: "Claude.ai chat"
---
"Polynomial time" is the line that complexity theory draws between problems that are practically solvable and problems that aren't. The definition is mathematical, but the intuition is concrete: polynomial = the algorithm scales gracefully with input size; exponential = it doesn't.

![Polynomial vs exponential growth](https://assets.han-ws.workers.dev/i/2026/05/polynomial-vs-exponential.svg)

## Definition

An algorithm runs in polynomial time if its runtime, for input size `n`, is bounded above by a polynomial:

`T(n) = a · n^k + b · n^(k-1) + ... + c`

The key property: **the highest exponent k is a fixed constant**, not a function of n. So n, n², n³, n^10, even n^100 are all polynomial. The runtime gets bad but it doesn't explode.

## Polynomial vs exponential

| Class | Examples | Character |
|---|---|---|
| Polynomial | n, n², n³, n^10 | "tractable", grows gracefully |
| Exponential | 2^n, 3^n, n! | "intractable", explodes quickly |

## Concrete numbers (1 op = 1 nanosecond)

For input size n = 100:

| Algorithm cost | Time | Feel |
|---|---|---|
| O(n) | 100 ns | instant |
| O(n²) | 10 µs | instant |
| O(n³) | 1 ms | still instant |
| O(n^10) | ~3,000 years | still "polynomial" |
| O(2^n) | ~10²² years | longer than the age of the universe |

The age of the universe is roughly 1.4 × 10^10 years. O(2^100) is *trillions* of times longer than that.

## The intuition in code

```python
# O(n): linear, polynomial
for i in range(n):
    do_thing(i)

# O(n²): quadratic, polynomial
for i in range(n):
    for j in range(n):
        do_thing(i, j)

# O(2^n): exponential, NOT polynomial
def subsets(arr):
    if not arr:
        return [[]]
    rest = subsets(arr[1:])
    return rest + [[arr[0]] + s for s in rest]
```

Three nested for-loops gives n³, polynomial. Recursion that doubles on each step gives 2^n, exponential. The difference is whether n appears as the *base* of the exponent (polynomial) or as the *exponent itself* (exponential).

## Why "polynomial" is the threshold

Polynomial time has three properties that make it the natural definition of "feasible":

1. **Closed under composition.** A polynomial algorithm calling another polynomial algorithm is still polynomial. This makes the class robust under modular programming.
2. **Robust across computing models.** Whether you measure on a Turing machine, a RAM machine, or x86, "polynomial" is still "polynomial". The degree may change, but the class doesn't.
3. **Matches practical feasibility.** Even n^10 is bad, but there's hope of optimization. 2^n is hopeless from the start.

## The hidden caveat

Polynomial time is a theoretical bound, not a practical one. An O(n^100) algorithm is technically polynomial but useless in practice. An O(n · 2^(0.001 · n)) algorithm is exponential but might run fine for small n.

In practice, "polynomial" usually means O(n²) or O(n³). Anything worse and people start hunting for better algorithms.

## Vietnamese

- Polynomial time → thời gian đa thức
- Exponential time → thời gian mũ
- Tractable → khả thi / giải được
- Intractable → không khả thi / khó giải

These are stable translations in Vietnamese academic writing.

## Related

- [[complexity-classes-p-np-bqp-qma-explained]] - the P / BPP / BQP classes that all rest on this polynomial-time threshold
- [[why-quantum-computing-talks-about-decision-problems]] - decision problems are the unit on which polynomial-time bounds are stated
- [[history-and-motivation-of-major-quantum-algorithms]] - Shor's polynomial-time factoring (O((log N)³)) is the bridge case
- [[monomial-polynomial-term-vietnamese-terminology-breakdown]] - the Vietnamese math vocabulary (đa thức / đơn thức) behind "polynomial"
- [[vietnamese-terminology-for-quantum-computing]] - quantum-side Vietnamese glossary