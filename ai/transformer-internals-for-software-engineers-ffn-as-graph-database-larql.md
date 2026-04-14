---
title: "Transformer internals for software engineers — FFN as graph database (LARQL)"
date: 2026-04-14
captured: 2026-04-14T05:03:34.910Z
tags: ["ai", "llm", "transformer", "architecture"]
source: "Claude Code session - larql repo walkthrough"
---
A software-engineer's mental model of how transformers work, why attention was invented, and how the LARQL project reframes the FFN as a queryable graph database. Written for someone comfortable with data structures, databases, graphs, mmap, and BLAS — but new to LLMs.

## What an LLM actually computes

Forget "AI." An LLM is a **pure function**:

```
next_token_probabilities = model(token_sequence)
```

Input: a list of integers (token IDs, e.g. `[1996, 3007, 1997, 2605, 2003]` = "The capital of France is"). Output: a probability distribution over ~262,000 possible next tokens. Pick the top one, append it, call the function again. That's "generation."

The function is ~16 GB of floating-point numbers (the *weights*) plus a fixed sequence of arithmetic operations (the *architecture*). No branching on data, no loops over unknown lengths. It's basically a giant pure expression evaluated over arrays. Think: a compiled shader, not a program with control flow.

### The residual stream

A transformer is N identical blocks stacked (Gemma-3-4B has N=34). Each block takes a `[seq_len, hidden_size]` matrix of floats (here `hidden_size=2560`) and returns one of the same shape. This matrix is called the **residual stream** — a mutable buffer flowing through the pipeline, where each block *adds* its contribution:

```python
x = embed(tokens)            # [seq, 2560]
for block in blocks:         # 34 times
    x = x + attention(x)     # blocks add to the stream
    x = x + ffn(x)
logits = x @ embed.T         # project back to vocab → 262K scores
```

The `x + attention(x) + ffn(x)` pattern is why it's called "residual" — each layer is a diff against the running state. Close to a middleware chain where each middleware mutates a shared context object by *adding* to it.

Two sub-modules per block:
- **Attention**: look at other tokens in the sequence and mix in information from them.
- **FFN (feed-forward network)**: independently transform each token's vector through a 2-layer MLP. **This is what LARQL reframes as a graph.** Attention is left alone.

## Why attention exists (the motivation)

Imagine writing a function that predicts the next word. To predict `Paris` from "The capital of France is", the model needs to know that `France` (4 positions back) is the relevant entity AND that `capital` tells it what to fetch about France. **Information from earlier positions has to reach the last position.** How?

### Attempt 1: concatenate everything
Flatten all token vectors, run an MLP. Problem: input size depends on sequence length. MLPs have fixed-size inputs. Dead end.

### Attempt 2: RNN (pre-2017)
Process tokens one at a time, carry a hidden state forward:

```python
state = zeros()
for token in tokens:
    state = f(state, token)   # sequential
```

Like `reduce()` over the sequence. Two fatal problems:

1. **Sequential.** Position 5 waits for 1, 2, 3, 4. GPUs hate this.
2. **Lossy.** Information from position 1 gets squashed through `f` four times. Long-range dependencies fade. Telephone game.

### Attempt 3: attention
Each token produces three vectors from its residual:
- **Query** (`Q`): what am I looking for?
- **Key** (`K`): what do I offer as a lookup key?
- **Value** (`V`): what payload do I return if matched?

Then for every position `i`:

```python
scores = [ dot(Q[i], K[j]) for j in range(seq_len) ]  # how well does j answer i?
weights = softmax(scores)                              # normalize to sum=1
output[i] = sum(weights[j] * V[j] for j in range(seq_len))
```

**Software analogy:** a `Map<Vec, Vec>` where `get(query)` doesn't return one value — it returns a weighted blend of all values, weighted by how similar each stored key is to your query. A soft, differentiable, content-addressed hash-map lookup.

### Why they had to invent it

1. **Parallelism.** All positions compute Q, K, V independently. The score matrix is one matmul: `Q @ K.T`. GPUs love this.
2. **Direct access.** Position 5 reads position 1 in one hop, not four. No telephone game.
3. **Content-addressed, not position-addressed.** The model learns to look up by meaning ("find the country name"), not by offset ("look 4 back").

The 2017 paper was literally titled "Attention Is All You Need."

## What the FFN actually is

Given the residual vector `x` (size 2560) at one token position:

```
h = activation(x @ W_gate) * (x @ W_up)    # expand: 2560 → 10240
y = h @ W_down                              # contract: 10240 → 2560
return y                                    # added back to residual
```

Three weight matrices:
- `W_gate`: `[10240, 2560]` — 10,240 rows, each a 2560-dim "detector"
- `W_up`: same shape
- `W_down`: `[10240, 2560]` — 10,240 columns, each a 2560-dim "writer"

