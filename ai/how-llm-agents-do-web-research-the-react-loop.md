---
title: "How LLM agents do web research: the ReAct loop"
date: 2026-04-19
captured: 2026-04-19T04:12:05.369Z
tags: ["ai", "agents", "research"]
source: "Claude.ai chat"
---
LLM-based agents (Claude, ChatGPT, Perplexity, etc.) don't use a classical search algorithm when given an open-ended research task. They run a **ReAct loop** (Reason + Act): the model writes out reasoning in natural language, calls a tool, reads the result, and decides what to do next. The "intelligence" isn't in a clever ranking function. It's the same next-token prediction that writes prose, applied to the question "what should I do now?"

## The core loop

![LLM research loop diagram](https://assets.han-ws.workers.dev/i/2026/04/llm-research-loop.svg)

The cycle runs: plan → search → evaluate snippets → either fetch a page or refine the query → check coverage → loop or stop. Two amber decision points act as gates: "do these search results have enough signal?" and "do I have enough to answer?"

## Query formulation

A human typically types 2-4 keywords into Google, scans results, and refines. A decent agent does something similar but with a twist: it decomposes one big question into sub-questions and often runs several queries in parallel.

For a question like "is technology X good or not", a good agent mentally splits that into:

- "What is X and what does it do?" (definitional)
- "X pros and cons" (evaluative)
- "X vs alternatives" (comparative)
- "X problems / issues / limitations" (adversarial, the most important one)
- "X changelog / recent news" (freshness check)

Humans do this too, but rarely systematically. The human tendency is to Google "X review", get biased content, and stop. A good agent explicitly searches for disconfirming evidence. A bad agent searches "why X is great" and finds exactly that.

**Where agents are worse than humans at query formulation:** creativity. Humans know Reddit slang, know to search `site:reddit.com X` or `"hackernews X"` to find skeptical takes, know to check Twitter for real-time sentiment from practitioners. Agents tend to write formal, neutral queries that miss subcultural signal.

## Search vs read tradeoff

After searching, the agent gets back ~10 results with titles and ~200-character snippets. It has to decide whether the snippet is enough or whether to fetch the full page.

Rough rule: **fetch when the question is evaluative, comparative, or requires reasoning that wouldn't compress into a snippet.** For factual lookups ("who is the CEO of X", "when did Y launch"), snippets are fine. For "is technology X good", snippets never contain the reasoning; the full article is where the answer lives.

Fetching costs 10-50x more tokens than reading snippets, so agents tend to be stingy with fetches. A common failure mode is over-searching and under-reading: accumulating 20 snippets and never reading a single article deeply. The discipline is to commit to reading 2-3 good sources deeply rather than skimming 20.

## Stopping conditions

There is no principled stopping rule. It's a mix of four signals:

**Budget-based.** Most agents have a soft cap (5-15 tool calls for a single task, 50-300 for research mode). Once you burn through that, you're being wasteful.

**Coverage-based.** Did I address each sub-question I planned? If I have "what is it", "pros", "cons", and "alternatives" covered, I'm done. If I'm still missing "cons", keep going.

**Diminishing returns.** If the next three searches return results I've already seen, stop. New queries should retrieve new information; if they don't, I'm overfishing the same pond.

**Confidence-based (the weakest signal).** "Do I feel I can answer now?" This is where overconfidence bites. The LLM's subjective sense of sufficiency is not well-calibrated. It often stops too early on hard questions and too late on easy ones. The budget is the real backstop.

## Source selection

Default ranking that most agents lean toward:

1. Official documentation and primary sources (docs, papers, official blogs)
2. Established publications (Reuters, major tech press)
3. High-reputation aggregators (Hacker News threads, Stack Overflow)
4. Wikipedia (often as a starting map, not a final source)
5. Forums (Reddit, Discord archives) — used selectively
6. Twitter/X — used rarely unless the query is explicitly about a Twitter discussion

The reasoning: official sources are factually dense, low-noise, and paraphrasable without copyright issues. Reddit is high-signal for opinions and real-world use but requires filtering, which is hard to do programmatically.

**The Reddit/HN blindspot.** For evaluating a technology, Reddit and HN are often where the real answer lives. Docs tell you what the tech claims to do. Reddit tells you what breaks at 3am. Agents often under-weight these sources. A good prompt explicitly says "check Hacker News and Reddit for practitioner takes" to force this.

Cases where the agent should go to Reddit/HN/Twitter but often doesn't:

- "Is X actually good in production" → Reddit threads from practitioners
- "Why is everyone talking about X" → Twitter, HN
- "What do people hate about X" → specific subreddits
- Anything about vibes, community sentiment, or tribal knowledge

## Raw agents vs productized research tools

The jump from "chat with search" to "Research mode" is architectural, not just a bigger budget. Research mode spawns multiple subagents in parallel, each with its own sub-question, each doing its own ReAct loop, and then a coordinator synthesizes. That's why it takes 5-20 minutes instead of 10 seconds.

| Product | Loop type | Typical tool calls | Strength | Weakness |
|---------|-----------|-------------------|----------|----------|
| Raw Claude + search | Single agent, ReAct | 3 to 10 | Fast, conversational | Shallow on broad topics |
| ChatGPT Search | Single agent, ReAct | 3 to 8 | Fresh results, citations | Same shallow pattern |
| Perplexity | RAG-first, then synthesize | 5 to 15 | Aggressive source retrieval | Weaker synthesis |
| Claude Research | Multi-agent, parallel subagents | 50 to 200+ | Deep, cross-verified | Slow, can overproduce |
| ChatGPT Deep Research | Multi-step planner | 50 to 300+ | Structured long reports | Verbose, repetitive |

## Where human research still wins

**Humans have a working BS filter.** When a human lands on a Medium post with 3 clap emojis titled "10 Reasons X is the Future", they close the tab. Agents read it and cite it unless something in the snippet already smells off. Source credibility based on vibe is something humans do effortlessly and agents do poorly.

**Humans iterate on queries based on what's missing; agents iterate based on what was asked.** If a human Googles "Is Cloudflare Workers good" and every result is a Cloudflare blog post, they notice the bias and add "site:reddit.com" or "cloudflare workers problems". An agent is more likely to just write a balanced summary from the biased results because it doesn't viscerally notice the asymmetry.

**Humans know when to stop Googling and ask someone.** For "is this technology good", 30 minutes in Discord with someone who actually uses it beats 3 hours of Google. Agents can only read what's already been written down.

**Humans and agents have opposite cutoff problems.** Humans don't know the newest stuff. Agents don't reliably know the newest stuff either (training cutoff), and also miss old pre-SEO content that's been buried. The complementary strength is real: humans know the vibe and the old canon; agents can read 50 sources in 2 minutes.

## Practical takeaways

- For factual questions, one search is enough. Don't overthink it.
- For "evaluate X" questions, explicitly tell the agent to check Reddit, HN, or critical takes. Otherwise it defaults to official docs.
- For deep comparisons, use Research mode, not chat. The architectures are different.
- Assume the agent is overconfident on stopping. If the answer feels thin, ask "what didn't you check?" and it will usually admit it.
- For Twitter-style current sentiment, tell the agent to search Twitter/X specifically. It rarely goes there on its own.

## Key takeaway

Agent web research is a ReAct loop, not an algorithm. The stopping condition is a soft mix of budget, coverage, and diminishing returns; the agent's confidence signal is unreliable. The biggest systematic failure mode is over-weighting official sources and under-weighting Reddit/HN/Twitter, which is where practitioner truth tends to live. Prompts that force the agent toward community sources close the biggest quality gap.