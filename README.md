# Learned

> Auto-generated index of 62 note(s). Last updated: 2026-04-13

## ai

- [Claude dispatch workflows and async AI orchestration from mobile](ai/claude-dispatch-workflows-and-async-ai-orchestration-from-mobile.md) — Orchestrate 60+ parallel AI sessions from your phone; knowledge layer compounds across surfaces
- [Complete guide to Claude Code features workflows and ecosystem](ai/complete-guide-to-claude-code-features-workflows-and-ecosystem.md) — Practitioner's guide: agentic loop, CLAUDE.md under 200 lines, Sonnet for 90% of tasks
- [LLM agent memory systems landscape 2026](ai/llm-agent-memory-systems-landscape-2026.md) — Memory systems all solve a 5-stage pipeline; differentiation is in structure around LLM decisions
- [LLM memory benchmarks and evaluation crisis](ai/llm-memory-benchmarks-and-evaluation-crisis.md) — LoCoMo has ~99 wrong answers; no trustworthy single benchmark exists for memory systems
- [LLM memory systems three competitive battlegrounds](ai/llm-memory-systems-three-competitive-battlegrounds.md) — Write/update loop absorbs 80% of innovation; all systems delegate conflict resolution to LLMs
- [Memory systems as agent harness plugins](ai/memory-systems-as-agent-harness-plugins.md) — Memory integrates via two lifecycle hooks: before-turn recall, after-turn capture
- [Multi-agent coding brain rot scan design](ai/multi-agent-coding-brain-rot-scan-design-externalized-state-clean-handoffs.md) — Fighter pilot scan patterns fix the brain rot of running 5+ AI agents in parallel

## ai-tooling

- [AI dev stack 8-layer model](ai-tooling/ai-dev-stack-8-layer-model-march-2026.md) — AI dev stack is 8 layers; L3.5 splits into codebase intelligence vs external docs
- [AI dev stack 8-layer model with tool evaluations](ai-tooling/ai-dev-stack-8-layer-model-with-tool-evaluations-march-2026.md) — Expanded stack model with SDD framework comparisons, tool scores, and 6-phase workflow
- [Autoresearch: the Karpathy loop pattern](ai-tooling/autoresearch-the-karpathy-loop-pattern.md) — Three-file contract (goal + artifact + frozen eval) ratchets quality via automated experiments
- [ClaudeKit deep dive: session recovery, red team and gaps](ai-tooling/claudekit-deep-dive-session-recovery-red-team-and-gaps.md) — ClaudeKit saves state on Stop hook, not just PreCompact; ship pipeline more complete than ours
- [ClaudeKit evaluation and unique features](ai-tooling/claudekit-evaluation-and-unique-features.md) — 50+ commands, interview-style spec gate, 4 adversarial reviewers; scored BOOKMARK (10/15)
- [Code graph context tools for token reduction](ai-tooling/code-graph-context-tools-for-token-reduction.md) — AST-based code graphs cut agent token usage 40-95% by replacing grep with structured queries
- [Context Hub vs Context7 vs the context layer ecosystem](ai-tooling/context-hub-vs-context7-vs-the-context-layer-ecosystem.md) — Context Hub = curated API docs with feedback loop; Context7 = 9k+ library docs via MCP
- [Prompt improvement as a learning technique](ai-tooling/prompt-improvement-as-a-learning-technique.md) — Sharpening vague prompts into structured ones is a thinking tool, not just better answers
- [Tool evaluation 5-question rubric](ai-tooling/tool-evaluation-5-question-rubric.md) — 5 questions in 10 min; the kill question: what past failure would this have prevented?

## claude-code

- [Claude Code ecosystem repo evaluations for kit building](claude-code/claude-code-ecosystem-repo-evaluations-for-kit-building.md) — Evaluated 7 repos; methodology + hardening tools don't overlap, integration gap is the opportunity
- [Claude Code hook lifecycle and event system](claude-code/claude-code-hook-lifecycle-and-event-system.md) — 21 hook events with exit code 2 as the only real enforcement; hooks beat CLAUDE.md rules
- [Claude Code hook schema decision values per event type](claude-code/claude-code-hook-schema-decision-values-per-event-type.md) — Stop hooks need "approve"/"block", not "allow"/"deny"; mixing causes silent validation errors
- [Commands vs hooks vs skills decision framework](claude-code/commands-vs-hooks-vs-skills-decision-framework.md) — If skipping it causes irreversible damage, use a hook; if output degrades, use a skill
- [Compaction defense patterns for Claude Code sessions](claude-code/compaction-defense-patterns-for-claude-code-sessions.md) — Two-layer defense: PreCompact backup + post-compaction re-injection of critical rules

