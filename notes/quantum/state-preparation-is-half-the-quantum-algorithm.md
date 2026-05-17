---
title: "State preparation is half the quantum algorithm"
date: 2026-05-17
captured: 2026-05-17T12:26:37.414Z
tags: ["quantum", "algorithms", "state-preparation"]
source: "Claude.ai chat"
---
Every quantum algorithm has the same three-stage skeleton: state preparation, quantum operation, measurement. The operation stage gets all the headlines (QFT, amplitude amplification, phase estimation), but state preparation is where the design problem actually lives. Most quantum "exponential speedup" claims die at the state preparation step.

![State preparation across quantum algorithms](https://assets.han-ws.workers.dev/i/2026/05/state-preparation-comparison.svg)

## The three-stage skeleton

1. **State preparation.** Encode the problem into a quantum state. This is where the bottleneck usually is.
2. **Quantum operations.** Apply gates, oracles, QFT, amplification: the part that gets the speedup.
3. **Measurement.** Collapse to a classical result and post-process.

The operation stage is generic across many algorithms. The preparation stage is where each algorithm has to invent something custom for its problem.

## Four algorithms, four preparations

### Shor (factoring)

- **Preparation.** Hadamard gates on the input register to create a uniform superposition of x. Then apply the function f(x) = a^x mod N via modular exponentiation circuits. State becomes Σ |x⟩|a^x mod N⟩.
- **Operation.** Quantum Fourier Transform to extract the period r.
- **Bottleneck.** Modular exponentiation circuit depth. For RSA-2048, this is millions of gates.

### Grover (search)

- **Preparation.** Just Hadamard on every qubit: uniform superposition of all N possibilities. Simplest preparation of any major algorithm.
- **Operation.** Repeat (oracle + diffusion) ~√N times.
- **Bottleneck.** Designing the oracle for the specific search criterion. The oracle itself is the algorithm's hidden complexity.

### HHL (solving Ax = b)

- **Preparation.** Encode the vector b as the amplitudes of a quantum state. Requires qRAM, which does not yet exist in practical form.
- **Operation.** Quantum Phase Estimation, then controlled rotation to apply A⁻¹.
- **Bottleneck.** Loading b kills the speedup. In 2018, Ewin Tang showed a classical algorithm with the same complexity if the same access assumptions hold. HHL's "exponential speedup" depended on caveats that turned out to be load-bearing.

### VQE (quantum chemistry)

- **Preparation.** Parameterized ansatz circuit: a variational form where parameters are tuned by a classical optimizer. Uses encodings like Jordan-Wigner to map molecular orbitals to qubits.
- **Operation.** Hybrid quantum-classical loop. Quantum measures expectation, classical optimizer adjusts parameters.
- **Bottleneck.** Barren plateau problem: the loss landscape becomes exponentially flat, gradient descent cannot find a direction.

## Why state preparation is the hard part

**1. Encoding efficiency.** For any exponential speedup to survive, state preparation must take time poly(log N). If preparation takes O(N), you lose all the gains. HHL's classical dequantization happened exactly because the preparation step was the load-bearing wall.

**2. Problem structure dictates encoding.** Each problem domain needs its own encoding strategy:
- Number-theoretic problems → modular arithmetic encoding (Shor)
- Unstructured search → uniform superposition (Grover)
- Quantum physics → physically-motivated wavefunction encoding (VQE)
- Graph optimization → vertex/edge encoded as qubit interactions (QAOA)

**3. The quantum-data input problem.** Most real-world data is classical (databases, CSV files, logs). Loading it into a quantum register requires either qRAM (which doesn't exist at scale) or per-call encoding (which is slow). This is the biggest practical reason quantum hasn't displaced classical ML: loading the data is slower than training the classical model.

## Code mental model

```python
# Every quantum algorithm has this structure
def quantum_algorithm():
    state = prepare_state()      # <- the hard, problem-specific part
    apply_operations(state)       # <- the headline algorithm
    return measure(state)         # <- collapse + post-process

# Each algorithm has its own preparation
def prepare_state_for_shor(N, a):
    hadamard_all(register_1)
    modular_exp(register_1, register_2, a, N)

def prepare_state_for_grover(N):
    hadamard_all(register)  # That's it

def prepare_state_for_vqe(molecule):
    jordan_wigner_mapping(molecule)
    apply_ansatz(parameters)  # Many parameters, classical optimizer tunes them

def prepare_state_for_hhl(A, b):
    encode_vector_as_amplitudes(b)  # Requires qRAM
    # ... preparation may dominate the runtime
```

## Takeaway

When you read "exponential quantum speedup", ask three questions:

1. What is the state preparation cost? If it's O(N) or worse, the speedup is gone.
2. What does the algorithm assume about data access? Does it need qRAM?
3. How much information does the measurement give? If it's a single scalar, that may already be possible classically.

The headlines focus on the operation stage, but the engineering reality lives in the preparation stage. Half the design work for a useful quantum algorithm is figuring out how to encode the problem efficiently, and most candidate algorithms fail at this step before they ever get to the "exciting" quantum part.

## Related

- [[history-and-motivation-of-major-quantum-algorithms]] - origin story for Shor, Grover, HHL, VQE; this note slices them by preparation cost instead
- [[quantum-superposition-state-and-qft-for-beginners]] - the superposition primitive that all four preparations build on
- [[why-quantum-computing-talks-about-decision-problems]] - the measurement model that determines what preparation has to achieve
- [[complexity-classes-p-np-bqp-qma-explained]] - why the BQP claim depends on preparation cost being poly(log N)
- [[vietnamese-terminology-for-quantum-computing]] - "chuẩn bị trạng thái" and related vocabulary