The activation (SiLU, GeGLU) is near-zero for most inputs, so **most of the 10,240 rows produce ~0 contribution**. Only a handful "fire" per token. This sparsity is the key insight.

### LARQL's renaming

- Each of the 10,240 rows of `W_gate` → a **feature** = a graph node / edge key. It fires when the residual looks like `(entity + relation)`.
- The matching column of `W_down` → the **edge payload**, a vector that nudges the output toward a specific target token.
- 34 layers × 10,240 features ≈ **348,160 edges**.

The FFN is literally: "for each token, find the few detectors that fire, add their payloads to the stream." That's **KNN lookup + sum**, not a matmul in spirit — the matmul was just how GPUs happened to compute it.

## Weights on disk

A model ships as **tensors** (n-dim float arrays) in a container format:
- **safetensors** (HuggingFace): header JSON + mmap-friendly raw float blobs
- **GGUF** (llama.cpp): similar, with optional quantization

Loading a model = mmap these files, reinterpret bytes as `f32`/`f16`, pass to the forward pass. No magic.

A **.vindex** (LARQL's format) is the same weights **reorganized**:

```
gemma3-4b.vindex/
  gate_vectors.bin    # W_gate rows, contiguous & indexed for KNN
  embeddings.bin      # token→vector table
  down_meta.bin       # per-feature metadata: feature F fires, best target token = T
  index.json          # schema, layer counts, provenance
  tokenizer.json
  relation_clusters.json  # auto-discovered: features that share a relation type
  feature_labels.json     # human-readable: F8821 = capital-of
```

A read-optimized index over the same numbers. Like Postgres having heap tables + a separate B-tree: the model weights are the heap, the vindex is the B-tree.

## Why this is a "graph database"

- **Nodes**: tokens/entities (rows of the embedding matrix, ~262K).
- **Edges**: features (rows of gate vectors across all layers, ~348K). Each edge has a detected `(entity, relation)` firing pattern and a target token it pushes toward.
- **Edge labels**: discovered by clustering features that behave similarly → `capital-of`, `language`, `borders`.

`DESCRIBE "France"` = "find all features whose gate vector strongly correlates with France's embedding, group by discovered relation cluster, list targets." Graph traversal over the weights. **No inference required to browse.**

## KNN walk = matmul (inference without matmul)

**Dense FFN.** Compute `x @ W_gate` (matmul against all 10,240 rows), apply activation (most outputs → 0), multiply, project through `W_down`. You did 10,240 dot products to use ~50.

**Walk FFN.** KNN-search `W_gate` for top-k rows that dot-product highest with `x`. For each hit, apply activation, fetch its `W_down` column, add weighted contribution. Same output, fewer operations.

LARQL's benchmark on Gemma 3 4B: Walk 517ms vs Dense 535ms per forward pass. Walk is slightly *faster* because memory bandwidth on `W_down` is the bottleneck and walk only touches the columns it needs (mmap'd, zero-copy).

## INSERT / COMPILE: editing knowledge without retraining

To add "Atlantis → capital-of → Poseidon":

1. Find a free feature slot `F` in some late layer (knowledge lives in layers ~22–27, determined empirically).
2. Synthesize a gate vector that dot-products high with `(Atlantis_embedding + capital-of_direction)` and ~0 with everything else.
3. Synthesize a down vector that rotates the residual toward `Poseidon`'s embedding.
4. Write both into a `.vlp` **patch file** (JSON-ish overlay, ~10 KB per fact). Base vindex stays untouched.

At query time, the patch overlay is merged in-memory. `COMPILE CURRENT INTO VINDEX` bakes the overlay into a new `down_weights.bin` by writing the new column in place — because `.vindex` stores column-major, the original file can be **hardlinked** and only changed columns rewritten. Instant.

**The audacious claim:** editing an LLM's factual knowledge reduces to "allocate a free row in a matrix and fill it in." No gradient descent.

## File format summary

| Format | What it is | Software analogy |
|---|---|---|
| `.safetensors` / `.gguf` | Raw model weights | Heap table |
| `.vindex` | Reorganized for query | Index + materialized view |
| `.vlp` | Patch overlay (edits) | WAL / diff file |
| `.trace` / `.bndx` | Per-inference record | Query plan + execution log |
| `Vindexfile` | Declarative build recipe | Dockerfile |

## Key takeaway

A transformer is mostly arithmetic over a flowing residual stream. Attention is a soft content-addressed hash-map that moves information between positions. FFN is a sparse KNN lookup (dressed up as a matmul) that transforms information at each position. Factual knowledge lives almost entirely in FFN weights, and those weights — reorganized — look exactly like a graph database with ~348K edges. LARQL is the tool that makes this view literal: query it with SQL-like syntax, edit it without retraining, recompile to standard model formats.