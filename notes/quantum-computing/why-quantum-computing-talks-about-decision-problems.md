---
title: "Why Quantum Computing Talks About Decision Problems"
date: 2026-05-17
captured: 2026-05-17T04:47:38.030Z
tags: ["quantum-computing", "complexity-theory", "BQP", "decision-problems", "fundamentals"]
source: "Claude session on quantum computing fundamentals, CLIMB framework (Classify phase)"
---
# Why Quantum Computing Talks About Decision Problems

> **Core insight**: Quantum computing didn't choose decision problems — it inherited them. Decision problems are the **assembly language of complexity theory**, the one shape every other problem can be compiled down to. To say anything mathematically precise about "is this problem hard?", you need a common reference frame. YES/NO is that frame.

![Decision Problems: The Common Language of Complexity Theory](../assets/notes-quantum-computing/decision-problem-quantum.svg)

## TL;DR

A **decision problem** is a problem with a YES/NO answer (1 bit of output). Every major complexity class — `P`, `NP`, `BPP`, `BQP`, `QMA` — is defined as a *set of decision problems* solvable under some resource constraint. Quantum computing uses this same language for four reasons:

1. **Mathematical normalization** — decision problems are the simplest output shape, so they're the cleanest unit for comparing difficulty
2. **Complexity class definitions** — `BQP` literally means "decision problems solvable by a quantum machine in poly-time with bounded error"
3. **Physical fit** — measuring a qubit yields a bit, so YES/NO output is the natural endpoint of a quantum circuit
4. **Apples-to-apples comparison** — to say "quantum beats classical", both sides must speak the same output language

In practice, quantum algorithms (Shor, Grover) solve more than decision problems. But when we *analyze* them, we reduce them to decision form.

---

## 1. Mathematical normalization: why YES/NO is the lowest common denominator

Real-world computational problems come in many shapes:

| Shape | Example | Output |
|---|---|---|
| Search | Find smallest prime factor of N | An integer |
| Optimization | Shortest path through N cities | A path |
| Function | Factor N into p × q | Two integers |
| Counting | How many solutions to f(x)=0? | A non-negative integer |
| Decision | Does N have a factor < k? | YES or NO |

The trick: **any of the above can be reduced to a sequence of decision problems**. Want to find the smallest prime factor of N? Ask "is there a factor < N/2?", then "< N/4?", and so on (binary search). Each question is a decision problem. Solve them all, and you've solved the search.

This means: if we understand the difficulty of decision problems, we automatically understand the difficulty of everything else (up to a polynomial factor). So focus the theory there.

```
Function problem        Decision oracle           Reconstructed answer
  "factor N"      →     "is there a factor       →    found a factor
                         in [a, b]?"   (×log N)        of N
```

## 2. Complexity classes are *built* on decision problems

Every famous complexity class is defined the same way:

> A complexity class = the set of **decision problems** solvable by a machine of type M using resource R.

| Class | Machine | Resource | Domain |
|---|---|---|---|
| `P` | deterministic | polynomial time | classical |
| `NP` | nondeterministic | polynomial time | classical |
| `BPP` | probabilistic | poly time, error < 1/3 | classical |
| `BQP` | quantum | poly time, error < 1/3 | quantum |
| `QMA` | quantum verifier | poly time | quantum |

Without decision problems, you can't even *write down* the definition of BQP. There's no "BQP for function problems" that people work with at the same level of generality. The decision framing is structural, not stylistic.

What's known about the inclusions: `P ⊆ BPP ⊆ BQP`, `P ⊆ NP`. Whether `BQP ⊆ NP` or `NP ⊆ BQP` is open. `BQP` is suspected to be strictly larger than `BPP` (Shor's algorithm suggests this).

## 3. Quantum measurement physically outputs bits

This reason is the most underappreciated. **A quantum measurement on n qubits returns n classical bits.** That's not a design choice. It's how the Born rule works.

So when a quantum circuit terminates, its output is a bitstring sampled from a probability distribution. The cleanest way to define "the algorithm solved problem X" is:

