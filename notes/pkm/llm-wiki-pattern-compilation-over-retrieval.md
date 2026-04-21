---
title: "LLM Wiki pattern: compilation over retrieval"
date: 2026-04-13
captured: 2026-04-13T01:30:00.000Z
tags: ["pkm", "llm", "zettelkasten", "knowledge-management", "karpathy"]
source: "Karpathy LLM Wiki gist (April 2026) + Claude.ai analysis session"
aliases: ["karpathy-llm-wiki", "compilation-over-retrieval"]
status: refined
---

## Context

Andrej Karpathy published an "idea file" for building personal knowledge bases using LLMs (April 2026, 5000+ GitHub stars). Studied it while migrating my TIL repo to a Zettelkasten-style wiki. This note captures the pattern, what's transferable, and where it breaks.

## The core insight

RAG re-derives knowledge from scratch on every query. The LLM Wiki pattern inverts this: the LLM incrementally compiles raw sources into a structured, interlinked wiki. Knowledge is built up once and kept current, not re-derived.

The analogy: RAG cooks every time you're hungry. LLM Wiki builds a kitchen that keeps improving its recipes.

## The three-layer architecture

1. **Raw sources** (immutable): articles, papers, clippings. LLM reads but never modifies.
2. **The wiki** (LLM-generated): summaries, entity pages, concept pages, cross-references. LLM owns this entirely.
3. **The schema** (co-evolved): CLAUDE.md or equivalent. Tells the LLM how to be a disciplined wiki maintainer. This is the real product.

## The three operations

- **Ingest**: new source arrives, LLM processes it, touches 10-15 wiki pages per source. Not just filing; updating cross-references, flagging contradictions, revising summaries.
- **Query**: ask questions against the compiled wiki, not raw docs. Good answers get filed back as new pages. Explorations compound.
- **Lint**: periodic health-check. Orphans, contradictions, stale claims, missing cross-references, gaps worth investigating.

## Four insights worth stealing

**1. "Idea file" as distribution format.** Share the idea/spec, not the app. Each person's LLM agent customizes the implementation. A new primitive for the agent era.

**2. Compilation mindset over retrieval mindset.** Stop treating LLMs as search engines over docs. Treat them as compilers that transform raw material into structured, queryable knowledge.

**3. Output-to-wiki feedback loop.** Most people lose 90% of value from LLM conversations by never persisting the output. Systematically routing good synthesis back into the wiki is what makes knowledge compound.

**4. Schema as product.** The raw docs and wiki pages are data. The schema is what turns a generic LLM into a disciplined knowledge worker. Invest heavily here; a mediocre schema produces a mediocre wiki regardless of LLM quality.

## Where it breaks at scale

Karpathy's own wiki: ~100 articles, ~400K words. Worked fine with flat files + index, no vector search.

At 100K+ records:
- `index.md` blows past any context window
- Cross-references become combinatorial (no single LLM pass can maintain coherence)
- Contradiction detection needs systematic traversal, not ad-hoc linting
- Staleness needs timestamps and decay functions

The LLM Wiki v2 extension proposes consolidation tiers: working memory, episodic memory, semantic memory, procedural memory. This is what Mem0 and Letta are building at the infrastructure level.

**Honest synthesis**: brilliant starting architecture for personal/small-team. The compilation mindset is correct at any scale. The flat-file approach stops working past a few hundred documents. Pattern stays the same; plumbing grows up.

## The PKM critique

If the LLM does all the writing, you've built a personalized research index, not a "second brain." The act of writing is load-bearing in the PKM sense. This is a real tension. The right balance: human thinks and directs; LLM handles bookkeeping (filing, linking, cross-referencing, linting). Never fully automate synthesis.

## How this applies to our wiki

We adopted the compilation mindset but initially only built the filing system (wikilinks, frontmatter, inbox). The compilation step (updating related notes, flagging contradictions, maintaining synthesis pages) was added to the ingest workflow after analyzing this pattern. The key addition: every note addition now triggers a check against existing knowledge, not just a file operation.

## Related

- [[why-knowledge-notes-need-context-not-just-facts]] - the meta-insight that motivated capturing this pattern
- [[llm-memory-systems-three-competitive-battlegrounds]] - Mem0/Letta/A-Mem approaches to the same problem at infrastructure level
- [[llm-agent-memory-systems-landscape-2026]] - broader landscape of memory systems that LLM Wiki sits within
- [[memory-systems-as-agent-harness-plugins]] - how memory could plug into agent frameworks (related to schema-as-product insight)
