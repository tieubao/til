---
title: "skill selection runs on the description text alone"
date: 2026-09-17
captured: 2026-09-17T00:00:00Z
tags: [claude-code, skills, agents, workflow]
source: "Claude Code session"
aliases: ["why does my skill never fire", "skill description trigger phrases", "plugin skill wins over my command"]
status: refined
---

When an agent picks between a slash command and a skill, the only thing it reads is the description text. Not the body, not the file name, not what the thing actually does. A command whose description is a bare one-liner therefore loses every matching prompt to a skill that spells out trigger phrases, even when the command is a strict superset of it and would have done the job better.

The failure mode is quiet. Nothing errors. The wrong thing runs, produces a plausible result, and the better tool sits unused for weeks.

The fix is to write the description as a router entry, not a label:

- Name the trigger phrases a user actually types, verbatim.
- Include every language the user types in, not just English.
- Say what the thing is NOT for, so the near-miss cases route elsewhere.

Then pin it. A description is prose, so it drifts on the next edit and nothing catches that. A test that feeds representative prompts and asserts which name is selected turns routing into something that can go red.

## Key takeaway

Description text is the routing table. Treat it as an interface with tests, not as documentation.

## Related

- [[commands-vs-hooks-vs-skills-decision-framework]] - which of the three a given piece of behaviour belongs in