- Measure the first qubit.
- If it's `1` more than 2/3 of the time, the answer is YES.
- If it's `0` more than 2/3 of the time, the answer is NO.

This is exactly the `BQP` definition. The YES/NO framing isn't imposed from outside — it falls out of the physics.

Compare to the **Deutsch-Jozsa algorithm**, the first quantum algorithm to provably beat classical: it asks "is f constant or balanced?" — a decision problem, by construction, because that's what fits the measurement model.

## 4. Apples-to-apples comparison

To claim "quantum is faster than classical", both sides need a shared notion of "solving the problem". If classical algorithms output integers and quantum algorithms output probability distributions over bitstrings, the comparison is meaningless.

Decision problems are the lingua franca:
- Classical machine: outputs 1 bit
- Quantum machine: measures, outputs 1 bit (with high probability)
- Same input space, same output space → comparable runtime

This is why proofs of quantum supremacy / advantage are always stated in terms of decision problems or sampling problems with decision-like verification.

---

## Nuance: in practice, quantum solves more than decision problems

**Shor's algorithm** finds the period of a function — a function problem. But when people prove "factoring is in BQP", they technically prove that the **decision version** ("does N have a factor < k?") is in BQP. The function version follows via the search-to-decision reduction.

**Grover's algorithm** is a search algorithm, not a decision algorithm. But its analysis is done in terms of "promise problems" (a slight generalization of decision problems), and its complexity is stated as O(√N) decision-oracle queries.

So the workflow looks like:

```
1. Have a real problem  (often: search, optimization, function)
2. Reduce it to a decision problem
3. Place that decision problem in a complexity class (e.g. BQP)
4. Lift the result back to the original problem
```

The decision problem is the **analytical bottleneck**, not the user-facing form.

---

## Code analogy (for the engineer brain)

Think of decision problems as **boolean return types**:

```go
// Decision problem signature
func Decide(input Input) bool { ... }

// Every other problem can be expressed as repeated decisions
func Search(input Input) Solution {
    lo, hi := 0, MAX
    for lo < hi {
        mid := (lo + hi) / 2
        if Decide(input, mid) {  // "Is there a solution ≤ mid?"
            hi = mid
        } else {
            lo = mid + 1
        }
    }
    return lo
}
```

If you can write the `Decide` function efficiently, you can write the `Search` function efficiently (with a `log N` overhead). Complexity theorists work at the `Decide` level because it's the smallest atom.

A quantum circuit is just `Decide` implemented with qubits and gates instead of registers and ALU ops. Same return type. Same analytical framework.

---

## CLIMB: where this sits in the taxonomy

**Classify** (top of CLIMB tree for quantum complexity):

```
Computational problems
├── Decision (1-bit output)            ← the canonical form
├── Function (k-bit output)            ← reducible to decision via binary search
├── Search                             ← reducible to decision
├── Optimization                       ← reducible to decision ("is there a solution of cost ≤ k?")
├── Counting (#P)                      ← reducible (harder, often #P-complete)
└── Sampling                           ← used in quantum supremacy proofs
```

Quantum complexity classes sit in this same tree, with `BQP` and `QMA` being the quantum analogs of `BPP` and `NP`.

---

## Follow-ups to learn next

- **Function → decision reduction**: the formal mechanism that lets us say "if `Decide` is in `BQP`, so is `Search`". Key tool: self-reducibility.
- **BQP structure and how it sits relative to P/NP**: the open question of whether BQP ⊆ PH, why BQP ≠ BPP is believed but unproven, what oracle separations exist.
- **Promise problems**: a generalization of decision problems where the input is *promised* to satisfy some condition. Critical for quantum because many quantum algorithms (Deutsch-Jozsa, Grover) are most naturally stated as promise problems, not pure decision problems.

---

## Related notes

- [[notes/quantum-computing/bqp-structure]] (to be written)
- [[notes/quantum-computing/function-to-decision-reduction]] (to be written)
- [[notes/quantum-computing/promise-problems]] (to be written)
- [[notes/computational-finance/]] (CLIMB framework reference)