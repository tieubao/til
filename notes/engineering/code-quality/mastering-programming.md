---
title: "mastering programming by Kent Beck"
date: 2016-06-08
captured: "2016-06-08T09:30:37Z"
tags: [programming, productivity, mindset, better-dev]
source: "GitHub issue tieubao/til#236"
aliases: []
status: refined
---

## Context

Kent Beck's observations from years of watching master programmers versus skilled journeymen. The key insight: journeymen scale by solving more problems at once, masters scale by solving fewer problems at once.

**Source:** [Kent Beck - Mastering Programming](https://www.facebook.com/notes/kent-beck/mastering-programming/1184427814923414)

## Time

- **Slicing.** Take a big project, cut it into thin slices, rearrange them to suit your context. You can always slice finer.
- **One thing at a time.** Reducing feedback cycles to save overhead leads to difficult debugging whose expected cost exceeds the overhead avoided.
- **Make it run, make it right, make it fast.** An example of slicing + one thing at a time.
- **Easy changes.** When faced with a hard change, first make it easy (warning: this may be hard), then make the easy change.
  - **Concentration:** rearrange code so the change only needs to happen in one element.
  - **Isolation:** extract a part so the whole subelement changes.
- **Baseline measurement.** Measure the current state before fixing things. Without a baseline you cannot know if you are actually improving.

## Learning

- **Call your shot.** Before running code, predict exactly what will happen.
- **Concrete hypotheses.** When code misbehaves, articulate exactly what you think is wrong before changing anything. If you have multiple hypotheses, find a differential diagnosis.
- **Remove extraneous detail.** Find the shortest repro steps. The shortest test case. The most basic API example.
- **Multiple scales.** Move between scales freely. Maybe it is a design problem, not a testing problem. Maybe a people problem, not a technology problem.

## Transcend logic

- **Symmetry.** Things almost the same can be divided into parts that are identical and parts that are clearly different.
- **Aesthetics.** Beauty is a powerful gradient to climb, and a liberating one to flout.
- **Rhythm.** Wait for the right moment. Act with intensity when the time comes.
- **Tradeoffs.** Knowing what a decision depends on matters more than knowing which answer to pick today.

## Risk

- **Fun list.** Note tangential ideas and get back to work. Revisit at stopping points.
- **Feed ideas.** Ideas are like frightened birds. Feed them a little. Invalidate from data, not from lack of self-esteem.
- **80/15/5.** 80% low-risk/reasonable-payoff, 15% related high-risk/high-payoff, 5% things that tickle you. Teach someone your 80%. When a 15% experiment pays off, it becomes your new 80%.

## Related
