# Index

> Full catalog of all notes, grouped by folder. Last updated: 2026-04-19

## ai

- [Claude dispatch workflows and async AI orchestration from mobile](ai/claude-dispatch-workflows-and-async-ai-orchestration-from-mobile.md) - Orchestrate 60+ parallel AI sessions from your phone; knowledge layer compounds across surfaces
- [Complete guide to Claude Code features workflows and ecosystem](ai/complete-guide-to-claude-code-features-workflows-and-ecosystem.md) - Practitioner's guide: agentic loop, CLAUDE.md under 200 lines, Sonnet for 90% of tasks
- [Grand unified theory of the AI hype cycle](ai/grand-unified-theory-of-ai-hype-cycle.md) - Seven decades of AI hype follow the same arc: novel mechanism gets labeled AI, boom, bust, rename
- [How LLM agents do web research: the ReAct loop](ai/how-llm-agents-do-web-research-the-react-loop.md) - Agent research is a ReAct loop, not an algorithm; biggest failure is over-weighting official sources
- [LLM agent memory synthesis April 2026](ai/llm-agent-memory-synthesis-april-2026.md) - Synthesis: 5-stage pipeline + 3 battlegrounds + harness hooks form one stack with a broken evaluation floor
- [LLM agent memory systems landscape 2026](ai/llm-agent-memory-systems-landscape-2026.md) - Memory systems all solve a 5-stage pipeline; differentiation is in structure around LLM decisions
- [LLM memory benchmarks and evaluation crisis](ai/llm-memory-benchmarks-and-evaluation-crisis.md) - LoCoMo has ~99 wrong answers; no trustworthy single benchmark exists for memory systems
- [LLM memory systems three competitive battlegrounds](ai/llm-memory-systems-three-competitive-battlegrounds.md) - Write/update loop absorbs 80% of innovation; all systems delegate conflict resolution to LLMs
- [Memory systems as agent harness plugins](ai/memory-systems-as-agent-harness-plugins.md) - Memory integrates via two lifecycle hooks: before-turn recall, after-turn capture
- [Multi-agent coding brain rot scan design](ai/multi-agent-coding-brain-rot-scan-design-externalized-state-clean-handoffs.md) - Fighter pilot scan patterns fix the brain rot of running 5+ AI agents in parallel
- [Transformer internals for software engineers, FFN as graph database (LARQL)](ai/transformer-internals-for-software-engineers-ffn-as-graph-database-larql.md) - FFN as sparse KNN lookup over ~348K "edges"; graph-DB reframe makes factual knowledge editable without retraining
- [TurboQuant KV cache compression](ai/turboquant-kv-cache-compression.md) - Random rotation + two-stage quantizer cuts KV cache to 3-4 bits with unbiased inner products; ICLR 2026

## ai-tooling

- [AI dev stack 8-layer model with tool evaluations](ai-tooling/ai-dev-stack-8-layer-model-with-tool-evaluations-march-2026.md) - 8-layer stack model with SDD framework comparisons, tool scores, and 6-phase workflow
- [AI tooling stack synthesis April 2026](ai-tooling/ai-tooling-stack-synthesis-april-2026.md) - Synthesis: 3 layers wired through one rubric; growth and adoption-readiness are inversely correlated
- [AutoResearch: the Karpathy loop pattern](ai-tooling/autoresearch-the-karpathy-loop-pattern.md) - Three-file contract (goal + artifact + frozen eval) ratchets quality via automated experiments
- [ClaudeKit deep dive: session recovery, red team and gaps](ai-tooling/claudekit-deep-dive-session-recovery-red-team-and-gaps.md) - ClaudeKit saves state on Stop hook, not just PreCompact; ship pipeline more complete than ours
- [ClaudeKit evaluation and unique features](ai-tooling/claudekit-evaluation-and-unique-features.md) - 50+ commands, interview-style spec gate, 4 adversarial reviewers; scored BOOKMARK (10/15)
- [Code graph context tools for token reduction](ai-tooling/code-graph-context-tools-for-token-reduction.md) - AST-based code graphs cut agent token usage 40-95% by replacing grep with structured queries
- [Context Hub vs Context7 vs the context layer ecosystem](ai-tooling/context-hub-vs-context7-vs-the-context-layer-ecosystem.md) - Context Hub = curated API docs with feedback loop; Context7 = 9k+ library docs via MCP
- [deepagents vs OpenClaw vs Hermes: category positioning](ai-tooling/deepagents-vs-openclaw-vs-hermes-category-positioning.md) - Library vs runtime distinction; deepagents stacks under runtimes, doesn't compete with them
- [Hermes Agent comprehensive briefing April 2026](ai-tooling/hermes-agent-comprehensive-briefing-april-2026.md) - Nous Research's self-hosted agent with auto-generated skills; 0 to 95.6K stars in seven weeks
- [Hermes vs OpenClaw competitive scene April 2026](ai-tooling/hermes-vs-openclaw-competitive-scene-april-2026.md) - OpenClaw wins on metrics, Hermes wins the narrative; realistic equilibrium is to run both
- [OpenClaw multi-persona dev team setup playbook](ai-tooling/openclaw-multi-persona-dev-team-setup-playbook.md) - End-to-end JSON5 config + SOUL/AGENTS/TOOLS files for a Telegram-led PM/Engineer/QA team
- [OpenClaw virtual company pattern](ai-tooling/openclaw-virtual-company-pattern.md) - "CEO/CTO/PM" multi-agent idiom is a convention, not a feature; six failure modes most writeups skip
- [Prompt improvement as a learning technique](ai-tooling/prompt-improvement-as-a-learning-technique.md) - Sharpening vague prompts into structured ones is a thinking tool, not just better answers
- [Tool evaluation 5-question rubric](ai-tooling/tool-evaluation-5-question-rubric.md) - 5 questions in 10 min; the kill question: what past failure would this have prevented?
- [Why developers migrate to Hermes, ranked real vs hype](ai-tooling/why-developers-migrate-to-hermes-ranked-real-vs-hype.md) - Push factor (OpenClaw CVEs + subscription cliff) beats pull factor; steal the auto-skill pattern

