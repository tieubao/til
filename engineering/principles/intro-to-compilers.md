---
title: "an intro to compilers"
date: 2017-08-14
captured: 2017-08-14T15:48:48Z
tags: [compilers, cs-fundamentals, llvm]
source: "GitHub issue tieubao/til#324 + https://nicoleorchard.com/blog/compilers"
aliases: []
status: refined
---

## Context

Nicole Orchard walks through how compilers work using a simple C program compiled through LLVM/Clang. The article demystifies the three-phase architecture that most modern compilers follow.

**Source:** [An Intro to Compilers](https://nicoleorchard.com/blog/compilers)

**Attachment:** [An intro to compilers.pdf](https://github.com/tieubao/til/files/1222704/An.intro.to.compilers.pdf)

## What a compiler does

A compiler translates source code into executable machine code. Some compilers (transpilers) translate between programming languages instead. The core job is bridging the gap between human-readable code and hardware instructions.

## Three-phase architecture

### Frontend

Converts source code into an intermediate representation (IR). This phase includes:

- **Preprocessor** - handles directives like `#include`
- **Lexer** - breaks source text into tokens
- **Parser** - builds an abstract syntax tree from tokens
- **Semantic analyzer** - checks types and validates meaning
- **IR generator** - produces the intermediate representation

Example tool: `clang` for C-family languages.

### Optimizer

Analyzes IR and produces more efficient versions. Optimizations include:

- Loop unrolling
- Constant folding (pre-computing known values)
- Dead code elimination
- Function inlining (e.g., replacing `printf()` with `puts()` when formatting is not needed)

The optimizer works on IR, which means it is language-agnostic. The same optimizer serves C, C++, Rust, and Swift through LLVM.

### Backend

Generates machine code from optimized IR through three steps:

1. **Instruction selection** - maps IR operations to CPU instructions
2. **Register allocation** - assigns variables to hardware registers
3. **Instruction scheduling** - orders instructions for pipeline efficiency

Example tool: `llc` for x86 architecture.

## Key takeaway

The three-phase design (frontend, optimizer, backend) is what makes LLVM so powerful. New languages only need a new frontend. New hardware only needs a new backend. The optimizer is shared. This separation of concerns enables the modern polyglot ecosystem where dozens of languages target the same optimized machine code.

## Related
