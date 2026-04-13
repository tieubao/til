---
title: "lessons learned in software development"
date: 2017-11-24
captured: 2017-11-24T23:39:57Z
tags: ["software-development", "heuristics", "debugging", "teamwork"]
source: "GitHub issue tieubao/til#340"
aliases: []
status: refined
---

## Context

Henrik Warne's collection of 22 heuristics and rules of thumb accumulated over years of software development, organized into four categories: development, troubleshooting, cooperation, and miscellaneous.

**Source:** [Lessons Learned in Software Development](https://henrikwarne.com/2015/04/16/lessons-learned-in-software-development/)

## Development

1. **Start small, then extend.** "A complex system that works is invariably found to have evolved from a simple system that worked." (John Gall)
2. **Change one thing at a time.** Short iterations make it much easier to find problems. Commit refactoring separately from new features.
3. **Add logging and error handling early.** Both are useful from the very beginning. As soon as something goes wrong, you need to see what is happening.
4. **All new lines must be executed at least once.** Cheat if necessary: misspell a column name to trigger error handling, invert an if-statement to test rare paths.
5. **Test the parts before the whole.** Well-tested parts save time when tracking down integration problems.
6. **Everything takes longer than you think.** Hofstadter's Law: "It always takes longer than you expect, even when you take into account Hofstadter's Law."
7. **First understand the existing code.** Reading code is as necessary a skill as writing code.
8. **Read and run.** Use both methods to understand code. Running code reveals behavior that reading alone cannot.

## Troubleshooting

9. **There will always be bugs.** Build systems for quick troubleshooting, fixing, and deploying, not for "getting it right the first time."
10. **Solve trouble reports.** Every developer should handle customer bugs. It reveals how the system is actually used.
11. **Reproduce the problem.** First step always. Then verify the fix makes the problem disappear.
12. **Fix the known errors, then see what's left.** Multiple bugs can interact and cause confusing symptoms.
13. **Assume no coincidences.** Changed a timer and the system restarts more? Not a coincidence. Investigate.
14. **Correlate with timestamps.** Look for even increments. A restart 3000ms after a request suggests a timer triggered it.

## Cooperation

15. **Face to face has the highest bandwidth.** Solutions are often much better after in-person discussion.
16. **Rubber ducking.** Explain the problem to a colleague. You often realize the answer as you talk.
17. **Ask.** If someone knowledgeable is available, ask. Minutes of conversation can replace days of code reading.
18. **Share credit.** Say "Marcus came up with the idea" instead of "we tried."

## Miscellaneous

19. **Try it.** Write a little program to test how a language feature works. Fiddling around often reveals bugs.
20. **Sleep on it.** Your subconscious works on hard problems overnight.
21. **Change.** Don't be afraid to change roles or jobs. It is stimulating.
22. **Keep learning.** Different languages, tools, books, courses. Small improvements compound.

## Related
