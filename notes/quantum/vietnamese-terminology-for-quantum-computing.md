---
title: "Vietnamese terminology for quantum computing"
date: 2026-05-17
updated: 2026-05-19
captured: 2026-05-17T12:24:49.130Z
tags: ["quantum", "vietnamese", "glossary"]
source: "Claude.ai chat"
---
A working glossary for quantum computing terms in Vietnamese. Most specialized literature in Vietnam still keeps the English originals because the field is small and tied to global papers. This note maps the standard translations where they exist, and flags where convention is split.

## Foundational concepts

| English | Vietnamese | Note |
|---|---|---|
| Qubit | Qubit / Bit lượng tử | Usually kept as "qubit" |
| Classical bit | Bit cổ điển | |
| Quantum state | Trạng thái lượng tử | Standard |
| Superposition | **Chồng chập** (lượng tử) | "Xếp chồng" appears in popular media but is non-standard |
| Entanglement | **Vướng víu** lượng tử / Rối lượng tử | Both used; "vướng víu" is more literal, "rối" is more common in popular writing |
| Maximally entangled | Vướng víu cực đại | Bell states are maximally entangled (cannot be more entangled for 2 qubits) |
| Separable state | Trạng thái tách được | Opposite of entangled; decomposes as tensor product of single-qubit states |
| Bell state | Trạng thái Bell | Canonical maximally entangled 2-qubit state. Four of them: $\Phi^\pm, \Psi^\pm$ |
| Bell basis | Cơ sở Bell | Orthonormal basis of 2-qubit space made of the four Bell states |
| ebit (entangled bit) | ebit (or "bit vướng víu") | Unit of entanglement resource. 1 Bell pair = 1 ebit. Currency in quantum protocol accounting |
| Non-local correlation | Tương quan phi định xứ | Correlation derivable from Bell state with multi-axis measurement, NOT reproducible classically. The genuine quantum feature |
| Measurement | Phép đo / Đo lường | |
| Born rule | Quy tắc Born | $P_i = |\alpha_i|^2$. The bridge from amplitude to probability |
| Wave function | Hàm sóng | |
| Collapse (of wavefunction) | Sụp đổ (hàm sóng) | |

## Phenomena and operations

| English | Vietnamese | Note |
|---|---|---|
| Interference | Giao thoa (lượng tử) | Inherited from classical wave physics |
| Amplitude interference | Giao thoa biên độ | Specifically: signed amplitudes can cancel; the engine of quantum advantage |
| Constructive interference | Giao thoa cộng / Giao thoa tăng cường | |
| Destructive interference | Giao thoa triệt tiêu / Giao thoa hủy | |
| Amplitude | Biên độ | In quantum: probability amplitude |
| Phase | Pha | |
| Phase flip | Lật pha | Z gate's action: $\ket{1} \to -\ket{1}$. Single-axis measurement can't detect it |
| Probability amplitude | Biên độ xác suất | |
| Reflection (as quantum gate) | Phép phản chiếu (lượng tử) | Linear operation with det = −1. X, Z, H are reflections |
| Rotation (as quantum gate) | Phép quay (lượng tử) | Linear operation with det = +1. $R(\theta)$ family |
| Self-inverse / Involutive | Tự nghịch đảo | Gate² = identity. Reflections are self-inverse; rotations generally are not |
| Anticommute | Phản giao hoán | $AB = -BA$. Pauli matrices pairwise anticommute |
| Decoherence | Mất kết hợp / Phân rã kết hợp | Often left in English |

## Gates and circuits

| English | Vietnamese | Note |
|---|---|---|
| Quantum gate | Cổng lượng tử | Standard |
| Quantum circuit | Mạch lượng tử | |
| Hadamard gate | Cổng Hadamard | Keep proper name; pronounced HA-da-mard or French "a-đa-ma" |
| CNOT gate | Cổng CNOT / Cổng phủ định có điều khiển | "see-not" |
| CCNOT / Toffoli gate | Cổng CCNOT / Cổng Toffoli | "see-see-not" or "Toffoli". 3-qubit gate. Universal for classical reversible computation |
| Fredkin / CSWAP gate | Cổng Fredkin / SWAP có điều khiển | 3-qubit reversible swap |
| Pauli gates (X, Y, Z) | Cổng Pauli X, Y, Z | Pronounce Pauli as POW-lee |
| Pauli matrices | Ma trận Pauli | Same matrices viewed as observable operators |
| Bell-state measurement | Đo trạng thái Bell | Inverse Bell circuit + computational measurement. Primitive for teleportation |
| Quantum Fourier Transform (QFT) | Biến đổi Fourier lượng tử | |
| Oracle | Tiên tri (lượng tử) / Oracle | Usually kept in English |

## Algorithms and techniques

