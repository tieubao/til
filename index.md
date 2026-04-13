# Index

> Full catalog of 106 note(s) across 19 folders. Last updated: 2026-04-13

## ai

- [Claude dispatch workflows and async AI orchestration from mobile](ai/claude-dispatch-workflows-and-async-ai-orchestration-from-mobile.md) - Orchestrate 60+ parallel AI sessions from your phone; knowledge layer compounds across surfaces
- [Complete guide to Claude Code features workflows and ecosystem](ai/complete-guide-to-claude-code-features-workflows-and-ecosystem.md) - Practitioner's guide: agentic loop, CLAUDE.md under 200 lines, Sonnet for 90% of tasks
- [LLM agent memory systems landscape 2026](ai/llm-agent-memory-systems-landscape-2026.md) - Memory systems all solve a 5-stage pipeline; differentiation is in structure around LLM decisions
- [LLM memory benchmarks and evaluation crisis](ai/llm-memory-benchmarks-and-evaluation-crisis.md) - LoCoMo has ~99 wrong answers; no trustworthy single benchmark exists for memory systems
- [LLM memory systems three competitive battlegrounds](ai/llm-memory-systems-three-competitive-battlegrounds.md) - Write/update loop absorbs 80% of innovation; all systems delegate conflict resolution to LLMs
- [Memory systems as agent harness plugins](ai/memory-systems-as-agent-harness-plugins.md) - Memory integrates via two lifecycle hooks: before-turn recall, after-turn capture
- [Multi-agent coding brain rot scan design](ai/multi-agent-coding-brain-rot-scan-design-externalized-state-clean-handoffs.md) - Fighter pilot scan patterns fix the brain rot of running 5+ AI agents in parallel

## ai-tooling

- [AI dev stack 8-layer model with tool evaluations](ai-tooling/ai-dev-stack-8-layer-model-with-tool-evaluations-march-2026.md) - 8-layer stack model with SDD framework comparisons, tool scores, and 6-phase workflow
- [AutoResearch: the Karpathy loop pattern](ai-tooling/autoresearch-the-karpathy-loop-pattern.md) - Three-file contract (goal + artifact + frozen eval) ratchets quality via automated experiments
- [ClaudeKit deep dive: session recovery, red team and gaps](ai-tooling/claudekit-deep-dive-session-recovery-red-team-and-gaps.md) - ClaudeKit saves state on Stop hook, not just PreCompact; ship pipeline more complete than ours
- [ClaudeKit evaluation and unique features](ai-tooling/claudekit-evaluation-and-unique-features.md) - 50+ commands, interview-style spec gate, 4 adversarial reviewers; scored BOOKMARK (10/15)
- [Code graph context tools for token reduction](ai-tooling/code-graph-context-tools-for-token-reduction.md) - AST-based code graphs cut agent token usage 40-95% by replacing grep with structured queries
- [Context Hub vs Context7 vs the context layer ecosystem](ai-tooling/context-hub-vs-context7-vs-the-context-layer-ecosystem.md) - Context Hub = curated API docs with feedback loop; Context7 = 9k+ library docs via MCP
- [Prompt improvement as a learning technique](ai-tooling/prompt-improvement-as-a-learning-technique.md) - Sharpening vague prompts into structured ones is a thinking tool, not just better answers
- [Tool evaluation 5-question rubric](ai-tooling/tool-evaluation-5-question-rubric.md) - 5 questions in 10 min; the kill question: what past failure would this have prevented?

## claude-code

- [Claude Code ecosystem repo evaluations for kit building](claude-code/claude-code-ecosystem-repo-evaluations-for-kit-building.md) - Evaluated 7 repos; methodology + hardening tools don't overlap, integration gap is the opportunity
- [Claude Code hook lifecycle and event system](claude-code/claude-code-hook-lifecycle-and-event-system.md) - 21 hook events with exit code 2 as the only real enforcement; hooks beat CLAUDE.md rules
- [Claude Code hook schema decision values per event type](claude-code/claude-code-hook-schema-decision-values-per-event-type.md) - Stop hooks need "approve"/"block", not "allow"/"deny"; mixing causes silent validation errors
- [Commands vs hooks vs skills decision framework](claude-code/commands-vs-hooks-vs-skills-decision-framework.md) - If skipping it causes irreversible damage, use a hook; if output degrades, use a skill
- [Compaction defense patterns for Claude Code sessions](claude-code/compaction-defense-patterns-for-claude-code-sessions.md) - Two-layer defense: PreCompact backup + post-compaction re-injection of critical rules

