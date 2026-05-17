---
title: "History and motivation of major quantum algorithms"
date: 2026-05-17
captured: 2026-05-17T12:27:37.551Z
tags: ["quantum", "history", "algorithms", "shor", "grover"]
source: "Claude.ai chat"
---
The history of quantum algorithms is a story of outsiders. Each major breakthrough came from someone who imported intuition from a different field, not from a quantum-physics specialist working inside the consensus. Reading the human stories explains why the algorithms look the way they do better than reading the math does.

![Quantum algorithms timeline](https://assets.han-ws.workers.dev/i/2026/05/quantum-algorithms-timeline.svg)

## The setup (1982 – 1993)

Richard Feynman proposed quantum computing in 1982 with a specific motivation: classical computers cannot efficiently simulate quantum systems, but a quantum computer could. David Deutsch formalized the model with the Quantum Turing Machine in 1985.

For the next decade, quantum computing was treated as science fiction in mainstream computer science. Results existed (Deutsch-Jozsa 1992, Bernstein-Vazirani 1993, Simon 1994) but all solved artificial problems with no real-world relevance. The cryptography community ignored quantum computing entirely.

## Shor (1994): the combinatorist who reframed factoring

**Setting.** Peter Shor was at Bell Labs, AT&T's industrial research paradise. Bell Labs had produced the transistor, the laser, C, Unix. Mathematicians and physicists got high salaries and freedom to chase whatever they wanted.

**Background.** Shor was a combinatorist, not a quantum physicist. He taught himself quantum mechanics by reading Deutsch and Bernstein-Vazirani.

**The leap.** Daniel Simon (also at Bell Labs) published an algorithm in 1994 that solved a contrived problem (find a hidden period `s` such that f(x) = f(x ⊕ s)) exponentially faster than classical. Shor read Simon's paper and asked: *is there a real-world problem with this same hidden-period structure?*

He found two:
1. **Discrete logarithm**: solved in a few weeks.
2. **Integer factorization**: Shor knew an old number-theoretic result (Miller, 1970s) that reduces factoring to finding the period of a^x mod N. That was the bridge.

Total time from "read Simon" to "have a polynomial factoring algorithm": about two months.

**Reaction.** When Shor presented at FOCS 1994, the room exploded. RSA, the backbone of modern commerce and government communication, had just received a theoretical death sentence. The NSA paid attention immediately. Quantum computing funding multiplied within a year. Before Shor, the field had dozens of researchers. After Shor, thousands.

**Shor's own framing.** "I'm not a physicist. I wasn't bound by the assumption that quantum computers are just for simulating physics. I looked at it as a new computational device and asked what mathematical problems it could solve."

## Grover (1996): the signal-processing engineer who weaponized cancellation

**Setting.** Also Bell Labs, same building as Shor. Lov Grover trained at IIT Delhi then Stanford, with a PhD in electrical engineering.

**Background.** Grover came from radar and signal processing, not computer science. He was fluent in amplitudes, phases, and interference patterns because that was his daily bread.

**The leap.** Grover didn't start by asking "how do we search faster?" He started by asking: *quantum mechanics has signed amplitudes. In signal processing, we use destructive interference to filter noise. Can we use destructive interference to cancel wrong answers in a search?*

That's a signal-processing engineer's intuition, not a CS theorist's. The algorithm fell out:
1. Start with uniform superposition over N candidates.
2. Use an oracle to flip the sign of the right answer's amplitude.
3. Reflect all amplitudes through their mean: the "diffusion" step.
4. Repeat √N times. The right answer's amplitude grows; the wrong answers' shrink.

**Reaction and the bitter truth.** A quadratic speedup (N → √N) is less dramatic than Shor's exponential. The initial response was muted. Over time, it became clear how important Grover was: search is a primitive for hundreds of other algorithms; Grover applies to any NP problem; it halves the effective security of AES-256.

The bitter truth: Grover was later proven **optimal**. No quantum algorithm can do unstructured search faster than √N. This is both a beautiful theorem and a cap on the dream that quantum gives exponential speedups for everything.

## HHL (2008): the linear-systems gamble

**Setting.** By 2008, quantum algorithm research had stagnated. A decade passed with no new major algorithm. Some questioned whether Shor and Grover were the only big results.

**Background.** Aram Harrow, Avinatan Hassidim, and Seth Lloyd at MIT. Seth Lloyd was a quirky generalist: studied philosophy, switched to physics, wrote pop-science books.

**The leap.** They asked a pragmatic question: *what classical problem is so ubiquitous that speeding it up would change everything?* Answer: solving linear systems Ax = b. ML, simulation, economics; everything reduces to this.

The insight: if you don't need the full solution vector x, only a scalar quantity of it (like x^T M x), quantum can do it in O(log N) instead of O(N²). Exponential speedup.

**The twist.** The HHL paper became the most-cited paper in quantum machine learning. But four caveats turned out to be the entire story:

1. Matrix A must be sparse and well-conditioned.
2. Vector b must be loadable via qRAM (which doesn't exist).
3. Only scalar quantities of x can be extracted, not the full vector.
4. A must be presented as an oracle, not raw data.

**Dequantization (2018).** Ewin Tang, an 18-year-old undergraduate at UT Austin, discovered that under the same access assumptions HHL requires, a *classical* algorithm achieves the same complexity. Scott Aaronson had given Tang the recommendation-systems variant of HHL as a thesis topic, expecting it would prove the quantum advantage. Tang proved the opposite: the speedup was an artifact of the unrealistic assumptions, not the quantum hardware.

This reshaped the entire field of quantum machine learning. The lesson: in quantum, the I/O assumptions matter as much as the algorithm.

## VQE and QAOA (2014): the pragmatic pivot

**Setting.** The NISQ era is becoming reality: real quantum machines exist but have 5 to 50 qubits and are extremely noisy. Shor needs millions of stable qubits. The gap between theory and hardware is a chasm.

The field splits:
- **Theorists.** Keep designing beautiful algorithms; hardware will catch up.
- **Pragmatists.** Design algorithms that accept noisy hardware right now.

**The people.** Alán Aspuru-Guzik at Harvard (quantum chemist focused on molecules and batteries). Edward Farhi at MIT (older statesman of the field, working on adiabatic computing since 2000).

**VQE (Aspuru-Guzik & Peruzzo et al, 2014).** Quantum Phase Estimation, the "correct" way to simulate molecules, needs millions of gates, impossible on real hardware. The insight: *why force everything onto quantum? Share the work with classical.* VQE runs a quantum subroutine to estimate energy, then a classical optimizer adjusts parameters. Iterate. Quantum does what it's good at (preparing complex states), classical does what it's good at (optimization).

**QAOA (Farhi, 2014).** Pushed the same idea to combinatorial optimization (Max-Cut, scheduling). The philosophical twist: QAOA does not guarantee optimal solutions, just "good enough". Classical quantum algorithms always promised "optimal or near-optimal". QAOA said: *accept approximation, in exchange for running on real hardware*.

**Reaction.** VQE and QAOA became the workhorses of 2015-2024 quantum computing. Every quantum hardware company (IBM, Google, Rigetti, IonQ) used them for demos.

**The 2022-2024 reality check.** A series of results showed:
- QAOA is matched or beaten by classical algorithms on most benchmarks.
- VQE suffers from barren plateaus: exponentially flat loss landscapes.
- Many "quantum advantage" demos are benchmark cherry-picking.

This is the current state. A small "quantum winter" in pragmatic research while the field figures out what NISQ-era algorithms actually deliver.

## Patterns that repeat

**1. Outsiders solve insider problems.** Shor (combinatorist), Grover (signal processing engineer), Aspuru-Guzik (chemist). Quantum computing advances not by deeper quantum specialists, but by people who carry tools across domains.

**2. Speedup requires structure.** QFT (Shor), amplitude amplification (Grover), phase estimation (HHL) all exploit periodicity, symmetry, or eigenstructure. **Problems without exploitable structure get no exponential quantum speedup.** This is a hard ceiling.

**3. Caveats are the whole story.** HHL had caveats → dequantized. VQE/QAOA have caveats (barren plateau, classical competition) → being benchmark-beaten. Whenever a paper claims "exponential speedup", the I/O assumptions are where the load-bearing wall is.

**4. Funding follows geopolitics, not science.**
- Post-Shor 1994: NSA money flowed in because of cryptography fear.
- Post-2016: China invested heavily in quantum communication.
- 2020: US National Quantum Initiative Act, $1.2B.
- 2026: US-China quantum supremacy race is heating up.

Money is driven by post-quantum cryptography concerns and military advantage, not by scientific breakthroughs.

## Connection to other domains

**To AI agent design.** The Hermes Agent constraint pattern ("no autonomous external actions, hard constraints, human-in-the-loop") mirrors VQE's hybrid model. Don't try to push everything onto the new technology; carve out what it does best and keep the rest classical.

**To quantitative finance.** Black-Scholes (1973) was a "Shor moment" for finance: a physicist (Black) and a mathematician (Scholes, Merton) imported stochastic calculus from physics into option pricing. Same pattern: outsiders, structure exploitation, reframing the problem so it becomes solvable.

## Related

- [[state-preparation-is-half-the-quantum-algorithm]] - companion note tracing each algorithm here through its preparation bottleneck
- [[quantum-superposition-state-and-qft-for-beginners]] - the QFT engine inside Shor and HHL
- [[complexity-classes-p-np-bqp-qma-explained]] - the class membership claims each of these algorithms triggered
- [[why-quantum-computing-talks-about-decision-problems]] - why these algorithms are analyzed in decision-problem terms
- [[optimization-as-the-bridge-to-computational-finance]] - finance / VQE parallel (variational, hybrid quantum-classical optimization)
- [[hermes-agent-comprehensive-briefing-april-2026]] - the Hermes constraint pattern the "to AI agent design" tie-back refers to