| English | Vietnamese | Note |
|---|---|---|
| Quantum algorithm | Thuật toán lượng tử | |
| Shor's algorithm | Thuật toán Shor | Keep proper name |
| Grover's algorithm | Thuật toán Grover | Keep proper name |
| Amplitude amplification | Khuếch đại biên độ | |
| State preparation | Chuẩn bị trạng thái | |
| Quantum Phase Estimation (QPE) | Ước lượng pha lượng tử | |
| VQE | Bộ giải trị riêng lượng tử biến phân | Usually kept as "VQE" |
| Period finding | Tìm chu kỳ | |
| Modular exponentiation | Lũy thừa modular / Lũy thừa đồng dư | |

## Complexity classes

| English | Vietnamese | Note |
|---|---|---|
| Complexity class | Lớp độ phức tạp | |
| Polynomial time | Thời gian đa thức | |
| Exponential time | Thời gian mũ | |
| Sub-exponential | Dưới mũ / Á mũ | |
| Bounded-error | Sai số giới hạn | |
| Tractable / Intractable | Khả thi / Không khả thi | |

## Hardware and experiment

| English | Vietnamese | Note |
|---|---|---|
| Quantum computer | Máy tính lượng tử | |
| Quantum supremacy | Ưu thế lượng tử | Google adopted this term in 2019 |
| Quantum advantage | Lợi thế lượng tử | More neutral than "supremacy" |
| NISQ | Lượng tử trung quy mô có nhiễu | Usually kept as "NISQ" |
| Error correction | Sửa lỗi (lượng tử) | |
| Fault-tolerant | Chịu lỗi | |
| qRAM | RAM lượng tử | Usually kept as "qRAM" |
| Bell test | Kiểm tra Bell | Lab experiment to demonstrate Bell inequality violation |
| CHSH inequality | Bất đẳng thức CHSH | Operational form of Bell's inequality (4 measurement settings) |
| Tsirelson bound | Cận Tsirelson | Quantum upper bound for CHSH: $2\sqrt{2}$ |
| Loophole-free Bell test | Kiểm tra Bell không lỗ hổng | Closes locality + detection + freedom-of-choice loopholes. First: Hensen 2015 |
| DiVincenzo criteria | Tiêu chí DiVincenzo | 5 hardware requirements for a quantum computer (DiVincenzo 2000) |

## Post-quantum cryptography

| English | Vietnamese | Note |
|---|---|---|
| Post-quantum cryptography | Mật mã hậu lượng tử | Standard |
| Quantum-resistant | Kháng lượng tử | |
| Lattice-based cryptography | Mật mã dựa trên dàn / Mật mã lưới | |
| Hash-based signatures | Chữ ký dựa trên hàm băm | |
| Discrete logarithm | Logarit rời rạc | |
| Integer factorization | Phân tích thừa số nguyên | |

## Usage conventions

**"Lượng tử" as a modifier.** Most concepts are formed by appending "lượng tử" (quantum) to the base noun: computing → tính toán lượng tử, algorithm → thuật toán lượng tử, gate → cổng lượng tử, state → trạng thái lượng tử.

**"Chồng chập" vs "xếp chồng" for superposition.** "Chồng chập" is the standard in Vietnamese physics textbooks, inherited from the superposition principle in classical wave mechanics. "Xếp chồng" appears in popular-science articles but is not the academic convention.

**"Vướng víu" vs "rối" for entanglement.** Both are used. "Vướng víu lượng tử" is closer to the literal meaning of "entanglement" and is more common in technical material. "Rối lượng tử" is shorter and dominates in popular books and translated literature.

**When to keep English.** In practice, specialists keep the English form for:
- Algorithm names: Shor, Grover, VQE, QAOA, HHL
- Concepts without a stable translation: oracle, ansatz, decoherence
- Acronyms: QFT, QPE, BQP, NISQ

**Safe convention when writing.** On first mention, give both forms: "chồng chập lượng tử (quantum superposition)" or "thuật toán Shor (Shor's algorithm)". Afterwards, pick one and stay consistent.

## Symbol pronunciation (Greek, ket/bra, operators)

This note covers term translations (English ↔ Vietnamese). For **how to pronounce** individual math symbols out loud (ψ as "pờ-sai", ⊗ as "ten-sơ", † as "đa-gơ", $\ket{\Phi^+}$ as "kết phi-cộng", etc.), see the symbol pronunciation guide in the private working-track companion.

## Related

- [[monomial-polynomial-term-vietnamese-terminology-breakdown]] - companion glossary for the math foundations (đơn thức, đa thức, số hạng) underlying complexity-class language
- [[quantum-superposition-state-and-qft-for-beginners]] - the concepts (chồng chập, giao thoa, biên độ) defined here in working context
- [[state-preparation-is-half-the-quantum-algorithm]] - "chuẩn bị trạng thái" applied across four algorithms
- [[complexity-classes-p-np-bqp-qma-explained]] - the English class names this note glosses
- [[what-polynomial-time-actually-means]] - "thời gian đa thức" as the threshold this glossary inherits
- [[history-and-motivation-of-major-quantum-algorithms]] - the algorithm names (Shor, Grover, VQE, QAOA, HHL) the glossary mostly leaves in English