## cs

- [Turing completeness](cs/turing-completeness.md) - A system needs branching, looping, and unbounded memory to compute anything
- [Why interviewers ask linked list questions](cs/why-linked-list-interview-questions.md) - Linked list interviews are a cultural artifact of 1980s C pointer manipulation, not a timeless test
- [Why Vim uses hjkl for navigation](cs/why-vim-uses-hjkl.md) - Chain of accidents from 1967 ASCII table to ADM-3A terminal to Bill Joy's vi

## devtools

- [Starship prompt configuration best practices](devtools/starship-prompt-configuration-best-practices.md) - Start from a preset, use $fill for right-alignment, disable 90% of modules
- [XDG base directory specification](devtools/xdg-base-directory-specification.md) - XDG separates config/data/state/cache into standard dirs; simplifies dotfile management

## diaspora

- [**Vietnamese diaspora synthesis**](diaspora/vietnamese-diaspora-synthesis.md) - Synthesis: structural gap (institutions, not culture) drives hollowing; bridge-builder model is the fix
- [Four Asian diasporas in 2055 projected trajectories](diaspora/four-asian-diasporas-in-2055-projected-trajectories.md) - Four variables predict diaspora survival by 2055; Vietnamese at crossroads, Chinese endure
- [The bridge-builder model](diaspora/the-bridge-builder-model-highest-value-position-for-the-next-vietnamese-generati.md) - Bicultural fluency connecting Vietnam's rising economy to global capital beats enclave and integration
- [Vietnamese vs Chinese diaspora: structural analysis](diaspora/vietnamese-vs-chinese-diaspora-a-structural-analysis-of-divergent-outcomes.md) - Five structural factors explain the gap; escape-through-education trap; bridge-builder alternative
- [Why Little Saigons hollow out](diaspora/why-little-saigons-hollow-out-the-success-driven-exit-problem.md) - Successful kids leave enclaves; renters not owners means no community anchor survives the exit
- [Why Vietnamese built nail salons instead of trade empires](diaspora/why-vietnamese-built-nail-salons-instead-of-trade-empires-the-subsistence-busine.md) - Tippi Hedren's manicurist created a refugee entry point; subsistence businesses rational to escape

## dwarves-kit

- [Building Dwarves Kit from extracted patterns](dwarves-kit/building-dwarves-kit-from-extracted-patterns.md) - Cherry-picked battle-tested patterns from 6+ repos; synthesize, don't originate
- [Dwarves Kit design philosophy and architecture](dwarves-kit/dwarves-kit-design-philosophy-and-architecture.md) - 7 principles: guardrails over guidance, bash over binaries, shallow+wide
- [Dwarves Kit self-assessment against philosophy](dwarves-kit/dwarves-kit-self-assessment-against-philosophy.md) - Ran /kit-health on itself; all 7 principles upheld, file count within budget
- [Dwarves Kit v1.2 agent roster and CDP](dwarves-kit/dwarves-kit-v1-2-agent-roster-and-cdp.md) - 8 specialized agents + collaborative design protocol with 3 decision modes
- [Dwarves Kit v1.2 ClaudeKit patterns adopted](dwarves-kit/dwarves-kit-v1-2-claudekit-patterns-adopted.md) - Adopted session-state save hook and ship pipeline from ClaudeKit
- [Dwarves Kit v1.2 five open decisions](dwarves-kit/dwarves-kit-v1-2-five-open-decisions.md) - Resolved: stay native Task tool, adopt codebase-memory-mcp, GSD v1 architecture
- [Dwarves Kit v1.2 verification pipeline architecture](dwarves-kit/dwarves-kit-v1-2-verification-pipeline-architecture.md) - Verify-fix-retry loop: read-only verifier, scoped fix-agent, max 2 retries
- [SDD landscape and Dwarves Kit v1.2 reference map](dwarves-kit/sdd-landscape-and-dwarves-kit-v1-2-reference-map.md) - Single source of truth: 8-layer stack, all tool scores, full kit inventory
- [SDD multi-agent verification architecture](dwarves-kit/sdd-multi-agent-verification-architecture.md) - 5 gaps found in v1.1; phases 1-2 built, parallel agent teams deferred

