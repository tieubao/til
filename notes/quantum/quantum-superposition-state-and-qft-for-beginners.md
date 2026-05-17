---
title: "Quantum superposition state and QFT for beginners"
date: 2026-05-17
captured: 2026-05-17T12:26:05.466Z
tags: ["quantum", "superposition", "qft"]
source: "Claude.ai chat"
---
The three concepts that make quantum speedup possible: quantum state, superposition, and interference. Most "quantum is faster because it tries all answers in parallel" explanations are wrong. The real mechanism is more interesting and more constrained.

![Quantum state, superposition, and interference](https://assets.han-ws.workers.dev/i/2026/05/superposition-and-interference.svg)

## 1. Classical bit vs qubit

A classical bit is one of two values: 0 or 1. Like a light switch, definitively in one state.

A qubit can be 0, 1, or **both at once** — a state called superposition. Written in Dirac notation:

|ψ⟩ = α|0⟩ + β|1⟩

Where α and β are complex numbers (the "amplitudes") with α² + β² = 1.

- α² = probability of measuring 0
- β² = probability of measuring 1

## 2. Quantum state

The "ket" notation |ψ⟩ is a way to write the state of a quantum system. For a qubit, it's a 2D vector. For n qubits, it's a vector in 2^n-dimensional space.

A useful analogy: a spinning coin. While in the air, it's not heads, not tails — it's both, with some probability distribution. The moment you catch it, it collapses to one outcome. Before that catch, the spinning coin is genuinely in both states.

## 3. Superposition scales exponentially

This is where the power lives.

| Qubits | States held simultaneously |
|---|---|
| 1 | 2 |
| 2 | 4 |
| 10 | 1,024 |
| 20 | ~1 million |
| 50 | ~10^15 (1 quadrillion) |
| 300 | More than atoms in the observable universe |

When you have n qubits in superposition, a single quantum operation acts on all 2^n states *at the same time*. Apply a function f to a superposition of inputs |x⟩, and the output is a superposition of all f(x).

## 4. The measurement catch

The catch that ruins the "quantum parallelism" fantasy: when you measure, the superposition **collapses to one random outcome**. You computed f(0), f(1), ..., f(N) "in parallel", but you only see one of them.

So how is any of this useful?

## 5. Interference

The trick: amplitudes are **complex numbers**, not just probabilities. They can be positive, negative, or have any phase. When you combine quantum states, amplitudes for the same outcome can add up (constructive) or cancel (destructive).

The wave analogy: drop two stones in a pond. Where the wave crests meet, they amplify. Where a crest meets a trough, they cancel. Same principle in quantum amplitudes.

Quantum algorithms work by orchestrating this so that:
- **Wrong answers have amplitudes that cancel** (destructive interference)
- **Right answers have amplitudes that reinforce** (constructive interference)

When you finally measure, the probability of seeing a right answer is near 100%, the wrong answers near 0%.

## 6. Quantum Fourier Transform (QFT)

The QFT is the quantum version of the discrete Fourier transform. Classical FFT takes a time-domain signal and extracts its frequency components. QFT does the same but on a superposition.

In Shor's algorithm, QFT is the engine that extracts the hidden period r from a superposition of f(x) = a^x mod N values:

1. Build a superposition of all (x, f(x)) pairs.
2. The values of f repeat with period r — so the superposition has a hidden periodic structure.
3. Apply QFT. States whose x is not a multiple of (something related to) r interfere destructively. States that are interfere constructively.
4. Measure. With high probability, the result reveals r.

## The core insight

Quantum speedup is **not** "try all options in parallel". A classical computer with parallelism could do that.

Quantum speedup is "create a superposition, then arrange interference so wrong answers cancel each other out, leaving only the right answer when you measure".

This is why quantum only helps with problems that have the right kind of structure (periodicity, symmetry, eigenstructure). Unstructured problems get at best a square-root speedup (Grover), not exponential.

## Common misconception, corrected

> "A quantum computer with 300 qubits explores 2^300 paths in parallel."

This is half-true and half-wrong. The 2^300 superposition exists, but you cannot extract 2^300 answers from it. You extract *one* measurement. The art is making sure that one measurement is meaningful — which only works when interference can be set up correctly. For most problems, it can't.

## Vietnamese

- Quantum state → trạng thái lượng tử
- Superposition → chồng chập (lượng tử)
- Interference → giao thoa lượng tử
- Amplitude → biên độ
- Quantum Fourier Transform → biến đổi Fourier lượng tử