---
title: "Complete guide to Claude Code - features, workflows, and ecosystem"
date: 2026-03-30
captured: 2026-03-30T16:50:24.441Z
tags: ["ai", "claude-code", "developer-tools", "axel-bitblaze69"]
source: "X Article by @Axel_bitblaze69"
aliases: []
status: refined
---
> Source: [@Axel_bitblaze69](https://x.com/Axel_bitblaze69/status/2037978621684621428) | 2026-03-28
> Original title: "Everything I Learned Using Claude Code for 2 Months"

![Cover](https://assets.han-ws.workers.dev/i/2026/03/tweet-2037978621684621428-cover.jpg)

## Summary

A comprehensive practitioner's guide to Claude Code based on 2 months of daily, intensive use. Covers the full feature set (agentic loop, multi-file editing, subagents, MCP, hooks, skills, memory), configuration best practices (CLAUDE.md under 200 lines, rules/ folder, path-scoped rules), cost management (Sonnet for 90% of tasks, match model to problem), power user workflows (plan mode, worktrees, context management, the "interview me" technique), the growing ecosystem (Superpowers, GSD, /last30days skill), and 7 common mistakes the author made. Positions Claude Code as the #1 most loved AI coding tool in early 2026 surveys, while recommending a combo strategy with Cursor for daily tab completions.

## Article

It's March 2026 and if you're still copy pasting code into chatgpt then we need to talk.

i've been deep in claude code for the last 2 months. not casually. daily. in the trenches. i did the courses, got certified, built projects with it, broke things, fixed them, and built more things. i've spent more time in my terminal talking to claude than talking to actual humans.

this isn't a "top 10 AI tools" fluff piece. this is everything i've learned the features nobody talks about, the workflows that actually work, the mistakes that cost me hours, and the tricks that saved me days.

bookmark this. you're going to need it more than once.

### What claude code actually is

let me kill the biggest misconception first. claude code is not copilot. it's not a chatbot you paste code into. it's not autocomplete on steroids.

it's an agent. an actual autonomous agent that:

- reads your entire codebase
- plans an approach
- edits files across your whole project
- runs your tests
- sees failures and fixes them
- iterates until the job is done

the key word is agentic. it operates in a loop gather context, take action, verify results, repeat. you tell it what you want, it figures out how to get there. you're not writing code together. you're delegating to something that actually understands what it's reading.

it lives in your terminal. that's not a limitation that's the point. your terminal is the most powerful interface on your machine. claude code meets you there.

![Claude Code terminal interface](https://assets.han-ws.workers.dev/i/2026/03/tweet-2037978621684621428-1.jpg)

### Where it runs

Claude code isn't just a CLI tool anymore. it's everywhere:

- **terminal**: the OG. full power. fastest. this is where the magic happens.
- **VS Code extension**: inline diffs, @-mentions, sidebar conversations. for people who live in their editor.
- **JetBrains plugin**: same deal, for the IntelliJ crew.
- **desktop app (claude cowork)**: visual diff review, multiple sessions, scheduled tasks. no terminal needed. the "i don't do command lines" option.
- **web app (claude.ai/code)**: no local setup needed. run long tasks from anywhere. open a browser and go.
- **mobile via remote control**: start a session on your laptop, control it from your phone.
- **slack**: mention @Claude in your workspace, turn issues into PRs from chat.
- **github actions**: @claude in a PR comment and it responds with actual code changes.
- **GitLab CI/CD**: same concept for gitlab teams.

the remote control feature is underrated and nobody talks about it. you're at lunch, get a slack message about a bug, pull out your phone, tell claude to fix it. it runs on your machine at home. you review the diff on your phone. approve. done. the future is now lads.

### Getting started

install:

then:

that's it. no config files. no setup wizard. no 47 extensions to install. no yaml hell. just cd into your project and type claude.

first thing you should do:

watch it explore your codebase, read your files, understand your architecture, and give you a summary. that moment when it correctly summarizes a 20,000+ line repo for the first time that's when it clicks. that's the moment you realize this isn't autocomplete.

### The models

claude code runs on anthropic's claude model family. knowing which to use and when is half the game:

- **claude opus 4.6**: the big brain. best reasoning. use for complex architecture decisions, tricky debugging, large refactors that touch 10+ files. when you need it to actually think.
- **claude sonnet 4.6**: the workhorse. default model. best balance of speed, cost, and quality for everyday coding. this is your daily driver.
- **claude haiku**: the speedster. cheap and fast. great for simple tasks and subagents. don't sleep on this for quick questions.

switch models anytime.

here's what i learned the hard way: match effort to the problem. don't burn opus tokens on renaming a variable. don't use haiku for redesigning your auth system. i wasted probably $200 in my first week using opus for everything because it "felt smarter." sonnet handles 90% of tasks identically.

you can also control thinking depth independently of the model.

### Pricing (the real talk)

let's be honest about money because nobody else is:

Real world numbers from anthropic themselves: average developer spends about $5/day. 90% of users are under $12/day.

here's the wild stat that sold me: one developer tracked 10 billion tokens over 8 months. on API pricing that would've been ~$15,000. on the max plan? $800. if you're using it daily (and you should be), max pays for itself by week 2.

when you hit your limits on max, you can enable "extra usage" billed at API rates. no hard cutoff. no "sorry, come back tomorrow."

### Cost management tips i wish i knew day 1

### The .claude folder your project's control center

this is where most people leave 80% of the value on the table. the .claude folder is not a black box. it's the control center for how claude behaves in your project.

and here's what most people don't realize: there are two .claude directories, not one.

the first lives inside your project (committed to git, shared with your team). the second lives at ~/.claude/ (personal preferences, machine-local state, session history).

![.claude folder structure](https://assets.han-ws.workers.dev/i/2026/03/tweet-2037978621684621428-2.jpg)

### CLAUDE.md: the single most important file

when you start a claude code session, the first thing it reads is CLAUDE.md. it loads it straight into the system prompt. everything in there, claude follows. every session. consistently.

if you tell claude to always write tests before implementation, it will. if you say "never use console.log, use the custom logger," it will respect that every time.

what to write:

- build, test, and lint commands (npm run test, make build)
- key architectural decisions ("we use a monorepo with turborepo")
- non-obvious gotchas ("typescript strict mode is on, unused variables are errors")
- import conventions, naming patterns, error handling styles
- file and folder structure for the main modules

what NOT to write:

- anything that belongs in a linter or formatter config (prettier handles that, not CLAUDE.md)
- full documentation you can already link to
- long paragraphs explaining theory

keep it under 200 lines. files longer than that start eating too much context, and claude's instruction adherence actually drops. i learned this the hard way when my 400-line CLAUDE.md was getting half-ignored. cut it to 150 lines, everything got better.

### CLAUDE.local.md: your personal overrides

sometimes you have preferences that are just yours, not the team's. maybe you prefer a different test runner. maybe you want claude to always open files in a specific pattern.

create CLAUDE.local.md in your project root. claude reads it alongside the main CLAUDE.md, and it's automatically gitignored so your personal tweaks never land in the repo.

### The rules/ folder modular instructions that scale

CLAUDE.md works great for a single project. but once your team grows, you end up with a 300-line CLAUDE.md that nobody maintains and everyone ignores.

the rules/ folder solves that.

every markdown file inside .claude/rules/ gets loaded alongside your CLAUDE.md automatically.

each file stays focused. the person who owns API conventions edits api-conventions.md. the person who owns testing standards edits testing.md. nobody stomps on each other.

the real power comes from path-scoped rules. add YAML frontmatter and the rule only activates when claude is working with matching files:

API Design Rules example:

- All handlers return { data, error } shape
- Use zod for request body validation
- Never expose internal error details to clients

claude won't even load this when it's editing a React component. it only kicks in when working inside src/api/ or src/handlers/. clean, focused, efficient.

![Rules folder structure](https://assets.han-ws.workers.dev/i/2026/03/tweet-2037978621684621428-3.jpg)

### The features that changed how i work

#### Multi-file editing

this is where claude code leaves everything else in the dust. this is the feature that made me stop using anything else for serious work.

it will:

- find every file touching auth
- update the middleware
- modify the routes
- change the tests
- update the config
- fix the imports

across 15+ files in one session. shows you diffs for review before applying. you approve, reject, or ask for changes on each file.

i refactored an entire express app from callbacks to async/await in one session. 23 files. every one correct. try doing that with tab completion.

#### Git integration

claude code speaks git natively and it's better at commit messages than most humans i've worked with.

it writes proper commit messages not "fixed stuff" but actual descriptions of what changed and why. it creates PRs with summaries and test plans. it handles merge conflicts by actually understanding the code on both sides, not just picking a side and hoping.

#### The agentic loop write, test, fix, repeat

claude doesn't just write code it runs it.

this loop is the whole point. it's not "here's some code, good luck." it's "i wrote it, tested it, fixed the edge case you didn't think of, and it passes." i've watched it catch bugs in its own code that i wouldn't have spotted for hours.

#### Subagents the game changer nobody talks about

this one took me 3 weeks to start using properly and i regret every day i wasted not using them.

claude can spawn specialized sub-agents to handle specific tasks in isolation:

- **explore agent** read-only research, fast, uses haiku. perfect for "go understand how this module works."
- **plan agent** analyzes before implementing. the "think before you code" enforcer.
- **general agent** complex multi-step tasks in a clean context.

why this matters: when you ask claude to run your full test suite, that output hundreds of lines of pass/fail.. floods your context window. your main conversation gets polluted with noise. subagents handle it in isolation. they do the messy work, compress the findings, and send back a clean summary.

![Subagents architecture](https://assets.han-ws.workers.dev/i/2026/03/tweet-2037978621684621428-4.jpg)

you can also create custom agents for your specific workflows.

now claude automatically delegates security reviews to your custom agent when relevant. or you can call it explicitly: type /security-reviewer and it spins up.

the tools field is intentional.. a security auditor only needs Read, Grep, and Glob. it has no business writing files. the model field lets you use haiku for cheap read-only tasks and save opus for the ones that actually need deep reasoning.

personal agents go in ~/.claude/agents/ and are available across all your projects.

### MCP model context protocol (the secret weapon)

this is where claude code goes from "good coding assistant" to "orchestration layer for my entire workflow."

MCP lets claude connect to external tools and services:

- **github**: search repos, read issues, manage PRs, review code
- **slack**: read channels, post messages, respond to threads
- **postgres/mysql**: query your database directly
- **jira**: update tickets, change statuses
- **figma**: read designs (yes, really)
- **puppeteer/playwright**: browser automation
- **sentry**: error monitoring
- **notion**: read and write docs

configure it in .mcp.json at your project root.

now you can do things like:

one tool, connected to everything. i have claude pulling github issues, reading my database, and posting summaries to slack in one conversation. no switching tabs. no context switching. no copy-pasting between tools.

![MCP integration diagram](https://assets.han-ws.workers.dev/i/2026/03/tweet-2037978621684621428-5.jpg)

### Hooks: deterministic automation

here's the thing about CLAUDE.md instructions, they're suggestions. claude follows them most of the time, not all of the time. you can't rely on a language model to always run your linter. always format your code. always check for dangerous commands.

hooks make these behaviors deterministic. they're event handlers that fire automatically at specific points in claude's workflow. your shell script runs every time, no exceptions.

the events you'll use most:

- **PreToolUse**: fires before any tool runs. your security gate. block dangerous commands here.
- **PostToolUse**: fires after a tool succeeds. auto-format, auto-lint, auto-validate.
- **Stop**: fires when claude finishes a task. quality gate. "tests must pass before you're done."
- **UserPromptSubmit**: fires when you press enter. prompt validation.
- **Notification**: desktop alerts when claude needs your attention.

here's a real hooks config that auto-formats every file claude touches and blocks dangerous bash commands:

the bash firewall script reads the command from stdin, checks it against dangerous patterns (rm -rf, git push --force, DROP TABLE), and exits with code 2 to block it. exit 0 = let it through. exit 1 = warn but continue. exit 2 = block completely.

a Stop hook that runs npm test and exits with code 2 on failure will prevent claude from declaring "done" until the suite is green. no more "i'm done!" when 3 tests are failing.

hooks don't hot-reload mid-session, restart if you change them. and they run with your full user permissions, so quote your shell variables and validate your JSON input.

![Hooks configuration](https://assets.han-ws.workers.dev/i/2026/03/tweet-2037978621684621428-6.jpg)

### Skills: reusable workflows on demand

skills are the feature that made me realize how deep this rabbit hole goes.

a skill is a workflow that claude can invoke on its own based on context, or that you can trigger with a slash command. skills watch the conversation and act when the moment is right.

each skill lives in its own subdirectory with a SKILL.md file.

the SKILL.md uses YAML frontmatter to describe when to activate.

when you say "review this PR for security issues," claude reads the description, recognizes it matches, and invokes the skill automatically. or you call it directly: /security-review.

the key difference from commands: skills can bundle supporting files alongside them. the @DETAILED_GUIDE.md reference pulls in a detailed document that lives right next to SKILL.md. commands are single files. skills are packages.

the /last30days skill that went viral on X is a perfect example, someone built a skill that scans Reddit and X from the last 30 days on any topic, synthesizes what the community has figured out, and writes you ready-to-use prompts. type /last30days prompting techniques for legal questions and it returns frameworks real lawyers and power users are actually using. that's the kind of thing you can build with skills.

personal skills go in ~/.claude/skills/ and are available across all your projects.

### Plan mode: think before you build

this one saved me from myself more times than i can count.

before you let claude loose on a big refactor:

claude explores your codebase, reads the relevant files, analyzes the architecture then presents a plan. no changes made. nothing modified. just analysis and a proposed approach.

you review it. adjust it. ask questions. poke holes. then when you're confident, approve and let it implement.

this prevents the "claude rewrote my entire project and i didn't ask for that" moment. trust me, that moment is not fun. plan first, build second. always.

### Memory system: it gets smarter the more you use it

claude remembers things across sessions. automatically.

correct it once "don't use class components in this project, we use hooks" and it saves that preference. next session, it already knows. you don't repeat yourself.

manage it with /memory. stored at ~/.claude/projects/<project>/memory/.

the combination of CLAUDE.md (team knowledge) + auto memory (personal learning) means claude compounds. it gets better the more you use it. week 1 claude and week 8 claude on the same project are different animals.

### Computer use

this one dropped in march 2026 and it's wild.

claude can now control your computer directly:

- open applications
- navigate browsers
- fill out spreadsheets
- interact with any GUI
- take screenshots and react to what it sees

no setup required. works from your phone via remote control.

i haven't gone deep on this one yet still experimenting. but the implications are insane. imagine telling claude to "open figma, screenshot the latest design, then implement it in react." we're getting there.

### Power user workflows

these are the patterns that took me from "using claude code" to "being dangerous with claude code." none of this is in the docs.

#### The "interview me" technique

starting a complex project? don't write a massive prompt. don't try to think of everything upfront. just say:

let claude ask YOU the questions. it'll ask about tech stack, requirements, edge cases, existing code, deployment targets, user types things you'd forget to mention in a prompt. 10 minutes of back-and-forth gives claude better context than a 500-word prompt ever could.

i use this for every new feature now. every single one.

#### The research -> implement split

never let claude implement something it doesn't understand first.

the quality difference between "just build it" and "understand it first, then build it" is night and day. especially on legacy codebases where nothing is where you'd expect it.

#### Parallel work with worktrees

each gets an isolated git branch. two features developed simultaneously by two separate claude sessions. merge when ready.

i run 3 worktrees sometimes. auth feature in terminal 1, API endpoint in terminal 2, test suite in terminal 3. all running in parallel. all isolated. it's obscene productivity.

#### The context management game

this is the #1 skill that separates good claude code users from great ones.

the context window is ~200K tokens. sounds like a lot. it's not when you're deep in a session. as it fills up:

- older tool outputs get cleared first
- conversation gets auto-summarized
- early instructions may be lost

manage it actively:

the golden rule: after two failed attempts at something, stop. don't keep going. /clear and write a better initial prompt from scratch. a fresh context with a clear prompt almost always works better than a polluted context full of failed approaches. i learned this at the cost of about 6 hours of wasted time.

#### The ! prefix trick

type ! before any command to run it directly in your shell without claude:

the output gets added to claude's context automatically. great for quickly showing claude what's happening without asking it to run the command and waiting for permission prompts.

#### External editor for long prompts

Ctrl+G opens your system editor (vim, VS Code, whatever you have set). write complex multi-line prompts with syntax highlighting. save and close it sends to claude.

way better than typing a paragraph into a single-line terminal input. i use this for every prompt longer than 2 sentences now.

#### The double-escape rewind

this one's clutch and nobody mentions it.

claude went the wrong direction? double-tap Escape, and you get a rewind menu. you can undo the last action, go back further, or "summarize from here" which compresses the failed attempt while keeping the useful context. way better than /clear because you don't lose everything, just the mistake.

![Keyboard shortcuts reference](https://assets.han-ws.workers.dev/i/2026/03/tweet-2037978621684621428-7.png)

print this table. tape it to your monitor. seriously.

### Permission modes

claude code has different levels of autonomy. knowing when to use each is important:

![Permission modes comparison](https://assets.han-ws.workers.dev/i/2026/03/tweet-2037978621684621428-8.png)

start with default. move to acceptEdits once you trust it (took me about a week). use plan for exploration before big refactors.

you can also whitelist and blacklist specific commands in .claude/settings.json.

the allow list runs without asking. the deny list is blocked entirely. anything not in either list claude asks first. that middle ground is intentional. safety net without micromanaging every command.

### Claude code vs cursor vs copilot

the honest comparison. no fanboy energy. just what i've experienced:

**claude code**: terminal-native, truly agentic. reads your entire codebase. autonomous multi-file changes. best for: complex refactors, architecture decisions, codebase-wide changes, debugging gnarly issues, anything touching more than 3 files. feels like a calm senior engineer who never gets tired and never judges your code.

**cursor** IDE-native (VS Code fork). best daily driver for tab completions and inline suggestions. tighter feedback loop in the editor. less autonomous but faster for small changes. best for: quick edits, inline completions, staying in flow while writing code.

**github copilot**: plugin approach. best free tier ($10/mo). safest corporate choice. agentic capabilities are improving but still lag behind both claude code and cursor.

here's what most people don't realize: you don't have to pick one.

the combo strategy is real and it's what most power users actually do. cursor for daily tab completions and inline edits. claude code for the heavy lifting refactors, new features, debugging, architecture decisions.

the data backs this up: experienced developers use 2.3 AI coding tools on average. it's not about replacing one with the other. it's about using the right tool for the right job.

claude code went from zero to the #1 most loved AI coding tool in under a year. 46% of developers rated it their favorite in early 2026 surveys. 95% of developers now use AI tools weekly. 75% use AI for more than half their coding.

the game has changed. permanently. the question isn't whether to use AI coding tools. it's how fast you can get good at them.

### The claude code ecosystem

the community has built an insane amount of tooling on top of claude code. here are the ones worth knowing:

**superpowers** (obra/superpowers): a full development methodology for AI coding agents. 117K+ stars on github. it changes how your agent writes code forces it to brainstorm first, create detailed implementation plans, launch subagent-driven development, and do two-stage code review before declaring done. if you feel like claude code produces "spaghetti" sometimes, this is the fix.

**GSD get shit done** (gsd-build/get-shit-done) a meta-prompting and context management system for claude code. lightweight but powerful. solves the context degradation problem.

**awesome-claude-code** community-curated collection of the best resources, skills, agents, and MCP servers. your starting point for discovering what's out there.

**/last30days skill** the one that went viral. scans Reddit, X, YouTube, Hacker News, and the web from the last 30 days on any topic you give it. synthesizes community knowledge into ready-to-use prompts. type /last30days cold email frameworks and it finds the 3 Ps framework, ADA, intention-based triggers stuff you'd never find on your own. then writes you ready-to-use prompts based on what actually works. open source, MIT license.

**claude how-to** visual, example-driven guide to mastering claude code. best resource for visual learners who want structured tutorials.

**claude mem** persistent memory layer. if the built-in memory system isn't enough for your workflow.

**UI UX pro max** design-focused tooling for claude code. for when you want claude to care about how things look, not just how they work.

**n8n-MCP** connects claude code to n8n workflow automation. if you're already using n8n, this is a no-brainer integration.

the ecosystem is growing fast. new tools every week. these are the ones i've actually used or seen produce real results. not a curated list of everything just the ones that matter right now.

### Common mistakes (i made all of these)

1. **writing a novel in CLAUDE.md** keep it under 200 lines. specific. actionable. if claude's ignoring your instructions, your CLAUDE.md is probably too long. i had a 400-line one that was basically a manifesto. cut it to 150, everything improved.

2. **not using /clear between tasks** a conversation about fixing a CSS bug has zero useful context for implementing a new API endpoint. the leftover context actively hurts performance. clear it. start fresh. every time.

3. **fighting it instead of restarting** after two failed attempts, the context is polluted with wrong approaches. claude starts referencing its own mistakes. /clear and write a better prompt. the fresh context almost always nails it. i wasted 6 hours once before learning this.

4. **not using subagents** running a full test suite in your main conversation floods the context with hundreds of lines of output. delegate to a subagent. keep your main conversation clean and focused on the actual work.

5. **using opus for everything** sonnet handles 90% of tasks perfectly. opus is for the 10% that actually needs deep reasoning complex architecture, tricky debugging, subtle logic errors. your wallet will thank you. mine did.

6. **ignoring plan mode for big changes** always plan first on anything touching more than 3 files. always. the 5 minutes you spend reviewing a plan saves you 2 hours of cleanup when claude goes in the wrong direction.

7. **not managing context actively** the context window is a resource. treat it like one. /compact when it's getting long. subagents for verbose operations. CLAUDE.md for instructions instead of repeating them every session. the people who manage context well get dramatically better results than those who don't.

### The bottom line

claude code isn't a tool. it's a teammate.

one that reads your entire codebase, follows your coding standards, runs your tests, creates your PRs, remembers your preferences, connects to all your tools, and gets better the more you use it.

i've been at this for 2 months. i've done the courses, got the certs, built projects daily. and i can tell you with absolute confidence the developers who learn to work WITH claude code, not just paste code at it, are shipping at a pace that would've been unthinkable a year ago.

it's 2026. you have access to an AI agent that can autonomously navigate a 50,000-line codebase, make surgical multi-file changes, run your tests, fix its own bugs, and create pull requests with documentation. your ancestors would have dreamed of this.

stop bookmarking articles about AI. start building with it.

## Key Takeaways

- Claude Code is an autonomous agentic coding tool, not autocomplete. It reads your whole codebase, plans, edits multi-file, runs tests, and iterates in a loop.
- CLAUDE.md is the highest-leverage file: keep it under 200 lines, specific and actionable. Use rules/ folder for modular, path-scoped instructions that scale with teams.
- Match model to task: Sonnet for 90% of work, Opus for complex reasoning, Haiku for cheap subagent tasks. The author wasted $200 in week 1 using Opus for everything.
- Context management is the #1 differentiator: /clear between tasks, use subagents for verbose operations, and after 2 failed attempts always start fresh.
- The combo strategy works: Cursor for daily inline edits + Claude Code for heavy refactors and architecture. Experienced devs use 2.3 AI coding tools on average.
- Hooks make behavior deterministic (unlike CLAUDE.md which is suggestive). Use PreToolUse for security gates, Stop hooks for quality gates.
- Skills are reusable workflow packages that auto-activate based on context. The /last30days viral skill is a good example of community-built tooling.

## Related

- [[claude-code-hook-lifecycle-and-event-system]] - deep dive into the hooks system summarized here
- [[commands-vs-hooks-vs-skills-decision-framework]] - when to use commands vs hooks vs skills, expanding on the distinction made in this guide
- [[ai-dev-stack-8-layer-model-march-2026]] - the 8-layer stack model that positions Claude Code at L3 with surrounding tooling layers
- [[claude-dispatch-workflows-and-async-ai-orchestration-from-mobile]] - the Dispatch/remote surface covered briefly here, explored in full