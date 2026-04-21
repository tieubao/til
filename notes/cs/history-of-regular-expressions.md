---
title: "history of regular expressions"
date: 2021-08-08
captured: 2021-08-08T00:00:00Z
tags: [regex, cs-history, unix, ai-history, formal-languages]
source: "GitHub issue tieubao/til#564"
aliases: [regex-history]
status: refined
---

## Context

An article from "Why Is This Interesting?" that traces regular expressions from their origins in 1950s neuroscience and AI research through to their everyday use in UNIX tooling and web development. The story opens with a 2021 incident where a Russian censor accidentally broke the internet by misusing regex.

**Source:** [The Regular Expression Edition](https://whyisthisinteresting.substack.com/p/the-regular-expression-edition)

## The Russia incident

In March 2021, someone at Roscomnadzor (Russia's state internet censor) tried to block the domain `t.co` (Twitter's URL shortener). They wrote a regex pattern without anchors, so instead of matching only `t.co`, it matched any domain containing the string "t.co" - including microsoft.com, reddit.com, and even Russia's own rt.com. Network traffic to Rostelecom dropped 24%.

The classic Zawinski quote proved true: "Some people, when confronted with a problem, think 'I know, I'll use regular expressions.' Now they have two problems."

## Origins in neuroscience

The term "regular expression" originated with mathematician Stephen Kleene. In 1943, neuroscientist Warren McCulloch and logician Walter Pitts had described the first mathematical model of an artificial neuron. Kleene wanted to investigate what networks of these artificial neurons could theoretically compute.

In a 1951 RAND Corporation paper, Kleene reasoned about pattern detection by applying neural networks to simple toy languages - "regular languages." He developed an algebraic notation for encapsulating these "regular grammars" (e.g., `a*b*` for an A/B language), and the regular expression was born.

Kleene's work was expanded by Noam Chomsky and Marvin Minsky, who formally established the relationship between regular expressions, neural networks, and finite state machines.

## The AI winter connection

These discoveries made early AI pioneers feel they were unlocking fundamental mysteries of the mind. Then in 1969, Minsky published "Perceptrons," exploring limitations of early neural networks, which devastated the field. Walter Pitts, depressed and alcoholic, burned all his notes including an unpublished dissertation. He died of cirrhosis in 1969 at age 46.

Neural network research entered a long fallow period as researchers shifted from "connectionist" theories (intelligence from parallel networks) toward "symbolist" approaches (databases of facts plus logic rules).

## From AI research to everyday tooling

Regular expressions would likely have remained in esoteric CS if not for Ken Thompson, co-creator of UNIX at Bell Labs. In 1968, Thompson integrated regexes into the search feature of his QED text editor. From there, regex became a UNIX mainstay for wildcard matching anywhere text search was needed.

In the 1980s, Larry Wall made regexes a core feature of Perl, which became the "duct tape" of 1990s web development. This secured regex a permanent place in the everyday developer toolbox.

## The vindication

McCulloch, Pitts, and Kleene eventually got their vindication. The early 2010s saw a resurgence of interest in neural networks, leading to deep learning and advances in facial recognition, machine translation, and autonomous vehicles. The foundational work that also gave us regex turned out to be right all along.

## Related

- [[turing-completeness]] - related foundational CS concept
- [[grand-unified-theory-of-ai-hype-cycle]] - the AI winter pattern that disrupted regex's original field
