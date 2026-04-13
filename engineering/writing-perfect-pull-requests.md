---
title: "writing perfect pull requests"
date: 2016-05-12
captured: 2016-05-12T11:08:03Z
tags: [git, code-review, collaboration]
source: "GitHub issue tieubao/til#227 + https://github.blog/2015-01-21-how-to-write-the-perfect-pull-request/"
aliases: []
status: refined
---

## Context

A collection of best practices for pull requests, drawn primarily from GitHub's own guidelines. The issue aggregated multiple sources on PR workflows, including WIP patterns and types of PRs.

**Source:** [How to Write the Perfect Pull Request](https://github.blog/2015-01-21-how-to-write-the-perfect-pull-request/)

## Writing pull requests

- **Provide context**: Include the purpose and reasoning behind changes. Assume readers lack historical knowledge of the problem.
- **Be explicit about feedback needs**: Specify whether you want code review, technical discussion, or design critique. Not all PRs need the same type of attention.
- **Use clear signals**: Mark work-in-progress with `[WIP]` prefix. Mention specific people or teams with reasons for why their input matters.
- **Link related work**: Reference issues, prior PRs, and design docs to give reviewers full context.

## Offering feedback

- **Ask questions rather than issue commands**: Use collaborative language like "What do you think about...?" instead of directives. This opens dialogue rather than creating resistance.
- **Explain reasoning**: Connect suggestions to style guides or documented standards, not personal preference.
- **Mind your tone**: Written communication defaults to negative interpretation. Use emoji and positive language intentionally to clarify intent.
- **Focus on the work, not the person**: Avoid derogatory language about code choices. Critique the code, not the developer.

## Responding to feedback

- **Acknowledge appreciation**: Lead responses with thanks, particularly for mixed feedback.
- **Request clarification**: Do not assume understanding when a comment is ambiguous.
- **Link follow-up work**: Reference commits or related PRs to maintain traceability when addressing comments.
- **Escalate thoughtfully**: Switch to synchronous communication (call, video) if written discussion becomes circular or confusing.

## Types of pull requests

Not every PR serves the same purpose. Some are early drafts seeking directional feedback, others are polished implementations ready for final review. Signaling which type you are submitting saves everyone time and sets correct expectations.

## Related

- [[writing-good-commit-messages]] - complements PR writing with commit-level communication
- [[code-review-basics]] - the reviewer's side of the PR process
- [[effective-code-reviews]] - deeper patterns for productive review culture
