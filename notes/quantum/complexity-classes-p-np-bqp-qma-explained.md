---
title: "Complexity classes P NP BQP QMA explained"
date: 2026-05-17
captured: 2026-05-17T12:25:14.955Z
tags: ["quantum", "complexity-theory", "cs"]
source: "Claude.ai chat"
---
Complexity classes group problems by how hard they are to solve and verify. The classical hierarchy (P, NP, NP-Complete, NP-Hard) was extended for quantum computing with BQP and QMA. The relationships between these classes drive much of what quantum computers can and cannot do.

![Complexity class containment diagram](https://assets.han-ws.workers.dev/i/2026/05/complexity-classes-containment.svg)

## Classical classes

**P (Polynomial time).** Problems a classical computer can solve in time polynomial in input size. The "tractable" zone. Sorting, shortest path, primality testing.

**NP (Nondeterministic Polynomial time).** Problems where a *given* solution can be verified in polynomial time, even if finding one is hard. Sudoku is a classic example: hard to solve, easy to check. Factoring is in NP because given the factors, multiplying them back to verify is fast.

**NP-Complete.** The "hardest" problems in NP. If one NP-Complete problem can be solved in polynomial time, then *all* NP problems can. Travelling Salesman, SAT, graph coloring.

**NP-Hard.** At least as hard as NP-Complete, but possibly outside NP (no requirement that solutions be verifiable in polynomial time).

## Quantum classes

**BQP (Bounded-error Quantum Polynomial time).** The quantum analogue of P. Problems a quantum computer can solve in polynomial time with bounded error probability (typically ≥ 2/3 correct). Repeat the algorithm a few times to drive error arbitrarily low. Factoring sits here (Shor's algorithm). Discrete logarithm sits here. Simulating quantum systems sits here.

**QMA (Quantum Merlin-Arthur).** The quantum analogue of NP. Named for a game: Merlin (an all-knowing prover) hands Arthur (a quantum-equipped verifier) a quantum state as a "proof". Arthur checks it in polynomial time on a quantum computer. The key difference from NP: the proof itself is a *quantum* state (can be in superposition, can be entangled), not a classical bit string. Local Hamiltonian problem is QMA-Complete.

## Known containment

```
P ⊆ BQP ⊆ QMA
P ⊆ NP  ⊆ QMA
```

- BQP contains P (a quantum computer can do anything a classical one can).
- QMA contains NP (Arthur can ignore the quantum part and just read a classical proof).
- **Whether BQP contains NP is an open problem.** This is the quantum analogue of P vs NP and is just as unsolved.

## The Millennium question: P vs NP

One of the seven Millennium Prize Problems, $1M for a proof either way.

- If **P = NP**, every problem with fast verification has a fast solution. RSA, blockchain signatures, digital signatures, most modern cryptography collapses overnight.
- If **P ≠ NP** (which most researchers believe), there are problems that are inherently hard to solve no matter what classical algorithm you invent.

Most cryptographic security rests on the *empirical* observation that P ≠ NP. We have no proof, just 50+ years of failure to find polynomial algorithms for NP-Complete problems.

## Why factoring landed in BQP

Factoring's classical complexity is unknown. The best known classical algorithm (General Number Field Sieve) runs in **sub-exponential** time, slower than polynomial but faster than fully exponential. Nobody has proved factoring is or isn't in P.

What Shor proved in 1994 is that factoring is in **BQP**. He didn't solve it on a classical computer faster, he reformulated it as a period-finding problem and solved *that* with Quantum Fourier Transform in polynomial time on a quantum machine. The factoring step itself becomes trivial classical arithmetic once the period is known.

## Vietnamese

These class names are kept in English in Vietnamese-language technical writing because they are international standards. Only the descriptive terms get translated:

- Complexity class → lớp độ phức tạp
- Polynomial time → thời gian đa thức
- Exponential time → thời gian mũ
- Bounded-error → sai số giới hạn
- Tractable / Intractable → khả thi / không khả thi