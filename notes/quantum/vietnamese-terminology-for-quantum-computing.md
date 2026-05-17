---
title: "Vietnamese terminology for quantum computing"
date: 2026-05-17
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
| Measurement | Phép đo / Đo lường | |
| Wave function | Hàm sóng | |
| Collapse (of wavefunction) | Sụp đổ (hàm sóng) | |

## Phenomena and operations

| English | Vietnamese | Note |
|---|---|---|
| Interference | Giao thoa (lượng tử) | Inherited from classical wave physics |
| Constructive interference | Giao thoa cộng / Giao thoa tăng cường | |
| Destructive interference | Giao thoa triệt tiêu / Giao thoa hủy | |
| Amplitude | Biên độ | In quantum: probability amplitude |
| Phase | Pha | |
| Probability amplitude | Biên độ xác suất | |
| Decoherence | Mất kết hợp / Phân rã kết hợp | Often left in English |

## Gates and circuits

| English | Vietnamese | Note |
|---|---|---|
| Quantum gate | Cổng lượng tử | Standard |
| Quantum circuit | Mạch lượng tử | |
| Hadamard gate | Cổng Hadamard | Keep proper name |
| CNOT gate | Cổng CNOT / Cổng phủ định có điều khiển | |
| Pauli gates (X, Y, Z) | Cổng Pauli X, Y, Z | |
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