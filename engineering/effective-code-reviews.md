---
title: "effective code reviews without wasting time"
date: 2017-11-17
captured: 2017-11-17T17:15:02Z
tags: ["code-review", "team-practices", "quality"]
source: "GitHub issue tieubao/til#335"
aliases: ["dont-waste-time-on-code-reviews"]
status: refined
---

## Context

A detailed guide from swreflections.blogspot.com on how to get maximum value from code reviews without wasting developer time. Based on research from Microsoft, Google, and Cisco.

**Source:** [Don't Waste Time on Code Reviews](http://swreflections.blogspot.com/2014/08/dont-waste-time-on-code-reviews.html)

## Keep it simple

Formal code inspection meetings (prep work, room full of reviewers, moderator, secretary) are expensive and add delays without proportional value. Studies show only 4% of defects are found in the meeting itself; the rest are found by reviewers working on their own.

Microsoft and Google use lightweight collaborative review platforms (Gerrit, CodeFlow, ReviewBoard) with asynchronous feedback. These are just as effective as inspections but far cheaper and faster.

## Keep reviewers small

One reviewer finds roughly half of defects. A second finds half as many new problems as the first. Beyond two reviewers, you waste time. One study showed no difference between teams of 3, 4, or 5 reviewers. There is also a "social loafing" problem: more reviewers means each feels less pressure to find problems.

At Google and Microsoft, the median number of reviewers is 2.

## Who should review

Reviews should NOT be done by new team members learning the codebase; it is a lousy way to train people and a lousy way to do reviews. Reviews SHOULD be done for new team members (i.e., review their code).

Your best developers and architects will spend a lot of time reviewing, and they should. They find problems faster and offer more valuable feedback. Pair programming is better for onboarding than asking newcomers to review.

## Substance over style

Use tools like Checkstyle to enforce formatting. Free up reviewers to focus on what matters:

**Correctness:** functional correctness, coding errors (off-by-one, wrong variable, copy-paste errors), design mistakes, safety and defensiveness (threading, error handling, data validation), security (auth, access control, encryption).

**Maintainability:** clarity (naming, comments), consistency (using common patterns), organization (no duplication or dead code), approach (simpler or more efficient alternatives).

## Focus on risk

Apply the 80/20 rule. High risk code: network-facing APIs, framework code, critical business logic, security libraries, code handling private data, old or complex code with lots of bug history.

High risk changes: code from a new team member, big changes, large-scale refactoring disguised as refactoring.

## Microsoft's two-pass approach

Sometimes two separate reviews work better: a superficial "code cleanup" review for standards and clarity, followed by an in-depth correctness review after the code is tidied up.

## Related
