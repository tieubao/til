---
title: "Forward Deployed Engineer vs Requirements Engineer"
date: 2026-06-10
captured: 2026-06-10T04:43:33.431Z
tags: ["career", "engineering-roles", "consulting"]
source: "Claude Code session"
---
Forward Deployed Engineer (FDE) and Requirements Engineer are not equivalent roles. They overlap on one slice (discovering what the customer actually needs) but the FDE's mandate is to **build and ship**, while the requirements engineer's mandate is to **specify and hand off**. Requirements engineering is roughly the first 20% of what an FDE does.

## Overlap at a glance

```
┌─────────────── FDE ────────────────┐
│  ship production code              │
│  embedded at customer site         │
│  own the customer outcome     ┌────┼──── Requirements Engineer ─────┐
│  build integrations / glue    │    │                                │
│  deploy + iterate in days     │ elicitation;                       │
│  feed learnings back to       │ translate business pain            │
│  the core product team        │ into concrete needs;               │
│                               │ stakeholder interviews             │
└───────────────────────────────┼────┘                               │
                                │  write SRS / specs / user stories  │
                                │  acceptance criteria, traceability │
                                │  validation + sign-off, hand off   │
                                └────────────────────────────────────┘
   BUILD & SHIP  ◄──── shared: discover what the ────►  SPECIFY & HAND OFF
                       customer actually needs
```

## What a Forward Deployed Engineer is

The role was popularized by Palantir (~2010s) and revived hard by AI companies (OpenAI, Anthropic, Scale, many AI startups) because LLM products need heavy last-mile adaptation per customer.

An FDE is a full software engineer embedded with a specific customer, often on-site or deep in their Slack and systems, who:

1. Lives inside the customer's real workflow and data, and discovers the actual problem (rarely what the sales deck said).
2. Builds working software against it: integrations, pipelines, custom UIs, prompt/agent tuning, glue code on top of the core product.
3. Ships and iterates in days, owning outcomes ("the fraud team now catches X") rather than tickets.
4. Feeds what they learned back to the core product team, so one-off hacks become product features.

The defining trait: they write production code at the customer boundary. Palantir's internal split was FDE ("Delta") vs core product dev ("Dev"); AI companies copied the model because every enterprise AI deployment is bespoke.

## Comparison

| Axis | Forward Deployed Engineer | Requirements Engineer |
|---|---|---|
| Mandate | Make the product work for THIS customer, end to end | Elicit, document, validate what should be built |
| Primary deliverable | Working, deployed software + product feedback | SRS / specs, user stories, acceptance criteria, traceability |
| Builds code? | Yes, daily; that is the job | Typically no; hands off to dev team |
| Position in cycle | Entire loop: discover, build, deploy, iterate, feed back | Front of the cycle, before build |
| Org home | Vendor side, embedded at customer | Either side; classic in regulated/waterfall industries (aero, auto, banking) |
| Success metric | Customer outcome and expansion/renewal | Spec completeness, correctness, sign-off |
| Typical background | Strong SWE who can talk to customers | BA/systems engineering, IREB/CPRE certified |

The overlap: both do elicitation, both translate vague business pain into something concrete. The difference: the requirements engineer's output is a document; the FDE's output is the system itself, with requirements discovered implicitly by shipping and watching.

## Verdict

Distinct roles with a shared discovery slice. Read "FDE" in a job posting or org chart as "deployment-side product engineer", not "BA/requirements analyst". The requirements engineer is the closer cousin to a BA or pre-sales solutions consultant who never touches the codebase. The FDE model is essentially what a good services-agency engineer does on a client engagement: embed, discover, build, own the outcome.

One nuance: companies sometimes abuse the FDE title for what is really support engineering or solutions consulting with no code ownership. The litmus test in any posting is "do you ship code into the customer's environment?"