---
title: "Monomial polynomial term Vietnamese terminology breakdown"
date: 2026-05-17
captured: 2026-05-17T08:55:05.189Z
tags: ["math", "vietnamese-terms", "etymology", "quantum-computing"]
source: "Claude.ai chat"
---
Breakdown of Vietnamese mathematical terms `đơn thức`, `đa thức`, and `số hạng`, with etymology of the English `-nomial` suffix. Useful when reading Vietnamese math/quantum computing material where Hán Việt terms can be ambiguous.

## "Thức" in đơn thức / đa thức is NOT "công thức"

Common confusion: the `thức` in `đơn thức` (monomial) and `đa thức` (polynomial) is unrelated to `công thức` (formula).

- `Thức` (式) = Hán Việt for "biểu thức" / expression / a cluster of mathematical symbols
- It refers to a **term** (số hạng), not a **formula**

| Vietnamese | English | Example |
|------------|---------|---------|
| Đơn thức | Monomial | $3x^2$, $5xy$, $7$ |
| Đa thức | Polynomial | $3x^2 + 5x - 2$ |
| Công thức | Formula | $E = mc^2$ |

**Đơn thức** = a single term (only multiplication and exponents, no addition/subtraction between variables).
**Đa thức** = a sum of multiple monomials joined by + or -.

## Etymology of "-nomial"

The `-nomial` suffix in `monomial`/`binomial`/`polynomial` has a contested origin, but the OED-accepted path is:

**Main hypothesis (Latin):** From Latin `nomen` = "name". A monomial is "one named thing" (one term with its own identity); a polynomial is "many named things".

**Alternative hypothesis (Greek):** From Greek `nomos` (νόμος) = "part" or "division". Less commonly accepted.

**Historical path:** François Viète coined `binomial` in the 16th century, modeled on Latin `binominal` (having two names). `Trinomial` and `polynomial` followed the same template. So even if the deep root is Greek, the **actual entry into mathematics is via Latin `nomen`**.

| Prefix | Meaning | Example |
|--------|---------|---------|
| mono- | one | monomial: $3x^2$ |
| bi- | two | binomial: $x + y$ |
| tri- | three | trinomial: $x^2 + 2x + 1$ |
| poly- | many | polynomial: $x^3 + 2x^2 + x + 5$ |

`-nomial` = "named term".

## "Số hạng" decomposed

`Số hạng` is the Vietnamese term for **term** (English math vocab). Breakdown:

- `Số` (數) = number, quantity
- `Hạng` (項) = item, category, a separate part in a list

The character `項` originally meant "neck/nape", then extended to "separate item" (like the vertebrae of the neck being distinct sequential parts).

Familiar uses of `hạng`:

| Vietnamese | Meaning |
|------------|---------|
| Hạng mục | item, category |
| Hạng nhất, hạng nhì | rank 1, rank 2 |
| Điều khoản / hạng khoản | each clause, each item in a contract |

Core sense of `hạng`: **a separate, independent, countable part**.

## English equivalence: "term"

`Số hạng` = `term` in English math.

English "term" comes from Latin `terminus` = "boundary/limit", later extended to "a discrete unit with clear boundaries". Same logic as Vietnamese: **each term is an independent unit with clear separation from the others**.

## Worked example

For the polynomial $3x^2 + 5x - 7$, there are **3 terms (số hạng)**:

| Term | Value |
|------|-------|
| Term 1 | $3x^2$ |
| Term 2 | $5x$ |
| Term 3 | $-7$ |

Terms are **separated by + or - signs**. Those signs are the boundaries.

## Full Vietnamese math vocabulary

| Vietnamese | English | Meaning |
|------------|---------|---------|
| Số hạng | Term | A unit in a sum/difference |
| Thừa số | Factor | A unit in a product |
| Hệ số | Coefficient | The numeric multiplier on a variable |
| Bậc | Degree | The highest exponent |
| Biến | Variable | The letter symbol ($x, y, z$) |

For $3x^2$:
- It is **one term** (một số hạng)
- $3$ and $x^2$ are **two factors** (hai thừa số)
- $3$ is the **coefficient** (hệ số)
- $2$ is the **degree** (bậc) of this term
- $x$ is the **variable** (biến)

## Why this matters for quantum computing

Polynomials appear constantly in QC, so the vocabulary needs to be solid:

- **Polynomial-time algorithms**: Shor's algorithm runs in polynomial time $O((\log N)^3)$ vs exponential classically. This is why QC threatens RSA.
- **BQP (Bounded-error Quantum Polynomial time)**: the complexity class of problems solvable by a quantum machine in polynomial time with bounded error.
- **Hamiltonians as polynomials of Pauli operators**: e.g. Ising model Hamiltonian $H = \sum_i h_i Z_i + \sum_{ij} J_{ij} Z_i Z_j$ is a degree-2 polynomial in the $Z_i$ operators. Each $h_i Z_i$ or $J_{ij} Z_i Z_j$ is a monomial (đơn thức).

## Key takeaway

When reading Vietnamese math/QC material, mentally substitute:
- `thức` → "expression" (biểu thức), NOT "formula"
- `số hạng` → "term"
- `đơn thức` → "monomial" (one term)
- `đa thức` → "polynomial" (many terms)

If Vietnamese terminology causes friction, read QC material directly in English. Vietnamese math translation is sparse and sometimes ambiguous, and you already read technical English fluently.

## Related

- [[vietnamese-terminology-for-quantum-computing]] - quantum-side companion glossary; pair these two when reading Vietnamese QC material
- [[what-polynomial-time-actually-means]] - polynomial-time as a complexity-class threshold, the QC application of "đa thức"
- [[complexity-classes-p-np-bqp-qma-explained]] - where "polynomial" shows up in BQP / BPP / P
- [[history-and-motivation-of-major-quantum-algorithms]] - Shor's `O((log N)^3)` is the canonical "polynomial-time quantum" example