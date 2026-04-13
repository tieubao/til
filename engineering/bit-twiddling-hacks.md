---
title: "Bit Twiddling Hacks"
date: 2021-05-25
captured: 2026-04-13T00:00:00Z
tags: [better-dev, bit-manipulation, algorithms, cs-fundamentals]
source: "GitHub issue tieubao/til#552 + https://graphics.stanford.edu/~seander/bithacks.html"
aliases: []
status: refined
---

## Context

Sean Eron Anderson's comprehensive reference of bit manipulation techniques collected over many years at Stanford. These are low-level tricks that avoid branching, replace expensive operations (division, modulus) with bitwise equivalents, and solve common problems in fewer instructions. Essential reading for systems programming, competitive programming, and understanding how hardware-aware code works.

**Source:** [Bit Twiddling Hacks](https://graphics.stanford.edu/~seander/bithacks.html)

## Key categories

### Sign and comparison operations

Extracting sign bits via right-shift, detecting opposite signs through XOR, and computing absolute values without branching. Useful on architectures where conditional jumps are expensive.

### Bit counting and parity

Multiple approaches to count set bits (popcount). Brian Kernighan's method iterates only as many times as there are set bits, making it efficient for sparse data. Parallel methods using magic binary constants achieve results in constant time. Lookup tables offer a good speed-to-memory tradeoff.

### Bit reversal and rotation

Ranges from obvious loops to lookup tables and parallel algorithms. One approach uses 64-bit multiply and modulus division to reverse byte bits in just 3 operations.

### Finding highest/lowest set bits

Methods to locate the most or least significant bit position, essential for fast logarithm calculations. De Bruijn sequence multiplication offers an elegant solution. Lookup tables provide predictable performance.

### Modulus and division shortcuts

Specialized techniques for divisors of form 2^n or 2^n-1, avoiding expensive division instructions through masking and shift operations.

### Trailing zero counting

Implementations range from linear scans to parallel binary search approaches, including float-casting tricks and multiply-based methods.

### Morton numbers (interleaving)

Combining two integers into linearized 2D coordinates by interleaving their bits. Useful for spatial indexing (Z-order curves) and cache-friendly memory access patterns.

### Byte-level word operations

"Determine if a word has a zero byte" in only 5 operations. Methods for finding bytes matching specific value ranges within larger words, useful for string processing without per-byte loops.

## How to spot this

When you need to optimize tight loops in systems code, replace branching with arithmetic, or implement spatial indexing. Most application code should not reach for these tricks, but knowing they exist helps you read performance-critical codebases.

## Related

- [[zen-of-python]] - contrast: "readability counts" vs. raw performance tricks
