---
title: "a prompt constraint is a request, not an enforcement"
date: 2026-09-17
captured: 2026-09-17T00:00:00Z
tags: [ai, llm, reliability, workflow]
source: "Claude Code session"
aliases: ["model ignores my instruction sometimes", "enforce llm output format", "validate model output and retry"]
status: refined
---

A rule written into a prompt is satisfied most of the time. The run that violates it looks identical to the runs that did not: same shape, same confidence, same absence of any error. There is no signal to key on, so a pipeline that trusts the prompt line ships the violation downstream.

Put the enforcement on the output instead:

```
model -> validator -> ok?  -- yes --> use it
                       |
                       no ---> retry with the failure named, bounded, then fail loudly
```

The validator is the contract. It rejects anything that breaks the rule and asks again, naming what was wrong. Keep the prompt line as well, because a good instruction is what makes the validator fire rarely, but the prompt is now the optimisation and the validator is the guarantee.

Two things this buys beyond correctness. The rejection count is a measurement: a rule the model breaks often is a rule worth rewording. And a bounded retry converts a silent wrong answer into a visible failure, which is the trade you want on any path where a bad output is worse than no output.

## Key takeaway

If a constraint matters, something other than the model has to check it. Prompt text sets a probability; only a validator sets a floor.

## Related

- [[checks-that-report-success-while-verifying-nothing]] - same shape one layer over: a check that reports success while verifying nothing is a validator that quietly stopped being the guarantee