## cs

- [Turing completeness](cs/turing-completeness.md) — A system needs branching, looping, and unbounded memory to compute anything

## devtools

- [Starship prompt configuration best practices](devtools/starship-prompt-configuration-best-practices.md) — Start from a preset, use $fill for right-alignment, disable 90% of modules
- [XDG base directory specification](devtools/xdg-base-directory-specification.md) — XDG separates config/data/state/cache into standard dirs; simplifies dotfile management

## diaspora

- [**Vietnamese diaspora synthesis**](diaspora/vietnamese-diaspora-synthesis.md) — Synthesis: structural gap (institutions, not culture) drives hollowing; bridge-builder model is the fix
- [Four Asian diasporas 30-year projection](diaspora/four-asian-diasporas-30-year-projection.md) — Four variables (homeland gravity, institutions, soft power, intermarriage) predict survival by 2055
- [Four Asian diasporas in 2055 projected trajectories](diaspora/four-asian-diasporas-in-2055-projected-trajectories.md) — Japanese dissolve, Korean persist culturally, Chinese endure institutionally, Vietnamese at crossroads
- [The bridge-builder model](diaspora/the-bridge-builder-model-highest-value-position-for-the-next-vietnamese-generati.md) — Bicultural fluency connecting Vietnam's rising economy to global capital beats enclave and integration
- [Vietnamese vs Chinese diaspora: structural analysis](diaspora/vietnamese-vs-chinese-diaspora-a-structural-analysis-of-divergent-outcomes.md) — Five factors (time, migration type, institutions, legal status, motherland) explain the gap
- [Vietnamese vs Chinese diaspora: why one builds hubs](diaspora/vietnamese-vs-chinese-diaspora-why-one-builds-economic-hubs-and-the-other-doesnt.md) — Refugees without merchant skills or institutions can't replicate 1,500-year trade networks
- [Why Little Saigons hollow out](diaspora/why-little-saigons-hollow-out-the-success-driven-exit-problem.md) — Successful kids leave enclaves; renters not owners means no community anchor survives the exit
- [Why Vietnamese built nail salons instead of trade empires](diaspora/why-vietnamese-built-nail-salons-instead-of-trade-empires-the-subsistence-busine.md) — Tippi Hedren's manicurist created a refugee entry point; subsistence businesses rational to escape

## dwarves-kit

- [Building Dwarves Kit from extracted patterns](dwarves-kit/building-dwarves-kit-from-extracted-patterns.md) — Cherry-picked battle-tested patterns from 6+ repos; synthesize, don't originate
- [Dwarves Kit design philosophy and architecture](dwarves-kit/dwarves-kit-design-philosophy-and-architecture.md) — 7 principles: guardrails over guidance, bash over binaries, shallow+wide
- [Dwarves Kit self-assessment against philosophy](dwarves-kit/dwarves-kit-self-assessment-against-philosophy.md) — Ran /kit-health on itself; all 7 principles upheld, file count within budget
- [Dwarves Kit v1.2 agent roster and CDP](dwarves-kit/dwarves-kit-v1-2-agent-roster-and-cdp.md) — 8 specialized agents + collaborative design protocol with 3 decision modes
- [Dwarves Kit v1.2 ClaudeKit patterns adopted](dwarves-kit/dwarves-kit-v1-2-claudekit-patterns-adopted.md) — Adopted session-state save hook and ship pipeline from ClaudeKit
- [Dwarves Kit v1.2 five open decisions](dwarves-kit/dwarves-kit-v1-2-five-open-decisions.md) — Resolved: stay native Task tool, adopt codebase-memory-mcp, GSD v1 architecture
- [Dwarves Kit v1.2 verification pipeline architecture](dwarves-kit/dwarves-kit-v1-2-verification-pipeline-architecture.md) — Verify-fix-retry loop: read-only verifier, scoped fix-agent, max 2 retries
- [SDD landscape and Dwarves Kit v1.2 reference map](dwarves-kit/sdd-landscape-and-dwarves-kit-v1-2-reference-map.md) — Single source of truth: 8-layer stack, all tool scores, full kit inventory
- [SDD multi-agent verification architecture](dwarves-kit/sdd-multi-agent-verification-architecture.md) — 5 gaps found in v1.1; phases 1-2 built, parallel agent teams deferred