## claude-code

- [Claude Code ecosystem repo evaluations for kit building](claude-code/claude-code-ecosystem-repo-evaluations-for-kit-building.md) - Evaluated 7 repos; methodology + hardening tools don't overlap, integration gap is the opportunity
- [Claude Code hook lifecycle and event system](claude-code/claude-code-hook-lifecycle-and-event-system.md) - 21 hook events with exit code 2 as the only real enforcement; hooks beat CLAUDE.md rules
- [Claude Code hook schema decision values per event type](claude-code/claude-code-hook-schema-decision-values-per-event-type.md) - Stop hooks need "approve"/"block", not "allow"/"deny"; mixing causes silent validation errors
- [Commands vs hooks vs skills decision framework](claude-code/commands-vs-hooks-vs-skills-decision-framework.md) - If skipping it causes irreversible damage, use a hook; if output degrades, use a skill
- [Compaction defense patterns for Claude Code sessions](claude-code/compaction-defense-patterns-for-claude-code-sessions.md) - Two-layer defense: PreCompact backup + post-compaction re-injection of critical rules

## crypto

- [Asynchronous Byzantine Fault Tolerance](crypto/asynchronous-byzantine-fault-tolerance.md) - aBFT removes timing assumptions; strongest fault tolerance model for permissionless blockchains
- [The Bitcoin investment paradox](crypto/bitcoin-investment-paradox.md) - Either Bitcoin becomes universal (you're auto-invested) or it fails (you lose nothing)
- [Cobie on (3,3) and crypto incentives](crypto/cobie-on-33-and-crypto-incentives.md) - Time horizon determines VC behavior; mercenary capital erodes trust in bull markets
- [Double spending in cryptocurrency](crypto/double-spending.md) - The fundamental digital currency problem: race attacks, 51% attacks, and confirmation defenses
- [Ethereum token standards and security tokens](crypto/ethereum-token-standards-and-security-tokens.md) - From ERC-20 to security tokens; STOs bring SEC-compliant equity and dividends on-chain
- [Ray Dalio on Bitcoin as digital gold](crypto/ray-dalio-on-bitcoin.md) - Dalio's 2021 assessment: Bitcoin is "one hell of an invention" but risks remain
- [Runtime verification for blockchain security](crypto/runtime-verification-for-blockchain-security.md) - K Framework proves smart contract correctness mathematically; stronger than manual audits
- [Stellar vs Nano comparison](crypto/stellar-vs-nano-comparison.md) - Both fast and cheap; Nano is currency-only, Stellar is a platform with tokens and DEX
- [Stripe on Bitcoin as the IP layer of payments](crypto/stripe-on-bitcoin.md) - Stripe backed then dropped Bitcoin; vision of global payment protocol hit fee and speed walls
- [Token emission models](crypto/token-emission-models.md) - Six models: halving, exponential decay, linear decay, fixed supply, constant, bonding curve
- [Undercollateralized loans in DeFi](crypto/undercollateralized-loans-in-defi.md) - Eight approaches from flash loans to social trust pools; each trades off risk differently

## cs

- [A brief totally accurate history of programming languages](cs/brief-totally-accurate-history-of-programming-languages.md) - Satirical timeline of programming language creation from 1800 punch cards to modern languages
- [Comparing algorithm textbooks: CLRS, Tardos, Skiena, Sedgewick](cs/comparing-algorithm-textbooks.md) - Four major algo textbooks compared by use case: reference, network flow, interviews, coursework
- [Go To statement considered harmful](cs/goto-considered-harmful.md) - Dijkstra's 1968 argument: go to destroys the coordinate system needed to reason about programs
- [History of regular expressions](cs/history-of-regular-expressions.md) - From 1950s neuroscience (Kleene's algebra) through UNIX tooling to the Russian internet outage
- [History of software - resource collection](cs/history-of-software-resources.md) - Curated links covering software history: timelines, shareware, accessibility, word processing
- [The immutability of math for programmers](cs/immutability-of-math-for-programmers.md) - Math underpins CS at its core; frameworks change, linear algebra and probability do not
- [Syntactic sugar, salt, and saccharin](cs/syntactic-sugar-salt-saccharin.md) - Sugar helps readability, salt enforces discipline, saccharin is empty calories in language design
- [A TCP/IP tutorial - RFC 1180](cs/tcp-ip-tutorial-rfc1180.md) - RFC 1180 walkthrough of TCP/IP layers, ARP resolution, and packet flow between networks
- [The actor model in 10 minutes](cs/the-actor-model.md) - Actors replace shared memory with isolated units communicating via async message passing
- [The next century of computing](cs/the-next-century-of-computing.md) - End of Moore's Law triggers a Cambrian Explosion of bizarre hardware beyond 1940s architectures
- [Turing completeness](cs/turing-completeness.md) - A system needs branching, looping, and unbounded memory to compute anything
- [What's next in computing](cs/whats-next-in-computing.md) - Computing eras cycle every 10-15 years; 2016 prediction: hardware + AI is the next wave
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

Reorganized into 6 sub-folders on 2026-04-19 (was 107 notes flat). See `engineering/README.md` for the sub-folder taxonomy.

### engineering/architecture

- [10 tips to improve application performance](engineering/architecture/10-tips-application-performance.md) - NGINX guide: reverse proxy, load balancing, caching, compression, and HTTP/2 for 10x gains
- [Apache ZooKeeper for distributed coordination](engineering/architecture/apache-zookeeper-distributed-coordination.md) - Znodes, watches, sessions for leader election, service discovery, and distributed locks
- [Benefits of continuous delivery](engineering/architecture/benefits-of-continuous-delivery.md) - Small deploys mean lower risk, fresher context, faster feedback, and features reaching users sooner
- [Creating a microservice - answer these 10 questions first](engineering/architecture/creating-a-microservice-ten-questions.md) - Operational checklist: testing, config, security, discovery, scaling, failure handling, upgrades, monitoring
- [CSS architecture - first steps](engineering/architecture/css-architecture-first-steps.md) - BEM, SMACSS, ITCSS methodologies for maintainable stylesheets
- [DevOps team topologies](engineering/architecture/devops-team-topologies.md) - Anti-patterns (silos, fake DevOps teams) vs recommended topologies for Dev-Ops collaboration
- [Hidden dividends of microservices](engineering/architecture/hidden-dividends-of-microservices.md) - Beyond scaling: microservices force explicit interfaces, independent deploys, and team autonomy
- [The history of Hadoop](engineering/architecture/history-of-hadoop.md) - From Doug Cutting's Lucene to Apache Hadoop, driven by two Google papers (GFS and MapReduce)
- [HTTP caching guide](engineering/architecture/http-caching-guide.md) - Cache-Control, ETag, Last-Modified headers explained; caching layers from browser to CDN
- [Testing strategies in a microservice architecture](engineering/architecture/microservice-testing-strategies.md) - Five-layer test pyramid for microservices: unit, integration, component, contract, end-to-end
- [Monorepo advantages](engineering/architecture/monorepo-advantages.md) - Single repo for all code: atomic commits, unified tooling, easier refactoring across boundaries
- [The SaaS CTO security checklist](engineering/architecture/saas-cto-security-checklist.md) - Comprehensive checklist: infrastructure, application, and organizational security for SaaS
- [Martin Fowler's software architecture guide](engineering/architecture/software-architecture-guide-fowler.md) - Architecture as shared understanding; quality drives speed; social boundaries shape systems
- [The SRE model](engineering/architecture/the-sre-model.md) - Google's SRE: voluntary support that scales with product maturity, not a new ops title

### engineering/careers

- [The junior programmer's guide to asking for help](engineering/careers/asking-for-help-at-work.md) - Timebox your struggle, then ask; balance between pestering and spinning your wheels
- [Chin thoi quen xau can bo neu muon theo nganh CNTT](engineering/careers/chin-thoi-quen-xau-nganh-cntt.md) - Nine bad habits to drop for IT careers: not reading docs, skimming, copying without thinking
- [Discipline doesn't scale](engineering/careers/discipline-doesnt-scale.md) - Calls for discipline fail because there is no motivation to adopt them; change the environment instead
- [Egoless Engineering](engineering/careers/egoless-engineering.md) - Ego and parochialism destroy orgs; domain experts who teach beat domain owners who gatekeep
- [Five problem-solving skills for software engineers](engineering/careers/five-problem-solving-skills.md) - Break problems down, stay calm, think before coding, ask for help, practice regularly
- [Heisenberg developers](engineering/careers/heisenberg-developers.md) - Measuring developer work alters their behavior; estimation demands kill creativity and drive talent away
- [How to learn software design](engineering/careers/how-to-learn-software-design.md) - Read other people's code, study patterns, then practice by rewriting existing programs
- [How to read research papers](engineering/careers/how-to-read-research-papers.md) - Three-pass method: skim structure, grasp arguments, then verify details
- [How to succeed as a poor programmer](engineering/careers/how-to-succeed-as-a-poor-programmer.md) - Compensate for weak coding with communication, testing, simplicity, and asking for help
- [Lessons from a senior developer](engineering/careers/lessons-from-a-senior-developer.md) - Hard-won lessons: ego kills growth, code reviews teach more than coding, shipping beats perfection
- [Lessons learned in software development](engineering/careers/lessons-learned-in-software-dev.md) - Collected wisdom on development practices, debugging, team dynamics, and project management
- [Leveraging poor memory in engineering](engineering/careers/leveraging-poor-memory-in-engineering.md) - Poor memory forces good habits: write everything down, automate, document decisions
- [Programmer competency matrix](engineering/careers/programmer-competency-matrix.md) - 0-3 scale across 20+ skill categories for self-assessment; knowledge is cumulative per level
- [The purple developer - 10x productivity is contextual](engineering/careers/purple-developer-10x-myth.md) - The 10x engineer is the one who built the system; spread the knowledge, spread the productivity
- [What makes a senior developer](engineering/careers/so-you-want-to-be-senior.md) - Seniority is judgment, positive impact beyond code, and being a force multiplier for the team
- [The ACM/IEEE Software Engineering code of ethics](engineering/careers/software-engineering-code-of-ethics.md) - Eight principles covering public interest, client duties, product quality, and professional judgment
- [Software engineering vs computer science](engineering/careers/software-engineering-vs-computer-science.md) - CS focuses on algorithms; SE focuses on process management for complex software systems
- [12 years, 12 lessons at ThoughtWorks](engineering/careers/twelve-lessons-at-thoughtworks.md) - Patrick Kua on tools vs thinking, agile transformations, safety for learning, and coding architects
- [Working as a software developer](engineering/careers/working-as-a-software-developer.md) - Production software realities: programs are big, never done, and reading matters more than writing

### engineering/code-quality

- [Best practices for agile documentation](engineering/code-quality/agile-documentation-best-practices.md) - Prefer executable specs, document stable concepts, keep it simple; fewer docs done well
- [The antipattern scripting language](engineering/code-quality/antipattern-scripting-language.md) - Antipatterns are contextual; in throwaway scripts, they become good ideas that speed completion
- [Code for readability](engineering/code-quality/code-for-readability.md) - Code as if the maintainer is a violent psychopath who knows where you live
- [Code review basics](engineering/code-quality/code-review-basics.md) - Fundamentals of starting code review as a team practice; overcoming resistance and building habit
- [What to know before debating type systems](engineering/code-quality/debating-type-systems.md) - Static vs dynamic, strong vs weak, nominal vs structural; most type debates use terms imprecisely
- [Deleting code](engineering/code-quality/deleting-code.md) - Delete unused code permanently; version control is your safety net, not commented-out blocks
- [Effective code reviews without wasting time](engineering/code-quality/effective-code-reviews.md) - One reviewer finds half of defects; beyond two reviewers, social loafing kicks in
- [Mastering programming](engineering/code-quality/mastering-programming.md) - Kent Beck's practices: slicing problems, one thing at a time, concrete then abstract
- [No primitives - model domain concepts with types](engineering/code-quality/no-primitives-domain-modeling.md) - Primitive Obsession code smell; wrap domain concepts in types to enforce invariants at construction
- [Programming practices - Unix philosophy and beyond](engineering/code-quality/programming-practices-principles.md) - Timeless principles: prototype first, fail noisily, separate policy from mechanism, least surprise
- [10 modern software over-engineering mistakes](engineering/code-quality/software-over-engineering-mistakes.md) - Anticipating futures, premature abstraction, shallow wrappers, and metrics over correctness
- [What makes code Swifty](engineering/code-quality/swifty-code.md) - Three pillars: strong type safety, path to performance, clear expressive naming
- [Type wars - static vs dynamic typing history](engineering/code-quality/type-wars.md) - Uncle Bob traces six decades of the type debate from Frege through Fortran to modern languages
- [Write code that is easy to delete, not easy to extend](engineering/code-quality/write-code-easy-to-delete.md) - Lines of code are lines spent; build disposable software, not reusable software
- [How to write a successful conference proposal](engineering/code-quality/writing-conference-proposals.md) - Proposals target reviewers, not audiences; the talk and the proposal are different skills
- [Writing good commit messages](engineering/code-quality/writing-good-commit-messages.md) - 50-char summary, 72-char body wrapping; explain why, not what
- [Writing great documentation for open source](engineering/code-quality/writing-great-documentation.md) - Start with empathy, linearize non-linear concepts, write a TOC of reader questions first
- [Writing perfect pull requests](engineering/code-quality/writing-perfect-pull-requests.md) - Provide context, be explicit about feedback needs, ask questions rather than issue commands

### engineering/functional

- [Building web apps with functional programming](engineering/functional/building-web-apps-with-functional-programming.md) - Full FP stack: Elm frontend, Haskell backend, NixOS infra for reproducible builds
- [Elm language overview](engineering/functional/elm-language-overview.md) - Functional language compiling to JS; Model-View-Update pattern influenced Redux
- [Functional programming for the rest of us](engineering/functional/fp-for-the-rest-of-us.md) - FP appears difficult due to presentation, not complexity; immutability enables easy testing and debugging
- [Functional thinking](engineering/functional/functional-thinking.md) - Neal Ford on shifting from OO to FP mindset: composition over inheritance, Either over exceptions
- [Functional vs imperative vs declarative programming](engineering/functional/functional-vs-imperative-vs-declarative.md) - Reference card for three paradigms: imperative (how), declarative (what), functional (pure transforms)
- [Good and bad Elixir](engineering/functional/good-and-bad-elixir.md) - Anti-patterns: piping side effects, over-using with, hiding higher-order functions
- [Goodbye, Object Oriented Programming](engineering/functional/goodbye-object-oriented-programming.md) - OOP's three pillars dismantled: banana-gorilla-jungle, diamond problem, fragile base class
- [Pragmatic functional programming](engineering/functional/pragmatic-functional-programming.md) - Uncle Bob: FP matters beyond concurrency; immutability brings simplicity even on 4-core laptops
- [Railway Oriented Programming](engineering/functional/railway-oriented-programming.md) - Two-track error handling: happy path and failure path via Either/Result types, no monad jargon
- [What is functional programming](engineering/functional/what-is-functional-programming.md) - FP defined by side effects management: pure functions with declared inputs and outputs only
- [Which programming languages are functional](engineering/functional/which-programming-languages-are-functional.md) - Side-effects management as the criterion; Haskell is genuine, JS is not, Clojure is 80%
- [Why OO sucks - Joe Armstrong's critique](engineering/functional/why-oo-sucks.md) - Erlang creator's four objections: binding data to functions, everything-is-object, scattered types, private state

### engineering/go

- [Between Go and Elixir](engineering/go/between-golang-and-elixir.md) - Complementary model: Elixir for orchestration and fault tolerance, Go for compute-heavy tasks
- [Building a worker pool in Go](engineering/go/building-worker-pool-in-go.md) - Job queue, workers, and dispatcher pattern for bounded concurrency in Go
- [Channels in Golang](engineering/go/channels-in-golang.md) - Channel types, buffering, nil/closed behavior, and edge cases for correct concurrent Go
- [Comparing Elixir and Go](engineering/go/comparing-elixir-and-go.md) - Concurrency model comparison: preemptive actors with isolated heaps vs cooperative goroutines with channels
- [Effective error handling in Go](engineering/go/effective-error-handling-in-go.md) - Indented flow pattern, custom error types, and idiomatic error handling practices
- [Elixir concepts for Go developers](engineering/go/elixir-concepts-for-go-developers.md) - Actor model vs CSP comparison: addressable processes with mailboxes vs anonymous goroutines with channels
- [Error handling in Upspin](engineering/go/error-handling-in-upspin.md) - Rob Pike's structured error type with Path, User, Op, Kind fields for rich context across boundaries
- [Four days of Go](engineering/go/four-days-of-go.md) - C/Erlang developer's candid evaluation: fast compilation but syntax inconsistencies and missing features
- [Go best practices for production environments](engineering/go/go-best-practices-for-production.md) - SoundCloud's Go in production: single GOPATH, flat repo structure, go fmt on save
- [Go best practices, six years in](engineering/go/go-best-practices-six-years-in.md) - Peter Bourgon's core principle: make dependencies explicit across config, testing, and design
- [Go concurrency through illustrations](engineering/go/go-concurrency-through-illustrations.md) - Visual introduction to goroutines, channels, and select using mining analogies
- [Go context should go away](engineering/go/go-context-should-go-away.md) - Michal Strba argues Go's context.Context is a poor design that pollutes every function signature
- [Go performance optimization guide](engineering/go/go-performance-optimization-guide.md) - Lock-free ring buffers 3x faster than channels; sync.Pool, escape analysis, and profiling tips
- [Go proverbs](engineering/go/go-proverbs.md) - Rob Pike's Go proverbs: don't communicate by sharing memory, share memory by communicating
- [Go, REST APIs, and pointers](engineering/go/go-rest-apis-and-pointers.md) - Pointer fields solve the zero-value vs intentionally-empty ambiguity in PATCH requests
- [Go testing principles by Dave Cheney](engineering/go/go-testing-principles-dave-cheney.md) - Table-driven tests, test behavior not implementation, use t.Helper() and t.Run()
- [Go - the little language that could](engineering/go/go-the-little-language-that-could.md) - Simplicity and pragmatism drove Go's rise past Swift, Scala, and Rust in language rankings
- [A closer look at Go's type system](engineering/go/go-type-system-closer-look.md) - Named vs unnamed types, underlying types, and assignability rules that trip up Go developers
- [Go vs Swift comparison](engineering/go/go-vs-swift-comparison.md) - Side-by-side comparison: static typing, concurrency models, memory management, error handling
- [Go 2 error handling draft design](engineering/go/go2-error-handling-draft-design.md) - Proposed check/handle keywords to reduce if-err-nil boilerplate; ultimately not accepted
- [Idiomatic Go](engineering/go/idiomatic-go.md) - Naming conventions, spelling, formatting, and style nuances from Go's standard library
- [A million WebSockets and Go](engineering/go/million-websockets-and-go.md) - Mail.Ru optimized 3M concurrent WebSockets from 72 GB to manageable with epoll and buffer pooling
- [The generic dilemma in Go](engineering/go/the-generic-dilemma-in-go.md) - Three approaches to generics: leave out (C), compile-time specialization (C++), boxing (Java)
- [Typed nils in Go](engineering/go/typed-nils-in-go.md) - Interface holds (type, data); nil concrete value in interface is non-nil, breaking nil checks
- [Understanding nil in Go](engineering/go/understanding-nil-in-go.md) - Nil is the zero value for 6 types; each behaves differently when nil, enabling idiomatic patterns
- [Why Go is a poorly designed language](engineering/go/why-go-is-poorly-designed.md) - Seven design flaws: nil interface paradox, variable shadowing, slice pain, compiler rigidity
- [The Zen of Go](engineering/go/zen-of-go.md) - Dave Cheney's Go principles: single-purpose packages, flat control flow, goroutine discipline

### engineering/principles

- [Bit Twiddling Hacks](engineering/principles/bit-twiddling-hacks.md) - Stanford reference of bitwise tricks: branchless abs, popcount, De Bruijn log, Morton interleaving
- [Choose Boring Technology](engineering/principles/choose-boring-technology.md) - Finite innovation tokens; spend them on business problems, not infrastructure novelty
- [Conway's law](engineering/principles/conways-law.md) - System architecture mirrors org communication structure; you cannot fight it, only reshape it
- [Data drives code structure](engineering/principles/data-drives-code-structure.md) - Software structure follows data structure: arrays become loops, graphs become traversals
- [Intro to compilers](engineering/principles/intro-to-compilers.md) - Compiler pipeline: lexing, parsing, AST, optimization, code generation in plain language
- [Papers I like (part 1)](engineering/principles/papers-i-like-part-1.md) - Fabian Giesen's 10 essential CS papers: Lamport, Herlihy, Cook, and more
- [Rob Pike's 5 rules of programming](engineering/principles/rob-pike-five-rules-of-programming.md) - Measure before optimizing; fancy algorithms are slow when n is small; data dominates
- [Rust is not a good C replacement](engineering/principles/rust-is-not-a-good-c-replacement.md) - Drew DeVault: Rust replaces C++, not C; 15 new features/year vs C's 0.73
- [Stack Overflow technical deconstruction](engineering/principles/stack-overflow-technical-deconstruction.md) - Nick Craver's inside look at Stack Overflow infrastructure: radical transparency and embracing failure
- [Pattern matching with case let in Swift](engineering/principles/swift-pattern-matching-case-let.md) - Swift's case let for destructuring enums, optionals, and tuples with pattern matching
- [Technical debt as a city metaphor](engineering/principles/technical-debt-as-a-city.md) - Codebase as city: rushed construction, changing requirements, and patch culture cause decay
- [The 80x24 rule for code formatting](engineering/principles/the-80x24-rule.md) - 80 chars wide, 24 lines tall per method; constraints nudge toward better design
- [UML as a communication tool](engineering/principles/uml-as-communication-tool.md) - UML diagrams for requirements and design communication; modeling language, not a process
- [What if GitHub is the devil - curl's pragmatic take](engineering/principles/what-if-github-is-the-devil.md) - Daniel Stenberg on why curl stays on GitHub: network effect, contingency plans, pragmatism over purity
- [Why big tech companies are so slow](engineering/principles/why-big-tech-is-slow.md) - Feature interaction complexity grows combinatorially; slowness is math, not incompetence
- [Wisdom of programming quotes](engineering/principles/wisdom-of-programming-quotes.md) - Henrik Warne's curated quotes on complexity, debugging, teams, and the nature of programming
- [The Zen of Python](engineering/principles/zen-of-python.md) - PEP 20: beautiful over ugly, explicit over implicit, simple over complex, readability counts

## finance

- [Financial knowledge as compound information advantage](finance/financial-knowledge-as-compound-information-advantage.md) - Bille Finance narrative: information compounds like capital; the gap between Tier 1 and Tier 2 is learnable
- [How the bond market controls housing, stocks, and jobs](finance/how-the-bond-market-controls-housing-stocks-and-jobs.md) - Yield seesaw sets mortgage rates, equity risk premium, and corporate refinancing costs; one chain from the 10-year

## finance-tooling

- [FinceptTerminal evaluation](finance-tooling/fincept-terminal-evaluation.md) - AGPL-3 Qt6 Bloomberg-alternative; 10/15 BOOKMARK; AGPL §13 blocks integration into any network-facing trading stack
- [OpenBB evaluation](finance-tooling/openbb-evaluation.md) - Python-first financial SDK (equities, options, macro, FRED); 11/15 BOOKMARK; crypto value thin, TradFi inflection point
- [OSS trading stack survey, April 2026](finance-tooling/oss-trading-stack-survey-april-2026.md) - 3-category synthesis (execution frameworks / agentic AI / infra libs); Freqtrade + VectorBT canonical for semi-pro crypto; ai-hedge-fund as first vendor pilot
- [Why rotating ISP IPs break Binance API keys, and how to fix it with WireGuard](finance-tooling/wireguard-static-ip-exchange-whitelist.md) - Cheap VPS + WireGuard beats every bundled static-IP product for exchange whitelisting; $5/mo beats $15-500/mo alternatives
- [Static outbound IP solutions for crypto trading bots, ten options compared](finance-tooling/static-ip-solutions-compared-for-trading-bots.md) - 10 options across 5 categories; every commercial static-IP service is Category A (rent a VPS) re-bundled with markup

## geopolitics

- [Australia's Washminster government structure](geopolitics/australias-washminster-government-structure.md) - Australia blends Westminster parliament with US federalism; federal/state split complicates crisis response
- [How the 2026 Strait of Hormuz crisis impacts Australia](geopolitics/how-the-2026-strait-of-hormuz-crisis-impacts-australia.md) - Hormuz closure hits Australia via Asian refineries; only 29-39 days fuel reserves
- [Measuring oil supply disruption severity](geopolitics/measuring-oil-supply-disruption-severity-2026-hormuz-vs-historical-crises.md) - 2026 Hormuz removed 12 mb/d, largest oil disruption ever; 400 mb strategic reserve release

## health

- [Alkaline water health claims vs reality](health/alkaline-water-health-claims-vs-reality.md) - Stomach acid neutralizes alkaline water instantly; premium price buys marketing
- [Vitamins and longevity stack](health/vitamins-and-longevity-stack.md) - Daily supplement stack for anti-aging: NMN, Omega-3, Magnesium, CoQ10, and 12 more with dosages

## hiring

- [40 best questions to ask in an interview](hiring/40-best-questions-to-ask-in-an-interview.md) - High-signal questions candidates should ask employers, organized by category
- [Assessing software engineering candidates](hiring/assessing-software-engineering-candidates.md) - Bryan Cantrill's framework: written artifacts over pop quizzes; evaluate aptitude, motivation, values
- [Company culture is who you hire, fire, and promote](hiring/company-culture-is-who-you-hire-fire-promote.md) - Culture defined by three actions, not mission statements
- [Developer Happiness Index 2021](hiring/developer-happiness-index.md) - What makes developers happy at work; data on retention factors across regions
- [A developer's guide to interviewing](hiring/developers-guide-to-interviewing.md) - Questions to evaluate employers as a developer candidate
- [Don't hire 0x engineers](hiring/dont-hire-0x-engineers.md) - Against the 10x myth; build functional teams with capable people
- [Facebook hiring: strengths, builders, and learners](hiring/facebook-hiring-strengths-builders-learners.md) - Three factors Facebook evaluates in every candidate at every level
- [How to hire](hiring/how-to-hire.md) - Six principles: strengths over weakness, trajectory over experience, doers over talkers
- [How to hire programmers and outsourced developers](hiring/how-to-hire-programmers.md) - Donn Felker's 4-step hiring process with programming challenges as the great equalizer

## history

- [China as a civilization state, not a nation state](history/china-as-a-civilization-state-not-a-nation-state.md) - China is a 2,000-year civilization wearing nation-state clothing; continuity and order trump liberty
- [Imperial examinations: how China replaced religion with meritocracy](history/imperial-examinations-how-china-replaced-religion-with-meritocracy.md) - 1,300-year exam system created cultural unity and loyal bureaucracy without a holy book
- [Israel, Palestine va Jerusalem](history/israel-palestine-va-jerusalem.md) - History of the Israel-Palestine conflict from shared Abrahamic origins to modern territorial disputes
- [Predictive history and the ambition of psycho-history](history/predictive-history-and-the-ambition-of-psycho-history.md) - Prof Jiang aims to build Asimov's psycho-history: connect past, explain present, predict future
- [Sinicization: how China absorbs its conquerors](history/sinicization-how-china-absorbs-its-conquerors.md) - Every conqueror adopted Chinese civilization because demographics and bureaucracy made it inevitable
- [The Tainter trap: why complexity kills empires](history/the-tainter-trap-why-complexity-kills-empires-and-chinas-reset-mechanism.md) - Empires die from complexity overload; China survives by expecting collapse and having a reset protocol

## investing

- [Compound interest levels and lifestyle progression](investing/compound-interest-levels-and-lifestyle-progression.md) - Six wealth levels from $200/mo saver to $200M foundation; instruments change at each threshold
- [How and why I invest in startups](investing/how-and-why-i-invest-in-startups.md) - Fund the best people on the hardest problems; measure both LP returns and happiness

## leadership

- [A decade of remote work](leadership/a-decade-of-remote-work.md) - Ten years of remote lessons: go all-in or not at all, summits matter, hire for self-discipline
- [The consulting secret - ask your senior ICs what is broken](leadership/consulting-secret-ask-the-ics.md) - Schedule 90 min with your best IC, ask what is broken, read it back to leadership
- [CTO vs VP Engineering](leadership/cto-vs-vp-engineering.md) - CTO is outward-facing (vision, customers, innovation); VP Eng is inward-facing (delivery, process, people)
- [How to work out what to charge clients](leadership/how-to-charge-clients.md) - Calculate your floor rate from costs and billability, then apply three markup factors
- [HR evaluation as unique value measurement](leadership/hr-evaluation-unique-value.md) - Market value = Differentiation x Influence; uniqueness beats commoditized skill checklists
- [In pursuit of excellence](leadership/in-pursuit-of-excellence.md) - Excellence comes from unique positioning at domain intersections, not being best at common skills
- [Lam an kieu Cu Ho](leadership/lam-an-kieu-cu-ho.md) - Five business lessons from Ho Chi Minh's methods, applied at FPT Software by Nguyen Thanh Nam
- [How to manage people who are smarter than you](leadership/managing-people-smarter-than-you.md) - Reframe management as enabling others' success; confront insecurity, learn from your team
- [Masayoshi Son and the SoftBank Vision Fund](leadership/masayoshi-son-softbank-vision.md) - $100B fund betting AI runs the planet; gun-senryaku connects portfolio companies in a flock
- [Nguyen tac truc giac trong lanh dao](leadership/nguyen-tac-truc-giac.md) - Shark Phu on "tuong" traits and Maxwell on intuition as the differentiator in leadership
- [Note to new design managers](leadership/note-to-new-design-managers.md) - Practical guide: manage time, communicate definitively, document everything, protect craft quality
- [The rise of the interim CTO](leadership/rise-of-the-interim-cto.md) - Temporary tech executive bridges the gap between coding founder and strategic tech leadership
- [Steve Jobs negotiation and persuasion tactics](leadership/steve-jobs-negotiation-tactics.md) - Pitch with passion, be brutally honest, earn respect through work ethic, disarm with charm
- [Tips on working with talents](leadership/tips-on-working-with-talents.md) - Four angles on using talent: genuinely need them, create worthy challenges, fair treatment, tolerate quirks
- [Why soldiers and chefs make the best product managers](leadership/why-soldiers-and-chefs-make-best-pms.md) - Leading without authority under pressure with imperfect info; OODA loop for shipping decisions
- [Why you need engineering managers](leadership/why-you-need-engineering-managers.md) - Coordination math, calendar incompatibility, and accountability require dedicated management roles

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
- [Tieu chuan cua ban la gi](life/tieu-chuan-cua-ban-la-gi.md) - High standards vs low standards in work; the difference shows in how you treat details and quality
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

- [Pattern - Backends for Frontends (BFF)](patterns/backend-for-frontend-pattern.md) - Dedicated backend per client type; avoids API bloat from serving diverse frontends
- [Redundant API pre-checks in wrapper functions](patterns/redundant-api-pre-checks-in-wrapper-functions.md) - Wrapper checks file existence, then library re-checks internally; doubled API calls

## pkm

- [LLM Wiki pattern: compilation over retrieval](pkm/llm-wiki-pattern-compilation-over-retrieval.md) - LLM compiles raw sources into interlinked wiki instead of re-deriving via RAG each time
- [Why knowledge notes need context, not just facts](pkm/why-knowledge-notes-need-context-not-just-facts.md) - Default capture depth was TIL (shallow); changing default to Atomic Note fixed quality

## startup

- [Anatomy of software frauds](startup/anatomy-of-software-frauds.md) - Three-layer fraud architecture: unlimited scapegoats, sales-driven culture, deceptive founding
- [Tap trung vao san pham](startup/tap-trung-vao-san-pham.md) - When product is broken, sales and marketing accelerate failure; fix the product first
- [Tesla and GM - founders vs professional managers](startup/tesla-gm-founders-vs-managers.md) - Steve Blank parallels Musk/Durant (visionary founders) vs Sloan (professional management)

## wealth

- [Anti-patterns that destroy trust permanently](wealth/anti-patterns-that-destroy-trust-permanently.md) - Catalog of trust-killing behaviors; gossip and info leaks are unrecoverable
- [Enterprise trust ladder: vendor to strategic partner](wealth/enterprise-trust-ladder-vendor-to-strategic-partner.md) - Five rungs from cold contact to strategic partner; pilot delivery is 60% process
- [How to sit at the table: the thesis](wealth/how-to-sit-at-the-table-the-thesis.md) - Seek opportunity access and judgment transfer from elders, not money
- [The 12-month progression: deposit to partnership](wealth/the-12-month-progression-deposit-to-partnership.md) - Trust builds in 4 phases over 12 months; most quit by month 3
- [The three gates: what elders screen for](wealth/the-three-gates-what-elders-screen-for.md) - Invisible character, value, and role tests that gatekeep inner circles

## youtube

- [YouTube transcript extraction from cloud containers](youtube/youtube-transcript-extraction-from-cloud-containers.md) - Node.js fetch ignores HTTPS_PROXY; fix with undici ProxyAgent for YouTube transcripts

## zed

- [Zed global agent rules live in the Rules Library, not AGENTS.md](zed/zed-global-agent-rules-live-in-the-rules-library-not-agents-md.md) - No global file escape hatch; user-scope rules live in an LMDB database and need the paperclip icon to be default