## engineering

- [The antipattern scripting language](engineering/antipattern-scripting-language.md) - Antipatterns are contextual; in throwaway scripts, they become good ideas that speed completion
- [Data drives code structure](engineering/data-drives-code-structure.md) - Software structure follows data structure: arrays become loops, graphs become traversals
- [Discipline doesn't scale](engineering/discipline-doesnt-scale.md) - Calls for discipline fail because there is no motivation to adopt them; change the environment instead
- [No primitives - model domain concepts with types](engineering/no-primitives-domain-modeling.md) - Primitive Obsession code smell; wrap domain concepts in types to enforce invariants at construction
- [Programming practices - Unix philosophy and beyond](engineering/programming-practices-principles.md) - Timeless principles: prototype first, fail noisily, separate policy from mechanism, least surprise
- [The purple developer - 10x productivity is contextual](engineering/purple-developer-10x-myth.md) - The 10x engineer is the one who built the system; spread the knowledge, spread the productivity
- [What if GitHub is the devil - curl's pragmatic take](engineering/what-if-github-is-the-devil.md) - Daniel Stenberg on why curl stays on GitHub: network effect, contingency plans, pragmatism over purity
- [Bit Twiddling Hacks](engineering/bit-twiddling-hacks.md) - Stanford reference of bitwise tricks: branchless abs, popcount, De Bruijn log, Morton interleaving
- [Choose Boring Technology](engineering/choose-boring-technology.md) - Finite innovation tokens; spend them on business problems, not infrastructure novelty
- [Egoless Engineering](engineering/egoless-engineering.md) - Ego and parochialism destroy orgs; domain experts who teach beat domain owners who gatekeep
- [Good and bad Elixir](engineering/good-and-bad-elixir.md) - Anti-patterns: piping side effects, over-using with, hiding higher-order functions
- [Why big tech companies are so slow](engineering/why-big-tech-is-slow.md) - Feature interaction complexity grows combinatorially; slowness is math, not incompetence

## geopolitics

- [Australia's Washminster government structure](geopolitics/australias-washminster-government-structure.md) - Australia blends Westminster parliament with US federalism; federal/state split complicates crisis response
- [How the 2026 Strait of Hormuz crisis impacts Australia](geopolitics/how-the-2026-strait-of-hormuz-crisis-impacts-australia.md) - Hormuz closure hits Australia via Asian refineries; only 29-39 days fuel reserves
- [Measuring oil supply disruption severity](geopolitics/measuring-oil-supply-disruption-severity-2026-hormuz-vs-historical-crises.md) - 2026 Hormuz removed 12 mb/d, largest oil disruption ever; 400 mb strategic reserve release

## health

- [Alkaline water health claims vs reality](health/alkaline-water-health-claims-vs-reality.md) - Stomach acid neutralizes alkaline water instantly; premium price buys marketing
- [Vitamins and longevity stack](health/vitamins-and-longevity-stack.md) - Daily supplement stack for anti-aging: NMN, Omega-3, Magnesium, CoQ10, and 12 more with dosages

## history

- [China as a civilization state, not a nation state](history/china-as-a-civilization-state-not-a-nation-state.md) - China is a 2,000-year civilization wearing nation-state clothing; continuity and order trump liberty
- [Imperial examinations: how China replaced religion with meritocracy](history/imperial-examinations-how-china-replaced-religion-with-meritocracy.md) - 1,300-year exam system created cultural unity and loyal bureaucracy without a holy book
- [Predictive history and the ambition of psycho-history](history/predictive-history-and-the-ambition-of-psycho-history.md) - Prof Jiang aims to build Asimov's psycho-history: connect past, explain present, predict future
- [Sinicization: how China absorbs its conquerors](history/sinicization-how-china-absorbs-its-conquerors.md) - Every conqueror adopted Chinese civilization because demographics and bureaucracy made it inevitable
- [The Tainter trap: why complexity kills empires](history/the-tainter-trap-why-complexity-kills-empires-and-chinas-reset-mechanism.md) - Empires die from complexity overload; China survives by expecting collapse and having a reset protocol

## investing

- [Compound interest levels and lifestyle progression](investing/compound-interest-levels-and-lifestyle-progression.md) - Six wealth levels from $200/mo saver to $200M foundation; instruments change at each threshold
- [How and why I invest in startups](investing/how-and-why-i-invest-in-startups.md) - Fund the best people on the hardest problems; measure both LP returns and happiness

## leadership

- [The consulting secret - ask your senior ICs what is broken](leadership/consulting-secret-ask-the-ics.md) - Schedule 90 min with your best IC, ask what is broken, read it back to leadership
- [HR evaluation as unique value measurement](leadership/hr-evaluation-unique-value.md) - Market value = Differentiation x Influence; uniqueness beats commoditized skill checklists
- [In pursuit of excellence](leadership/in-pursuit-of-excellence.md) - Excellence comes from unique positioning at domain intersections, not being best at common skills
- [Lam an kieu Cu Ho](leadership/lam-an-kieu-cu-ho.md) - Five business lessons from Ho Chi Minh's methods, applied at FPT Software by Nguyen Thanh Nam
- [Masayoshi Son and the SoftBank Vision Fund](leadership/masayoshi-son-softbank-vision.md) - $100B fund betting AI runs the planet; gun-senryaku connects portfolio companies in a flock
- [Steve Jobs negotiation and persuasion tactics](leadership/steve-jobs-negotiation-tactics.md) - Pitch with passion, be brutally honest, earn respect through work ethic, disarm with charm
- [Tips on working with talents](leadership/tips-on-working-with-talents.md) - Four angles on using talent: genuinely need them, create worthy challenges, fair treatment, tolerate quirks

## life

- [100 little ideas](life/100-little-ideas.md) - Morgan Housel's reference card of psychological and behavioral concepts; mental models for how the world actually works
- [Always be quitting: make yourself replaceable to grow](life/always-be-quitting.md) - Being indispensable is a trap; 10 practices to make yourself replaceable and unlock growth
- [Average Joe](life/average-joe.md) - An ordinary person in denial can still push far; the value is in refusing to stop trying
- [Be dispassionate about your software career](life/be-dispassionate-about-software-careers.md) - Passion is a vulnerability employers exploit; invest in skills for self-preservation, not applause
- [Chon nguoi hop tac va ket giao](life/chon-nguoi-hop-tac-va-ket-giao.md) - Ancient wisdom: 6 types to avoid in business, 7 to not befriend, 4 to keep close
- [Dang Le Nguyen Vu - 9 bai hoc nhan tinh the thai](life/dang-le-nguyen-vu-nhan-tinh-the-thai.md) - Nine life lessons from Trung Nguyen's founder: self-reflect, build inner strength, accept impermanence
- [Great minds discuss ideas](life/great-minds-discuss-ideas.md) - Three levels of discourse: people, events, ideas; the problem is when people become the endpoint
- [Hygge - the Danish concept of cosiness](life/hygge-danish-concept-of-cosiness.md) - Hygge is a feeling of cosiness, not a lifestyle product; Danes created it to survive dark winters
- [John Vu on world class quality](life/john-vu-on-world-class-quality.md) - Bill Gates observed: small civic behaviors reveal a country's education quality and national class
- [Laziness does not exist, only unmet barriers](life/laziness-does-not-exist.md) - Situational factors predict behavior better than character; respond with curiosity, not judgment
- [Learning to say no without guilt](life/learning-to-say-no.md) - Fear of rejection drives reflexive yes; three mental shifts and practical tactics for boundaries
- [The Munger Operating System for life](life/munger-operating-system.md) - Charlie Munger's life principles: deserve what you want, lifetime learning, multidisciplinary thinking
- [Navagraha: nine celestial bodies in Hindu astrology](life/navagraha-nine-celestial-bodies.md) - Nine planets of Hindu cosmology; seven map to weekdays, Rahu/Ketu are shadow nodes governing karma
- [Pavel Durov's secrets for success](life/pavel-durov-secrets-for-success.md) - Telegram founder's principles: master what you love, read constantly, write daily, stay healthy
- [Simple burnout triage](life/simple-burnout-triage.md) - One question: can you sustain the last 2 months forever? Three response levels from crisis to thriving
- [Time is the only real currency we have](life/time-is-the-only-real-currency.md) - Engineers waste time on language tribalism and premature scale; invest in automation and tool mastery instead
- [To chat lanh dao kinh doanh](life/to-chat-lanh-dao-kinh-doanh.md) - How a Vietnamese tech corp screens management trainees; energy and persuasion are the two key tests
- [Vipassana for hackers](life/vipassana-for-hackers.md) - 10-day silent meditation course explained for rational minds; systematic self-observation as mind-hacking
- [We used to just live](life/we-used-to-just-live.md) - Technology colonized every gap where unguided thinking happened; boredom was doing cognitive work we didn't realize
- [What it feels like to become poor](life/what-it-feels-like-to-become-poor.md) - Lost $3M in 2008 crash, ended up at a car wash; humility came from the place he feared most
- [When and how to ask for help](life/when-and-how-to-ask-for-help.md) - Timebox your struggle, then ask; balance between pestering and spinning your wheels
- [Why explore space - Stuhlinger's letter](life/why-explore-space-stuhlinger-letter.md) - NASA scientist's 1970 defense of space spending: the microscope parable and satellites fighting hunger
- [Why we lie about being retired](life/why-we-lie-about-being-retired.md) - Retirement is an identity crisis, not just a financial event; work provides meaning most can't replace
- [Working attitude principles](life/working-attitude-principles.md) - Seven work ethic principles and ten anti-patterns; no industry is easy money, always harvest something

## mcp

- [MCP tool schema caching in Claude.ai connectors](mcp/mcp-tool-schema-caching-in-claude-ai-connectors.md) - Claude.ai caches MCP schemas per session; disconnect+reconnect to force refresh
- [Security gates for MCP tools that bridge private to public](mcp/security-gates-for-mcp-tools-that-bridge-private-to-public.md) - Server-side security gates: context-anchored secret scan, path traversal, cost-ordered pipeline

## patterns

- [Redundant API pre-checks in wrapper functions](patterns/redundant-api-pre-checks-in-wrapper-functions.md) - Wrapper checks file existence, then library re-checks internally; doubled API calls

## pkm

- [LLM Wiki pattern: compilation over retrieval](pkm/llm-wiki-pattern-compilation-over-retrieval.md) - LLM compiles raw sources into interlinked wiki instead of re-deriving via RAG each time
- [Why knowledge notes need context, not just facts](pkm/why-knowledge-notes-need-context-not-just-facts.md) - Default capture depth was TIL (shallow); changing default to Atomic Note fixed quality

## wealth

- [Anti-patterns that destroy trust permanently](wealth/anti-patterns-that-destroy-trust-permanently.md) - Catalog of trust-killing behaviors; gossip and info leaks are unrecoverable
- [Enterprise trust ladder: vendor to strategic partner](wealth/enterprise-trust-ladder-vendor-to-strategic-partner.md) - Five rungs from cold contact to strategic partner; pilot delivery is 60% process
- [How to sit at the table: the thesis](wealth/how-to-sit-at-the-table-the-thesis.md) - Seek opportunity access and judgment transfer from elders, not money
- [The 12-month progression: deposit to partnership](wealth/the-12-month-progression-deposit-to-partnership.md) - Trust builds in 4 phases over 12 months; most quit by month 3
- [The three gates: what elders screen for](wealth/the-three-gates-what-elders-screen-for.md) - Invisible character, value, and role tests that gatekeep inner circles

## youtube

- [YouTube transcript extraction from cloud containers](youtube/youtube-transcript-extraction-from-cloud-containers.md) - Node.js fetch ignores HTTPS_PROXY; fix with undici ProxyAgent for YouTube transcripts