## geopolitics

- [Australia's Washminster government structure](geopolitics/australias-washminster-government-structure.md) — Australia blends Westminster parliament with US federalism; federal/state split complicates crisis response
- [How the 2026 Strait of Hormuz crisis impacts Australia](geopolitics/how-the-2026-strait-of-hormuz-crisis-impacts-australia.md) — Hormuz closure hits Australia via Asian refineries; only 29-39 days fuel reserves
- [Measuring oil supply disruption severity](geopolitics/measuring-oil-supply-disruption-severity-2026-hormuz-vs-historical-crises.md) — 2026 Hormuz removed 12 mb/d, largest oil disruption ever; 400 mb strategic reserve release

## history

- [China as a civilization state, not a nation state](history/china-as-a-civilization-state-not-a-nation-state.md) — China is a 2,000-year civilization wearing nation-state clothing; continuity and order trump liberty
- [Imperial examinations: how China replaced religion with meritocracy](history/imperial-examinations-how-china-replaced-religion-with-meritocracy.md) — 1,300-year exam system created cultural unity and loyal bureaucracy without a holy book
- [Sinicization: how China absorbs its conquerors](history/sinicization-how-china-absorbs-its-conquerors.md) — Every conqueror adopted Chinese civilization because demographics and bureaucracy made it inevitable
- [The Tainter trap: why complexity kills empires](history/the-tainter-trap-why-complexity-kills-empires-and-chinas-reset-mechanism.md) — Empires die from complexity overload; China survives by expecting collapse and having a reset protocol
- [Predictive history and the ambition of psycho-history](history/predictive-history-and-the-ambition-of-psycho-history.md) — Prof Jiang aims to build Asimov's psycho-history: connect past, explain present, predict future

## health

- [Alkaline water health claims vs reality](health/alkaline-water-health-claims-vs-reality.md) — Stomach acid neutralizes alkaline water instantly; premium price buys marketing

## investing

- [Compound interest levels and lifestyle progression](investing/compound-interest-levels-and-lifestyle-progression.md) — Six wealth levels from $200/mo saver to $200M foundation; instruments change at each threshold

## mcp

- [MCP tool schema caching in Claude.ai connectors](mcp/mcp-tool-schema-caching-in-claude-ai-connectors.md) — Claude.ai caches MCP schemas per session; disconnect+reconnect to force refresh
- [Security gates for MCP tools that bridge private to public](mcp/security-gates-for-mcp-tools-that-bridge-private-to-public.md) — Server-side security gates: context-anchored secret scan, path traversal, cost-ordered pipeline

## patterns

- [Redundant API pre-checks in wrapper functions](patterns/redundant-api-pre-checks-in-wrapper-functions.md) — Wrapper checks file existence, then library re-checks internally; doubled API calls

## pkm

- [LLM Wiki pattern: compilation over retrieval](pkm/llm-wiki-pattern-compilation-over-retrieval.md) — LLM compiles raw sources into interlinked wiki instead of re-deriving via RAG each time
- [Why knowledge notes need context, not just facts](pkm/why-knowledge-notes-need-context-not-just-facts.md) — Default capture depth was TIL (shallow); changing default to Atomic Note fixed quality

## wealth

- [How to sit at the table: the thesis](wealth/how-to-sit-at-the-table-the-thesis.md) — Seek opportunity access and judgment transfer from elders, not money
- [The three gates: what elders screen for](wealth/the-three-gates-what-elders-screen-for.md) — Invisible character, value, and role tests that gatekeep inner circles
- [The 12-month progression: deposit to partnership](wealth/the-12-month-progression-deposit-to-partnership.md) — Trust builds in 4 phases over 12 months; most quit by month 3
- [Anti-patterns that destroy trust permanently](wealth/anti-patterns-that-destroy-trust-permanently.md) — Catalog of trust-killing behaviors; gossip and info leaks are unrecoverable
- [Enterprise trust ladder: vendor to strategic partner](wealth/enterprise-trust-ladder-vendor-to-strategic-partner.md) — Five rungs from cold contact to strategic partner; pilot delivery is 60% process

## youtube

- [YouTube transcript extraction from cloud containers](youtube/youtube-transcript-extraction-from-cloud-containers.md) — Node.js fetch ignores HTTPS_PROXY; fix with undici ProxyAgent for YouTube transcripts
