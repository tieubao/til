# Index

> Full catalog of all notes, grouped by folder. Last updated: 2026-05-27

## ai

- [Claude dispatch workflows and async AI orchestration from mobile](notes/ai/claude-dispatch-workflows-and-async-ai-orchestration-from-mobile.md) - Orchestrate 60+ parallel AI sessions from your phone; knowledge layer compounds across surfaces
- [Complete guide to Claude Code features workflows and ecosystem](notes/ai/complete-guide-to-claude-code-features-workflows-and-ecosystem.md) - Practitioner's guide: agentic loop, CLAUDE.md under 200 lines, Sonnet for 90% of tasks
- [Finding your unknowns: the bottleneck in agentic coding](notes/ai/finding-your-unknowns-agentic-coding.md) - Quality is bottlenecked by how fast the operator clarifies unknowns; a 2x2 with a distinct technique per quadrant (blindspot pass, interview, prototype-and-react)
- [Grand unified theory of the AI hype cycle](notes/ai/grand-unified-theory-of-ai-hype-cycle.md) - Seven decades of AI hype follow the same arc: novel mechanism gets labeled AI, boom, bust, rename
- [How LLM agents do web research: the ReAct loop](notes/ai/how-llm-agents-do-web-research-the-react-loop.md) - Agent research is a ReAct loop, not an algorithm; biggest failure is over-weighting official sources
- [LLM agent memory synthesis April 2026](notes/ai/llm-agent-memory-synthesis-april-2026.md) - Synthesis: 5-stage pipeline + 3 battlegrounds + harness hooks form one stack with a broken evaluation floor
- [LLM agent memory systems landscape 2026](notes/ai/llm-agent-memory-systems-landscape-2026.md) - Memory systems all solve a 5-stage pipeline; differentiation is in structure around LLM decisions
- [LLM memory benchmarks and evaluation crisis](notes/ai/llm-memory-benchmarks-and-evaluation-crisis.md) - LoCoMo has ~99 wrong answers; no trustworthy single benchmark exists for memory systems
- [LLM memory systems three competitive battlegrounds](notes/ai/llm-memory-systems-three-competitive-battlegrounds.md) - Write/update loop absorbs 80% of innovation; all systems delegate conflict resolution to LLMs
- [Memory systems as agent harness plugins](notes/ai/memory-systems-as-agent-harness-plugins.md) - Memory integrates via two lifecycle hooks: before-turn recall, after-turn capture
- [Multi-agent coding brain rot scan design](notes/ai/multi-agent-coding-brain-rot-scan-design-externalized-state-clean-handoffs.md) - Fighter pilot scan patterns fix the brain rot of running 5+ AI agents in parallel
- [Scaling the harness: six components of an agentic system](notes/ai/scaling-the-harness-six-components.md) - P = f(R,M,C,S,O,G); three named failure modes (exposure-without-access, stale-but-confident, confident-but-unchecked) and the process-metrics evaluation agenda
- [Transformer internals for software engineers, FFN as graph database (LARQL)](notes/ai/transformer-internals-for-software-engineers-ffn-as-graph-database-larql.md) - FFN as sparse KNN lookup over ~348K "edges"; graph-DB reframe makes factual knowledge editable without retraining
- [TurboQuant KV cache compression](notes/ai/turboquant-kv-cache-compression.md) - Random rotation + two-stage quantizer cuts KV cache to 3-4 bits with unbiased inner products; ICLR 2026

## ai-tooling

- [AI dev stack 8-layer model with tool evaluations](notes/ai-tooling/ai-dev-stack-8-layer-model-with-tool-evaluations-march-2026.md) - 8-layer stack model with SDD framework comparisons, tool scores, and 6-phase workflow
- [AI tooling stack synthesis April 2026](notes/ai-tooling/ai-tooling-stack-synthesis-april-2026.md) - Synthesis: 3 layers wired through one rubric; growth and adoption-readiness are inversely correlated
- [AutoResearch: the Karpathy loop pattern](notes/ai-tooling/autoresearch-the-karpathy-loop-pattern.md) - Three-file contract (goal + artifact + frozen eval) ratchets quality via automated experiments
- [ClaudeKit deep dive: session recovery, red team and gaps](notes/ai-tooling/claudekit-deep-dive-session-recovery-red-team-and-gaps.md) - ClaudeKit saves state on Stop hook, not just PreCompact; ship pipeline more complete than ours
- [ClaudeKit evaluation and unique features](notes/ai-tooling/claudekit-evaluation-and-unique-features.md) - 50+ commands, interview-style spec gate, 4 adversarial reviewers; scored BOOKMARK (10/15)
- [Code graph context tools for token reduction](notes/ai-tooling/code-graph-context-tools-for-token-reduction.md) - AST-based code graphs cut agent token usage 40-95% by replacing grep with structured queries
- [Context Hub vs Context7 vs the context layer ecosystem](notes/ai-tooling/context-hub-vs-context7-vs-the-context-layer-ecosystem.md) - Context Hub = curated API docs with feedback loop; Context7 = 9k+ library docs via MCP
- [deepagents vs OpenClaw vs Hermes: category positioning](notes/ai-tooling/deepagents-vs-openclaw-vs-hermes-category-positioning.md) - Library vs runtime distinction; deepagents stacks under runtimes, doesn't compete with them
- [Hermes Agent comprehensive briefing April 2026](notes/ai-tooling/hermes-agent-comprehensive-briefing-april-2026.md) - Nous Research's self-hosted agent with auto-generated skills; 0 to 95.6K stars in seven weeks
- [Hermes Agent fixed overhead: 13.9K tokens per API call](notes/ai-tooling/hermes-agent-fixed-overhead-13-9k-tokens-per-api-call.md) - 73% of every Hermes call is fixed overhead (tools + system prompt); cost scales with call count, not session count
- [Hermes Agent v0.13.0 release evaluation: top features ranked for 3-tier ecosystem](notes/ai-tooling/hermes-agent-v0-13-0-release-evaluation-top-features-ranked-for-3-tier-ecosystem.md) - Selective adoption verdict; CLONE Kanban + cron no_agent, ADOPT /goal + session resume, STUDY security wave; skip i18n / Google Chat / video tool hype
- [Hermes vs OpenClaw competitive scene April 2026](notes/ai-tooling/hermes-vs-openclaw-competitive-scene-april-2026.md) - OpenClaw wins on metrics, Hermes wins the narrative; realistic equilibrium is to run both
- [LLM API pricing comparison: DeepSeek direct vs Ollama Cloud vs OpenRouter (April 2026)](notes/ai-tooling/llm-api-pricing-comparison-deepseek-direct-vs-ollama-cloud-vs-openrouter-april-2.md) - Provider matrix per model, cost projections by Hermes usage tier, May 5 promo cliff, decision framework
- [OpenClaw multi-persona dev team setup playbook](notes/ai-tooling/openclaw-multi-persona-dev-team-setup-playbook.md) - End-to-end JSON5 config + SOUL/AGENTS/TOOLS files for a Telegram-led PM/Engineer/QA team
- [OpenClaw virtual company pattern](notes/ai-tooling/openclaw-virtual-company-pattern.md) - "CEO/CTO/PM" multi-agent idiom is a convention, not a feature; six failure modes most writeups skip
- [Prompt improvement as a learning technique](notes/ai-tooling/prompt-improvement-as-a-learning-technique.md) - Sharpening vague prompts into structured ones is a thinking tool, not just better answers
- [Secret resolution for pi agent providers via 1Password op read](notes/ai-tooling/secret-resolution-for-pi-agent-providers-via-1password-op-read.md) - `!op read` and `$ENV_VAR` keep provider API keys out of plaintext `auth.json`/`models.json`; service-account auth makes it headless
- [Tool evaluation 5-question rubric](notes/ai-tooling/tool-evaluation-5-question-rubric.md) - 5 questions in 10 min; the kill question: what past failure would this have prevented?
- [Why developers migrate to Hermes, ranked real vs hype](notes/ai-tooling/why-developers-migrate-to-hermes-ranked-real-vs-hype.md) - Push factor (OpenClaw CVEs + subscription cliff) beats pull factor; steal the auto-skill pattern

## agentkernel

- [agentkernel --no-network, --dir, --secret-file silently no-op on Apple Containers](notes/agentkernel/agentkernel-broken-flags-on-apple-containers.md) - Three documented isolation flags accept input and have zero effect on v0.16.0/v0.18.1 with Apple Containers backend; default isolation still works
- [agentkernel plugin install defaults to CWD, not user-global](notes/agentkernel/agentkernel-plugin-install-defaults-to-cwd-not-user-global.md) - First-time gotcha: `plugin install claude` writes `.claude/` and `.mcp.json` into your repo unless you pass `--global`

## career

- [How to win at office politics (BusinessCringe)](notes/career/how-to-win-at-office-politics-businesscringe.md) - The invisible scoreboard runs on perception, not performance; can't opt out, three offensive tactics to defend against, three defensive plays to run

## claude-code

- [Claude Code ecosystem repo evaluations for kit building](notes/claude-code/claude-code-ecosystem-repo-evaluations-for-kit-building.md) - Evaluated 7 repos; methodology + hardening tools don't overlap, integration gap is the opportunity
- [Claude Code hook lifecycle and event system](notes/claude-code/claude-code-hook-lifecycle-and-event-system.md) - 21 hook events with exit code 2 as the only real enforcement; hooks beat CLAUDE.md rules
- [Claude Code hook schema decision values per event type](notes/claude-code/claude-code-hook-schema-decision-values-per-event-type.md) - Stop hooks need "approve"/"block", not "allow"/"deny"; mixing causes silent validation errors
- [Claude Code surfaces - CLI vs web vs desktop and resource usage](notes/claude-code/claude-code-surfaces-cli-vs-web-vs-desktop-and-resource-usage.md) - Four surfaces, three runtimes; desktop Electron costs ~2x RAM and runs hot, CLI + web is the leaner pattern
- [Commands vs hooks vs skills decision framework](notes/claude-code/commands-vs-hooks-vs-skills-decision-framework.md) - If skipping it causes irreversible damage, use a hook; if output degrades, use a skill
- [Compaction defense patterns for Claude Code sessions](notes/claude-code/compaction-defense-patterns-for-claude-code-sessions.md) - Two-layer defense: PreCompact backup + post-compaction re-injection of critical rules
- [Managing Claude Code's agent view (background sessions)](notes/claude-code/managing-claude-codes-agent-view-background-sessions.md) - TUI lifecycle, 30-day retention, the worktree-delete gotcha; do not micromanage, name jobs and sweep orphaned worktrees

## coding-agents

- [Opt-in beats all-in for coding-agent sandboxing on a developer laptop](notes/coding-agents/opt-in-beats-all-in-for-coding-agent-sandboxing.md) - Wrap-every-call sandboxing kills adoption; per-trigger opt-in survives daily use because the host integrations Claude Code relies on are exactly what doesn't work in a sandbox

## comp-fin

- [Optimization as the bridge to computational finance](notes/comp-fin/optimization-as-the-bridge-to-computational-finance.md) - Comp-fin rests on three pillars (stochastic calculus, numerical methods, optimization); convex opt + DP + stochastic control are the workhorses, not MILP

## crypto

- [Asynchronous Byzantine Fault Tolerance](notes/crypto/asynchronous-byzantine-fault-tolerance.md) - aBFT removes timing assumptions; strongest fault tolerance model for permissionless blockchains
- [The Bitcoin investment paradox](notes/crypto/bitcoin-investment-paradox.md) - Either Bitcoin becomes universal (you're auto-invested) or it fails (you lose nothing)
- [Cobie on (3,3) and crypto incentives](notes/crypto/cobie-on-33-and-crypto-incentives.md) - Time horizon determines VC behavior; mercenary capital erodes trust in bull markets
- [Double spending in cryptocurrency](notes/crypto/double-spending.md) - The fundamental digital currency problem: race attacks, 51% attacks, and confirmation defenses
- [Ethereum token standards and security tokens](notes/crypto/ethereum-token-standards-and-security-tokens.md) - From ERC-20 to security tokens; STOs bring SEC-compliant equity and dividends on-chain
- [Ray Dalio on Bitcoin as digital gold](notes/crypto/ray-dalio-on-bitcoin.md) - Dalio's 2021 assessment: Bitcoin is "one hell of an invention" but risks remain
- [Runtime verification for blockchain security](notes/crypto/runtime-verification-for-blockchain-security.md) - K Framework proves smart contract correctness mathematically; stronger than manual audits
- [Stellar vs Nano comparison](notes/crypto/stellar-vs-nano-comparison.md) - Both fast and cheap; Nano is currency-only, Stellar is a platform with tokens and DEX
- [Stripe on Bitcoin as the IP layer of payments](notes/crypto/stripe-on-bitcoin.md) - Stripe backed then dropped Bitcoin; vision of global payment protocol hit fee and speed walls
- [Token emission models](notes/crypto/token-emission-models.md) - Six models: halving, exponential decay, linear decay, fixed supply, constant, bonding curve
- [Undercollateralized loans in DeFi](notes/crypto/undercollateralized-loans-in-defi.md) - Eight approaches from flash loans to social trust pools; each trades off risk differently

## cs

- [A brief totally accurate history of programming languages](notes/cs/brief-totally-accurate-history-of-programming-languages.md) - Satirical timeline of programming language creation from 1800 punch cards to modern languages
- [Comparing algorithm textbooks: CLRS, Tardos, Skiena, Sedgewick](notes/cs/comparing-algorithm-textbooks.md) - Four major algo textbooks compared by use case: reference, network flow, interviews, coursework
- [Go To statement considered harmful](notes/cs/goto-considered-harmful.md) - Dijkstra's 1968 argument: go to destroys the coordinate system needed to reason about programs
- [History of regular expressions](notes/cs/history-of-regular-expressions.md) - From 1950s neuroscience (Kleene's algebra) through UNIX tooling to the Russian internet outage
- [History of software - resource collection](notes/cs/history-of-software-resources.md) - Curated links covering software history: timelines, shareware, accessibility, word processing
- [The immutability of math for programmers](notes/cs/immutability-of-math-for-programmers.md) - Math underpins CS at its core; frameworks change, linear algebra and probability do not
- [Syntactic sugar, salt, and saccharin](notes/cs/syntactic-sugar-salt-saccharin.md) - Sugar helps readability, salt enforces discipline, saccharin is empty calories in language design
- [A TCP/IP tutorial - RFC 1180](notes/cs/tcp-ip-tutorial-rfc1180.md) - RFC 1180 walkthrough of TCP/IP layers, ARP resolution, and packet flow between networks
- [The actor model in 10 minutes](notes/cs/the-actor-model.md) - Actors replace shared memory with isolated units communicating via async message passing
- [The next century of computing](notes/cs/the-next-century-of-computing.md) - End of Moore's Law triggers a Cambrian Explosion of bizarre hardware beyond 1940s architectures
- [Turing completeness](notes/cs/turing-completeness.md) - A system needs branching, looping, and unbounded memory to compute anything
- [What's next in computing](notes/cs/whats-next-in-computing.md) - Computing eras cycle every 10-15 years; 2016 prediction: hardware + AI is the next wave
- [Why interviewers ask linked list questions](notes/cs/why-linked-list-interview-questions.md) - Linked list interviews are a cultural artifact of 1980s C pointer manipulation, not a timeless test
- [Why Vim uses hjkl for navigation](notes/cs/why-vim-uses-hjkl.md) - Chain of accidents from 1967 ASCII table to ADM-3A terminal to Bill Joy's vi

## decentralized

- [Radicle network: peer-to-peer git collaboration](notes/decentralized/radicle-network-peer-to-peer-git-collaboration-explained.md) - Cryptographic-quorum canonical branch (no merge button on a server); CRDT-based Collaborative Objects store issues and patches in plain Git

## devtools

- [age, a modern file-encryption CLI](notes/devtools/age-modern-file-encryption-cli.md) - Small opinionated replacement for GPG-for-files; X25519 + ChaCha20-Poly1305, native SSH-key identities, the default backend for SOPS
- [chezmoi source vs target two-layer mental model](notes/devtools/chezmoi-source-vs-target-two-layer-mental-model.md) - Source is the spec (`~/.local/share/chezmoi`), target is the build artifact (`~`); four verbs (add, re-add, apply, diff) traverse the gap; portability is a separate git layer
- [Starship prompt configuration best practices](notes/devtools/starship-prompt-configuration-best-practices.md) - Start from a preset, use $fill for right-alignment, disable 90% of modules
- [XDG base directory specification](notes/devtools/xdg-base-directory-specification.md) - XDG separates config/data/state/cache into standard dirs; simplifies dotfile management

## diaspora

- [**Vietnamese diaspora synthesis**](notes/diaspora/vietnamese-diaspora-synthesis.md) - Synthesis: structural gap (institutions, not culture) drives hollowing; bridge-builder model is the fix
- [Four Asian diasporas in 2055 projected trajectories](notes/diaspora/four-asian-diasporas-in-2055-projected-trajectories.md) - Four variables predict diaspora survival by 2055; Vietnamese at crossroads, Chinese endure
- [The bridge-builder model](notes/diaspora/the-bridge-builder-model-highest-value-position-for-the-next-vietnamese-generati.md) - Bicultural fluency connecting Vietnam's rising economy to global capital beats enclave and integration
- [Vietnamese vs Chinese diaspora: structural analysis](notes/diaspora/vietnamese-vs-chinese-diaspora-a-structural-analysis-of-divergent-outcomes.md) - Five structural factors explain the gap; escape-through-education trap; bridge-builder alternative
- [Why Little Saigons hollow out](notes/diaspora/why-little-saigons-hollow-out-the-success-driven-exit-problem.md) - Successful kids leave enclaves; renters not owners means no community anchor survives the exit
- [Why Vietnamese built nail salons instead of trade empires](notes/diaspora/why-vietnamese-built-nail-salons-instead-of-trade-empires-the-subsistence-busine.md) - Tippi Hedren's manicurist created a refugee entry point; subsistence businesses rational to escape

## dwarves-kit

- [Building Dwarves Kit from extracted patterns](notes/dwarves-kit/building-dwarves-kit-from-extracted-patterns.md) - Cherry-picked battle-tested patterns from 6+ repos; synthesize, don't originate
- [Dwarves Kit design philosophy and architecture](notes/dwarves-kit/dwarves-kit-design-philosophy-and-architecture.md) - 7 principles: guardrails over guidance, bash over binaries, shallow+wide
- [Dwarves Kit self-assessment against philosophy](notes/dwarves-kit/dwarves-kit-self-assessment-against-philosophy.md) - Ran /kit-health on itself; all 7 principles upheld, file count within budget
- [Dwarves Kit v1.2 agent roster and CDP](notes/dwarves-kit/dwarves-kit-v1-2-agent-roster-and-cdp.md) - 8 specialized agents + collaborative design protocol with 3 decision modes
- [Dwarves Kit v1.2 ClaudeKit patterns adopted](notes/dwarves-kit/dwarves-kit-v1-2-claudekit-patterns-adopted.md) - Adopted session-state save hook and ship pipeline from ClaudeKit
- [Dwarves Kit v1.2 five open decisions](notes/dwarves-kit/dwarves-kit-v1-2-five-open-decisions.md) - Resolved: stay native Task tool, adopt codebase-memory-mcp, GSD v1 architecture
- [Dwarves Kit v1.2 verification pipeline architecture](notes/dwarves-kit/dwarves-kit-v1-2-verification-pipeline-architecture.md) - Verify-fix-retry loop: read-only verifier, scoped fix-agent, max 2 retries
- [SDD landscape and Dwarves Kit v1.2 reference map](notes/dwarves-kit/sdd-landscape-and-dwarves-kit-v1-2-reference-map.md) - Single source of truth: 8-layer stack, all tool scores, full kit inventory
- [SDD multi-agent verification architecture](notes/dwarves-kit/sdd-multi-agent-verification-architecture.md) - 5 gaps found in v1.1; phases 1-2 built, parallel agent teams deferred

## engineering

Reorganized into 6 sub-folders on 2026-04-19 (was 107 notes flat). See `engineering/README.md` for the sub-folder taxonomy.

### engineering/architecture

- [10 tips to improve application performance](notes/engineering/architecture/10-tips-application-performance.md) - NGINX guide: reverse proxy, load balancing, caching, compression, and HTTP/2 for 10x gains
- [age and 1password as complementary encryption tiers](notes/engineering/architecture/age-and-1password-complementary-encryption-tiers.md) - Two-tier backup: 1P for availability, age for offline recovery; key-separation principle (key and ciphertext must have different failure modes)
- [Apache ZooKeeper for distributed coordination](notes/engineering/architecture/apache-zookeeper-distributed-coordination.md) - Znodes, watches, sessions for leader election, service discovery, and distributed locks
- [Benefits of continuous delivery](notes/engineering/architecture/benefits-of-continuous-delivery.md) - Small deploys mean lower risk, fresher context, faster feedback, and features reaching users sooner
- [Cloudflare Workers as a monitoring backend for self-hosted Linux](notes/engineering/architecture/cloudflare-workers-as-monitoring-backend-for-self-hosted-linux.md) - Agent + Worker + D1/KV free tier beats Kuma/Netdata/Prometheus for the 1-20 host solo operator
- [Creating a microservice - answer these 10 questions first](notes/engineering/architecture/creating-a-microservice-ten-questions.md) - Operational checklist: testing, config, security, discovery, scaling, failure handling, upgrades, monitoring
- [CSS architecture - first steps](notes/engineering/architecture/css-architecture-first-steps.md) - BEM, SMACSS, ITCSS methodologies for maintainable stylesheets
- [DevOps team topologies](notes/engineering/architecture/devops-team-topologies.md) - Anti-patterns (silos, fake DevOps teams) vs recommended topologies for Dev-Ops collaboration
- [Hidden dividends of microservices](notes/engineering/architecture/hidden-dividends-of-microservices.md) - Beyond scaling: microservices force explicit interfaces, independent deploys, and team autonomy
- [HIDS-lite rule set for a single-operator Linux VPS](notes/engineering/architecture/hids-lite-rule-set-for-single-operator-vps.md) - 15 rules over cheap shell signals catch 80% of script-kiddie-tier compromises; out-of-band engine required
- [The history of Hadoop](notes/engineering/architecture/history-of-hadoop.md) - From Doug Cutting's Lucene to Apache Hadoop, driven by two Google papers (GFS and MapReduce)
- [HTTP caching guide](notes/engineering/architecture/http-caching-guide.md) - Cache-Control, ETag, Last-Modified headers explained; caching layers from browser to CDN
- [Testing strategies in a microservice architecture](notes/engineering/architecture/microservice-testing-strategies.md) - Five-layer test pyramid for microservices: unit, integration, component, contract, end-to-end
- [Monorepo advantages](notes/engineering/architecture/monorepo-advantages.md) - Single repo for all code: atomic commits, unified tooling, easier refactoring across boundaries
- [The SaaS CTO security checklist](notes/engineering/architecture/saas-cto-security-checklist.md) - Comprehensive checklist: infrastructure, application, and organizational security for SaaS
- [Martin Fowler's software architecture guide](notes/engineering/architecture/software-architecture-guide-fowler.md) - Architecture as shared understanding; quality drives speed; social boundaries shape systems
- [The SRE model](notes/engineering/architecture/the-sre-model.md) - Google's SRE: voluntary support that scales with product maturity, not a new ops title

### engineering/careers

- [The junior programmer's guide to asking for help](notes/engineering/careers/asking-for-help-at-work.md) - Timebox your struggle, then ask; balance between pestering and spinning your wheels
- [Chin thoi quen xau can bo neu muon theo nganh CNTT](notes/engineering/careers/chin-thoi-quen-xau-nganh-cntt.md) - Nine bad habits to drop for IT careers: not reading docs, skimming, copying without thinking
- [Discipline doesn't scale](notes/engineering/careers/discipline-doesnt-scale.md) - Calls for discipline fail because there is no motivation to adopt them; change the environment instead
- [Egoless Engineering](notes/engineering/careers/egoless-engineering.md) - Ego and parochialism destroy orgs; domain experts who teach beat domain owners who gatekeep
- [Five problem-solving skills for software engineers](notes/engineering/careers/five-problem-solving-skills.md) - Break problems down, stay calm, think before coding, ask for help, practice regularly
- [Heisenberg developers](notes/engineering/careers/heisenberg-developers.md) - Measuring developer work alters their behavior; estimation demands kill creativity and drive talent away
- [How to learn software design](notes/engineering/careers/how-to-learn-software-design.md) - Read other people's code, study patterns, then practice by rewriting existing programs
- [How to read research papers](notes/engineering/careers/how-to-read-research-papers.md) - Three-pass method: skim structure, grasp arguments, then verify details
- [How to succeed as a poor programmer](notes/engineering/careers/how-to-succeed-as-a-poor-programmer.md) - Compensate for weak coding with communication, testing, simplicity, and asking for help
- [Lessons from a senior developer](notes/engineering/careers/lessons-from-a-senior-developer.md) - Hard-won lessons: ego kills growth, code reviews teach more than coding, shipping beats perfection
- [Lessons learned in software development](notes/engineering/careers/lessons-learned-in-software-dev.md) - Collected wisdom on development practices, debugging, team dynamics, and project management
- [Leveraging poor memory in engineering](notes/engineering/careers/leveraging-poor-memory-in-engineering.md) - Poor memory forces good habits: write everything down, automate, document decisions
- [Programmer competency matrix](notes/engineering/careers/programmer-competency-matrix.md) - 0-3 scale across 20+ skill categories for self-assessment; knowledge is cumulative per level
- [The purple developer - 10x productivity is contextual](notes/engineering/careers/purple-developer-10x-myth.md) - The 10x engineer is the one who built the system; spread the knowledge, spread the productivity
- [What makes a senior developer](notes/engineering/careers/so-you-want-to-be-senior.md) - Seniority is judgment, positive impact beyond code, and being a force multiplier for the team
- [The ACM/IEEE Software Engineering code of ethics](notes/engineering/careers/software-engineering-code-of-ethics.md) - Eight principles covering public interest, client duties, product quality, and professional judgment
- [Software engineering vs computer science](notes/engineering/careers/software-engineering-vs-computer-science.md) - CS focuses on algorithms; SE focuses on process management for complex software systems
- [12 years, 12 lessons at ThoughtWorks](notes/engineering/careers/twelve-lessons-at-thoughtworks.md) - Patrick Kua on tools vs thinking, agile transformations, safety for learning, and coding architects
- [Working as a software developer](notes/engineering/careers/working-as-a-software-developer.md) - Production software realities: programs are big, never done, and reading matters more than writing

### engineering/code-quality

- [Best practices for agile documentation](notes/engineering/code-quality/agile-documentation-best-practices.md) - Prefer executable specs, document stable concepts, keep it simple; fewer docs done well
- [The antipattern scripting language](notes/engineering/code-quality/antipattern-scripting-language.md) - Antipatterns are contextual; in throwaway scripts, they become good ideas that speed completion
- [Code for readability](notes/engineering/code-quality/code-for-readability.md) - Code as if the maintainer is a violent psychopath who knows where you live
- [Code review basics](notes/engineering/code-quality/code-review-basics.md) - Fundamentals of starting code review as a team practice; overcoming resistance and building habit
- [What to know before debating type systems](notes/engineering/code-quality/debating-type-systems.md) - Static vs dynamic, strong vs weak, nominal vs structural; most type debates use terms imprecisely
- [Deleting code](notes/engineering/code-quality/deleting-code.md) - Delete unused code permanently; version control is your safety net, not commented-out blocks
- [Effective code reviews without wasting time](notes/engineering/code-quality/effective-code-reviews.md) - One reviewer finds half of defects; beyond two reviewers, social loafing kicks in
- [Mastering programming](notes/engineering/code-quality/mastering-programming.md) - Kent Beck's practices: slicing problems, one thing at a time, concrete then abstract
- [No primitives - model domain concepts with types](notes/engineering/code-quality/no-primitives-domain-modeling.md) - Primitive Obsession code smell; wrap domain concepts in types to enforce invariants at construction
- [Programming practices - Unix philosophy and beyond](notes/engineering/code-quality/programming-practices-principles.md) - Timeless principles: prototype first, fail noisily, separate policy from mechanism, least surprise
- [10 modern software over-engineering mistakes](notes/engineering/code-quality/software-over-engineering-mistakes.md) - Anticipating futures, premature abstraction, shallow wrappers, and metrics over correctness
- [What makes code Swifty](notes/engineering/code-quality/swifty-code.md) - Three pillars: strong type safety, path to performance, clear expressive naming
- [Type wars - static vs dynamic typing history](notes/engineering/code-quality/type-wars.md) - Uncle Bob traces six decades of the type debate from Frege through Fortran to modern languages
- [Write code that is easy to delete, not easy to extend](notes/engineering/code-quality/write-code-easy-to-delete.md) - Lines of code are lines spent; build disposable software, not reusable software
- [How to write a successful conference proposal](notes/engineering/code-quality/writing-conference-proposals.md) - Proposals target reviewers, not audiences; the talk and the proposal are different skills
- [Writing good commit messages](notes/engineering/code-quality/writing-good-commit-messages.md) - 50-char summary, 72-char body wrapping; explain why, not what
- [Writing great documentation for open source](notes/engineering/code-quality/writing-great-documentation.md) - Start with empathy, linearize non-linear concepts, write a TOC of reader questions first
- [Writing perfect pull requests](notes/engineering/code-quality/writing-perfect-pull-requests.md) - Provide context, be explicit about feedback needs, ask questions rather than issue commands

### engineering/functional

- [Building web apps with functional programming](notes/engineering/functional/building-web-apps-with-functional-programming.md) - Full FP stack: Elm frontend, Haskell backend, NixOS infra for reproducible builds
- [Elm language overview](notes/engineering/functional/elm-language-overview.md) - Functional language compiling to JS; Model-View-Update pattern influenced Redux
- [Functional programming for the rest of us](notes/engineering/functional/fp-for-the-rest-of-us.md) - FP appears difficult due to presentation, not complexity; immutability enables easy testing and debugging
- [Functional thinking](notes/engineering/functional/functional-thinking.md) - Neal Ford on shifting from OO to FP mindset: composition over inheritance, Either over exceptions
- [Functional vs imperative vs declarative programming](notes/engineering/functional/functional-vs-imperative-vs-declarative.md) - Reference card for three paradigms: imperative (how), declarative (what), functional (pure transforms)
- [Good and bad Elixir](notes/engineering/functional/good-and-bad-elixir.md) - Anti-patterns: piping side effects, over-using with, hiding higher-order functions
- [Goodbye, Object Oriented Programming](notes/engineering/functional/goodbye-object-oriented-programming.md) - OOP's three pillars dismantled: banana-gorilla-jungle, diamond problem, fragile base class
- [Pragmatic functional programming](notes/engineering/functional/pragmatic-functional-programming.md) - Uncle Bob: FP matters beyond concurrency; immutability brings simplicity even on 4-core laptops
- [Railway Oriented Programming](notes/engineering/functional/railway-oriented-programming.md) - Two-track error handling: happy path and failure path via Either/Result types, no monad jargon
- [What is functional programming](notes/engineering/functional/what-is-functional-programming.md) - FP defined by side effects management: pure functions with declared inputs and outputs only
- [Which programming languages are functional](notes/engineering/functional/which-programming-languages-are-functional.md) - Side-effects management as the criterion; Haskell is genuine, JS is not, Clojure is 80%
- [Why OO sucks - Joe Armstrong's critique](notes/engineering/functional/why-oo-sucks.md) - Erlang creator's four objections: binding data to functions, everything-is-object, scattered types, private state

### engineering/go

- [Between Go and Elixir](notes/engineering/go/between-golang-and-elixir.md) - Complementary model: Elixir for orchestration and fault tolerance, Go for compute-heavy tasks
- [Building a worker pool in Go](notes/engineering/go/building-worker-pool-in-go.md) - Job queue, workers, and dispatcher pattern for bounded concurrency in Go
- [Channels in Golang](notes/engineering/go/channels-in-golang.md) - Channel types, buffering, nil/closed behavior, and edge cases for correct concurrent Go
- [Comparing Elixir and Go](notes/engineering/go/comparing-elixir-and-go.md) - Concurrency model comparison: preemptive actors with isolated heaps vs cooperative goroutines with channels
- [Effective error handling in Go](notes/engineering/go/effective-error-handling-in-go.md) - Indented flow pattern, custom error types, and idiomatic error handling practices
- [Elixir concepts for Go developers](notes/engineering/go/elixir-concepts-for-go-developers.md) - Actor model vs CSP comparison: addressable processes with mailboxes vs anonymous goroutines with channels
- [Error handling in Upspin](notes/engineering/go/error-handling-in-upspin.md) - Rob Pike's structured error type with Path, User, Op, Kind fields for rich context across boundaries
- [Four days of Go](notes/engineering/go/four-days-of-go.md) - C/Erlang developer's candid evaluation: fast compilation but syntax inconsistencies and missing features
- [Go best practices for production environments](notes/engineering/go/go-best-practices-for-production.md) - SoundCloud's Go in production: single GOPATH, flat repo structure, go fmt on save
- [Go best practices, six years in](notes/engineering/go/go-best-practices-six-years-in.md) - Peter Bourgon's core principle: make dependencies explicit across config, testing, and design
- [Go concurrency through illustrations](notes/engineering/go/go-concurrency-through-illustrations.md) - Visual introduction to goroutines, channels, and select using mining analogies
- [Go context should go away](notes/engineering/go/go-context-should-go-away.md) - Michal Strba argues Go's context.Context is a poor design that pollutes every function signature
- [Go performance optimization guide](notes/engineering/go/go-performance-optimization-guide.md) - Lock-free ring buffers 3x faster than channels; sync.Pool, escape analysis, and profiling tips
- [Go proverbs](notes/engineering/go/go-proverbs.md) - Rob Pike's Go proverbs: don't communicate by sharing memory, share memory by communicating
- [Go, REST APIs, and pointers](notes/engineering/go/go-rest-apis-and-pointers.md) - Pointer fields solve the zero-value vs intentionally-empty ambiguity in PATCH requests
- [Go testing principles by Dave Cheney](notes/engineering/go/go-testing-principles-dave-cheney.md) - Table-driven tests, test behavior not implementation, use t.Helper() and t.Run()
- [Go - the little language that could](notes/engineering/go/go-the-little-language-that-could.md) - Simplicity and pragmatism drove Go's rise past Swift, Scala, and Rust in language rankings
- [A closer look at Go's type system](notes/engineering/go/go-type-system-closer-look.md) - Named vs unnamed types, underlying types, and assignability rules that trip up Go developers
- [Go vs Swift comparison](notes/engineering/go/go-vs-swift-comparison.md) - Side-by-side comparison: static typing, concurrency models, memory management, error handling
- [Go 2 error handling draft design](notes/engineering/go/go2-error-handling-draft-design.md) - Proposed check/handle keywords to reduce if-err-nil boilerplate; ultimately not accepted
- [Idiomatic Go](notes/engineering/go/idiomatic-go.md) - Naming conventions, spelling, formatting, and style nuances from Go's standard library
- [A million WebSockets and Go](notes/engineering/go/million-websockets-and-go.md) - Mail.Ru optimized 3M concurrent WebSockets from 72 GB to manageable with epoll and buffer pooling
- [The generic dilemma in Go](notes/engineering/go/the-generic-dilemma-in-go.md) - Three approaches to generics: leave out (C), compile-time specialization (C++), boxing (Java)
- [Typed nils in Go](notes/engineering/go/typed-nils-in-go.md) - Interface holds (type, data); nil concrete value in interface is non-nil, breaking nil checks
- [Understanding nil in Go](notes/engineering/go/understanding-nil-in-go.md) - Nil is the zero value for 6 types; each behaves differently when nil, enabling idiomatic patterns
- [Why Go is a poorly designed language](notes/engineering/go/why-go-is-poorly-designed.md) - Seven design flaws: nil interface paradox, variable shadowing, slice pain, compiler rigidity
- [The Zen of Go](notes/engineering/go/zen-of-go.md) - Dave Cheney's Go principles: single-purpose packages, flat control flow, goroutine discipline

### engineering/principles

- [Bit Twiddling Hacks](notes/engineering/principles/bit-twiddling-hacks.md) - Stanford reference of bitwise tricks: branchless abs, popcount, De Bruijn log, Morton interleaving
- [Choose Boring Technology](notes/engineering/principles/choose-boring-technology.md) - Finite innovation tokens; spend them on business problems, not infrastructure novelty
- [Conway's law](notes/engineering/principles/conways-law.md) - System architecture mirrors org communication structure; you cannot fight it, only reshape it
- [Data drives code structure](notes/engineering/principles/data-drives-code-structure.md) - Software structure follows data structure: arrays become loops, graphs become traversals
- [Intro to compilers](notes/engineering/principles/intro-to-compilers.md) - Compiler pipeline: lexing, parsing, AST, optimization, code generation in plain language
- [Papers I like (part 1)](notes/engineering/principles/papers-i-like-part-1.md) - Fabian Giesen's 10 essential CS papers: Lamport, Herlihy, Cook, and more
- [Rob Pike's 5 rules of programming](notes/engineering/principles/rob-pike-five-rules-of-programming.md) - Measure before optimizing; fancy algorithms are slow when n is small; data dominates
- [Rust is not a good C replacement](notes/engineering/principles/rust-is-not-a-good-c-replacement.md) - Drew DeVault: Rust replaces C++, not C; 15 new features/year vs C's 0.73
- [Stack Overflow technical deconstruction](notes/engineering/principles/stack-overflow-technical-deconstruction.md) - Nick Craver's inside look at Stack Overflow infrastructure: radical transparency and embracing failure
- [Pattern matching with case let in Swift](notes/engineering/principles/swift-pattern-matching-case-let.md) - Swift's case let for destructuring enums, optionals, and tuples with pattern matching
- [Technical debt as a city metaphor](notes/engineering/principles/technical-debt-as-a-city.md) - Codebase as city: rushed construction, changing requirements, and patch culture cause decay
- [The 80x24 rule for code formatting](notes/engineering/principles/the-80x24-rule.md) - 80 chars wide, 24 lines tall per method; constraints nudge toward better design
- [UML as a communication tool](notes/engineering/principles/uml-as-communication-tool.md) - UML diagrams for requirements and design communication; modeling language, not a process
- [What if GitHub is the devil - curl's pragmatic take](notes/engineering/principles/what-if-github-is-the-devil.md) - Daniel Stenberg on why curl stays on GitHub: network effect, contingency plans, pragmatism over purity
- [Why big tech companies are so slow](notes/engineering/principles/why-big-tech-is-slow.md) - Feature interaction complexity grows combinatorially; slowness is math, not incompetence
- [Wisdom of programming quotes](notes/engineering/principles/wisdom-of-programming-quotes.md) - Henrik Warne's curated quotes on complexity, debugging, teams, and the nature of programming
- [The Zen of Python](notes/engineering/principles/zen-of-python.md) - PEP 20: beautiful over ugly, explicit over implicit, simple over complex, readability counts

## etymology

- [The Greek prefix para- means beside](notes/etymology/the-greek-prefix-para-means-beside.md) - `para-` = beside; paragraph was originally the margin stroke beside the text, not the text block

## finance

- [Financial knowledge as compound information advantage](notes/finance/financial-knowledge-as-compound-information-advantage.md) - Bille Finance narrative: information compounds like capital; the gap between Tier 1 and Tier 2 is learnable
- [How the bond market controls housing, stocks, and jobs](notes/finance/how-the-bond-market-controls-housing-stocks-and-jobs.md) - Yield seesaw sets mortgage rates, equity risk premium, and corporate refinancing costs; one chain from the 10-year

## finance-tooling

- [FinceptTerminal evaluation](notes/finance-tooling/fincept-terminal-evaluation.md) - AGPL-3 Qt6 Bloomberg-alternative; 10/15 BOOKMARK; AGPL §13 blocks integration into any network-facing trading stack
- [GeckoTerminal API evaluation](notes/finance-tooling/geckoterminal-evaluation.md) - Keyless H1/H4 OHLCV for DEX pools (SPL + EVM); 14/15 ADOPT; same data as $129/mo CoinGecko Pro, free
- [OpenBB evaluation](notes/finance-tooling/openbb-evaluation.md) - Python-first financial SDK (equities, options, macro, FRED); 11/15 BOOKMARK; crypto value thin, TradFi inflection point
- [OSS trading stack survey, April 2026](notes/finance-tooling/oss-trading-stack-survey-april-2026.md) - 3-category synthesis (execution frameworks / agentic AI / infra libs); Freqtrade + VectorBT canonical for semi-pro crypto; ai-hedge-fund as first vendor pilot
- [Why rotating ISP IPs break Binance API keys, and how to fix it with WireGuard](notes/finance-tooling/wireguard-static-ip-exchange-whitelist.md) - Cheap VPS + WireGuard beats every bundled static-IP product for exchange whitelisting; $5/mo beats $15-500/mo alternatives
- [Static outbound IP solutions for crypto trading bots, ten options compared](notes/finance-tooling/static-ip-solutions-compared-for-trading-bots.md) - 10 options across 5 categories; every commercial static-IP service is Category A (rent a VPS) re-bundled with markup
- [Vibe-Trading evaluation](notes/finance-tooling/vibe-trading-evaluation.md) - HKUDS multi-agent finance research workspace; LangGraph + 13 LLM providers + MCP server; 11/15 BOOKMARK; research-only (no live exec), heavy A-share / HK bias, 1 month old

## geopolitics

- [Australia's Washminster government structure](notes/geopolitics/australias-washminster-government-structure.md) - Australia blends Westminster parliament with US federalism; federal/state split complicates crisis response
- [How the 2026 Strait of Hormuz crisis impacts Australia](notes/geopolitics/how-the-2026-strait-of-hormuz-crisis-impacts-australia.md) - Hormuz closure hits Australia via Asian refineries; only 29-39 days fuel reserves
- [Measuring oil supply disruption severity](notes/geopolitics/measuring-oil-supply-disruption-severity-2026-hormuz-vs-historical-crises.md) - 2026 Hormuz removed 12 mb/d, largest oil disruption ever; 400 mb strategic reserve release

## health

- [Alkaline water health claims vs reality](notes/health/alkaline-water-health-claims-vs-reality.md) - Stomach acid neutralizes alkaline water instantly; premium price buys marketing
- [Vitamins and longevity stack](notes/health/vitamins-and-longevity-stack.md) - Daily supplement stack for anti-aging: NMN, Omega-3, Magnesium, CoQ10, and 12 more with dosages

## hiring

- [40 best questions to ask in an interview](notes/hiring/40-best-questions-to-ask-in-an-interview.md) - High-signal questions candidates should ask employers, organized by category
- [Assessing software engineering candidates](notes/hiring/assessing-software-engineering-candidates.md) - Bryan Cantrill's framework: written artifacts over pop quizzes; evaluate aptitude, motivation, values
- [Company culture is who you hire, fire, and promote](notes/hiring/company-culture-is-who-you-hire-fire-promote.md) - Culture defined by three actions, not mission statements
- [Developer Happiness Index 2021](notes/hiring/developer-happiness-index.md) - What makes developers happy at work; data on retention factors across regions
- [A developer's guide to interviewing](notes/hiring/developers-guide-to-interviewing.md) - Questions to evaluate employers as a developer candidate
- [Don't hire 0x engineers](notes/hiring/dont-hire-0x-engineers.md) - Against the 10x myth; build functional teams with capable people
- [Facebook hiring: strengths, builders, and learners](notes/hiring/facebook-hiring-strengths-builders-learners.md) - Three factors Facebook evaluates in every candidate at every level
- [How to hire](notes/hiring/how-to-hire.md) - Six principles: strengths over weakness, trajectory over experience, doers over talkers
- [How to hire programmers and outsourced developers](notes/hiring/how-to-hire-programmers.md) - Donn Felker's 4-step hiring process with programming challenges as the great equalizer

## history

- [China as a civilization state, not a nation state](notes/history/china-as-a-civilization-state-not-a-nation-state.md) - China is a 2,000-year civilization wearing nation-state clothing; continuity and order trump liberty
- [Da Nang's historical names: Tourane and Dogpatch](notes/history/da-nangs-historical-names-tourane-and-dogpatch.md) - Two outsider names; Tourane (French transliteration of Cửa Hàn) and Dogpatch (GI slang from Li'l Abner), neither used by locals
- [Imperial examinations: how China replaced religion with meritocracy](notes/history/imperial-examinations-how-china-replaced-religion-with-meritocracy.md) - 1,300-year exam system created cultural unity and loyal bureaucracy without a holy book
- [Israel, Palestine va Jerusalem](notes/history/israel-palestine-va-jerusalem.md) - History of the Israel-Palestine conflict from shared Abrahamic origins to modern territorial disputes
- [Predictive history and the ambition of psycho-history](notes/history/predictive-history-and-the-ambition-of-psycho-history.md) - Prof Jiang aims to build Asimov's psycho-history: connect past, explain present, predict future
- [Sinicization: how China absorbs its conquerors](notes/history/sinicization-how-china-absorbs-its-conquerors.md) - Every conqueror adopted Chinese civilization because demographics and bureaucracy made it inevitable
- [The Tainter trap: why complexity kills empires](notes/history/the-tainter-trap-why-complexity-kills-empires-and-chinas-reset-mechanism.md) - Empires die from complexity overload; China survives by expecting collapse and having a reset protocol

## investing

- [Compound interest levels and lifestyle progression](notes/investing/compound-interest-levels-and-lifestyle-progression.md) - Six wealth levels from $200/mo saver to $200M foundation; instruments change at each threshold
- [How and why I invest in startups](notes/investing/how-and-why-i-invest-in-startups.md) - Fund the best people on the hardest problems; measure both LP returns and happiness

## jupyter

- [Claude integration with Jupyter notebooks](notes/jupyter/claude-integration-with-jupyter-notebooks.md) - Three integration paths (NotebookEdit / jupyter-ai or NBI / Jupyter MCP); MCP is the agentic answer
- [Jupyter architecture: kernel, server, frontend](notes/jupyter/jupyter-architecture-kernel-server-frontend.md) - Three processes (frontend/server/kernel) over HTTP+WebSocket and ZeroMQ; kernel state plus arbitrary cell order is Jupyter's reproducibility footgun
- [Jupyter usage patterns and friction points](notes/jupyter/jupyter-usage-patterns-and-friction-points.md) - Six personas, one shared workflow, four mature team patterns; hidden state and unreadable diffs drive people out
- [Notebook landscape 2026: Jupyter alternatives and competitors](notes/jupyter/notebook-landscape-2026-jupyter-alternatives-and-competitors.md) - Three camps (Jupyter family, commercial cloud, reactive); Marimo is the post-Jupyter answer worth migrating to

## leadership

- [A decade of remote work](notes/leadership/a-decade-of-remote-work.md) - Ten years of remote lessons: go all-in or not at all, summits matter, hire for self-discipline
- [The consulting secret - ask your senior ICs what is broken](notes/leadership/consulting-secret-ask-the-ics.md) - Schedule 90 min with your best IC, ask what is broken, read it back to leadership
- [CTO vs VP Engineering](notes/leadership/cto-vs-vp-engineering.md) - CTO is outward-facing (vision, customers, innovation); VP Eng is inward-facing (delivery, process, people)
- [How to work out what to charge clients](notes/leadership/how-to-charge-clients.md) - Calculate your floor rate from costs and billability, then apply three markup factors
- [HR evaluation as unique value measurement](notes/leadership/hr-evaluation-unique-value.md) - Market value = Differentiation x Influence; uniqueness beats commoditized skill checklists
- [In pursuit of excellence](notes/leadership/in-pursuit-of-excellence.md) - Excellence comes from unique positioning at domain intersections, not being best at common skills
- [Lam an kieu Cu Ho](notes/leadership/lam-an-kieu-cu-ho.md) - Five business lessons from Ho Chi Minh's methods, applied at FPT Software by Nguyen Thanh Nam
- [Leadership in the agentic era](notes/leadership/leadership-in-the-agentic-era.md) - When agents absorb execution capacity, taste + trust + context become the scarce leadership work; coordination time collapses
- [How to manage people who are smarter than you](notes/leadership/managing-people-smarter-than-you.md) - Reframe management as enabling others' success; confront insecurity, learn from your team
- [Masayoshi Son and the SoftBank Vision Fund](notes/leadership/masayoshi-son-softbank-vision.md) - $100B fund betting AI runs the planet; gun-senryaku connects portfolio companies in a flock
- [Nguyen tac truc giac trong lanh dao](notes/leadership/nguyen-tac-truc-giac.md) - Shark Phu on "tuong" traits and Maxwell on intuition as the differentiator in leadership
- [Note to new design managers](notes/leadership/note-to-new-design-managers.md) - Practical guide: manage time, communicate definitively, document everything, protect craft quality
- [The rise of the interim CTO](notes/leadership/rise-of-the-interim-cto.md) - Temporary tech executive bridges the gap between coding founder and strategic tech leadership
- [Steve Jobs negotiation and persuasion tactics](notes/leadership/steve-jobs-negotiation-tactics.md) - Pitch with passion, be brutally honest, earn respect through work ethic, disarm with charm
- [Tips on working with talents](notes/leadership/tips-on-working-with-talents.md) - Four angles on using talent: genuinely need them, create worthy challenges, fair treatment, tolerate quirks
- [Why soldiers and chefs make the best product managers](notes/leadership/why-soldiers-and-chefs-make-best-pms.md) - Leading without authority under pressure with imperfect info; OODA loop for shipping decisions
- [Why you need engineering managers](notes/leadership/why-you-need-engineering-managers.md) - Coordination math, calendar incompatibility, and accountability require dedicated management roles

## life

- [100 little ideas](notes/life/100-little-ideas.md) - Morgan Housel's reference card of psychological and behavioral concepts; mental models for how the world actually works
- [Always be quitting: make yourself replaceable to grow](notes/life/always-be-quitting.md) - Being indispensable is a trap; 10 practices to make yourself replaceable and unlock growth
- [Average Joe](notes/life/average-joe.md) - An ordinary person in denial can still push far; the value is in refusing to stop trying
- [Be dispassionate about your software career](notes/life/be-dispassionate-about-software-careers.md) - Passion is a vulnerability employers exploit; invest in skills for self-preservation, not applause
- [Chon nguoi hop tac va ket giao](notes/life/chon-nguoi-hop-tac-va-ket-giao.md) - Ancient wisdom: 6 types to avoid in business, 7 to not befriend, 4 to keep close
- [Dang Le Nguyen Vu - 9 bai hoc nhan tinh the thai](notes/life/dang-le-nguyen-vu-nhan-tinh-the-thai.md) - Nine life lessons from Trung Nguyen's founder: self-reflect, build inner strength, accept impermanence
- [Great minds discuss ideas](notes/life/great-minds-discuss-ideas.md) - Three levels of discourse: people, events, ideas; the problem is when people become the endpoint
- [Hygge - the Danish concept of cosiness](notes/life/hygge-danish-concept-of-cosiness.md) - Hygge is a feeling of cosiness, not a lifestyle product; Danes created it to survive dark winters
- [John Vu on world class quality](notes/life/john-vu-on-world-class-quality.md) - Bill Gates observed: small civic behaviors reveal a country's education quality and national class
- [Laziness does not exist, only unmet barriers](notes/life/laziness-does-not-exist.md) - Situational factors predict behavior better than character; respond with curiosity, not judgment
- [Learning to say no without guilt](notes/life/learning-to-say-no.md) - Fear of rejection drives reflexive yes; three mental shifts and practical tactics for boundaries
- [The Munger Operating System for life](notes/life/munger-operating-system.md) - Charlie Munger's life principles: deserve what you want, lifetime learning, multidisciplinary thinking
- [Navagraha: nine celestial bodies in Hindu astrology](notes/life/navagraha-nine-celestial-bodies.md) - Nine planets of Hindu cosmology; seven map to weekdays, Rahu/Ketu are shadow nodes governing karma
- [Pavel Durov's secrets for success](notes/life/pavel-durov-secrets-for-success.md) - Telegram founder's principles: master what you love, read constantly, write daily, stay healthy
- [Simple burnout triage](notes/life/simple-burnout-triage.md) - One question: can you sustain the last 2 months forever? Three response levels from crisis to thriving
- [Tieu chuan cua ban la gi](notes/life/tieu-chuan-cua-ban-la-gi.md) - High standards vs low standards in work; the difference shows in how you treat details and quality
- [Time is the only real currency we have](notes/life/time-is-the-only-real-currency.md) - Engineers waste time on language tribalism and premature scale; invest in automation and tool mastery instead
- [To chat lanh dao kinh doanh](notes/life/to-chat-lanh-dao-kinh-doanh.md) - How a Vietnamese tech corp screens management trainees; energy and persuasion are the two key tests
- [Vipassana for hackers](notes/life/vipassana-for-hackers.md) - 10-day silent meditation course explained for rational minds; systematic self-observation as mind-hacking
- [We used to just live](notes/life/we-used-to-just-live.md) - Technology colonized every gap where unguided thinking happened; boredom was doing cognitive work we didn't realize
- [What it feels like to become poor](notes/life/what-it-feels-like-to-become-poor.md) - Lost $3M in 2008 crash, ended up at a car wash; humility came from the place he feared most
- [When and how to ask for help](notes/life/when-and-how-to-ask-for-help.md) - Timebox your struggle, then ask; balance between pestering and spinning your wheels
- [Why explore space - Stuhlinger's letter](notes/life/why-explore-space-stuhlinger-letter.md) - NASA scientist's 1970 defense of space spending: the microscope parable and satellites fighting hunger
- [Why we lie about being retired](notes/life/why-we-lie-about-being-retired.md) - Retirement is an identity crisis, not just a financial event; work provides meaning most can't replace
- [Working attitude principles](notes/life/working-attitude-principles.md) - Seven work ethic principles and ten anti-patterns; no industry is easy money, always harvest something

## local-llm

- [Local LLM hybrid stack: Ollama + Ollama Cloud + OpenRouter for Hermes Agent](notes/local-llm/local-llm-hybrid-stack-ollama-ollama-cloud-openrouter-for-hermes-agent.md) - Local-first three-tier escalation (Qwen local → Ollama Cloud → OpenRouter); routing rules and cost reasoning
- [Ollama Cloud :cloud suffix: hosted inference via local endpoint](notes/local-llm/ollama-cloud-cloud-suffix-hosted-inference-via-local-endpoint.md) - The `:cloud` suffix proxies through `localhost:11434`; same daemon serves local + hosted, switching is just a model name change
- [Qwen3.6-35B-A3B on M4 Pro: memory budget and context sizing](notes/local-llm/qwen3-6-35b-a3b-on-m4-pro-memory-budget-and-context-sizing.md) - Hybrid DeltaNet/attention architecture means 128k context fits in ~26 GB on 64 GB Apple Silicon; prefill cost is the real ceiling

## macos

- [1Password backup pattern for Apple developer signing certs](notes/macos/1password-backup-pattern-for-apple-dev-certs.md) - Split a codesigning `.p12` + its passphrase into two tagged 1Password items (Document + Password) so a lost keychain doesn't lose the cert; restore flow and the `security export -t identities` scoping caveat
- [Apple Containers: the macOS-native microVM runtime](notes/macos/apple-containers-overview-the-macos-native-microvm-runtime.md) - `apple/container` runs each container as its own Linux VM via the Apple Virtualization framework; OCI-compatible, macOS 26+ Apple Silicon only, what AWS Lambda would look like on a Mac
- [Firecracker microVMs do not run on macOS](notes/macos/firecracker-microvms-do-not-run-on-macos.md) - Firecracker requires Linux + KVM; reach for Apple Containers on Apple Silicon instead, otherwise you stack two layers of virtualization and pay the cost up front
- [macOS Input Method Kit (IMK) architecture and lifecycle](notes/macos/macos-input-method-kit-imk-architecture-and-lifecycle.md) - Out-of-process IM model, Mach IPC keystroke routing, IMKInputController lifecycle, and why Secure Input breaks every IME
- [macOS LaunchAgent/LaunchDaemon authoring for a BTM-friendly identity](notes/macos/macos-launchagent-launchdaemon-btm-friendly-plists.md) - `ProgramArguments[0]` must be the launcher's own path, no `.sh` extension on the entry point, `#!/bin/bash` not `env bash` so TCC grants survive a bash upgrade
- [macOS multi-user cost myth: it's the GUI session that's heavy, not the user](notes/macos/macos-multi-user-cost-myth-gui-vs-service-users.md) - 161 system service users coexist on one laptop for ~935 MB; multi-user GUI is heavy, multi-user services is essentially free, daemon-per-UID beats containers for mutually-trusted tenants

## math

- [Monomial polynomial term Vietnamese terminology breakdown](notes/math/monomial-polynomial-term-vietnamese-terminology-breakdown.md) - `thức` in `đơn thức` is NOT `công thức`; `-nomial` from Latin `nomen`; Vietnamese math vocab with QC tie-back to BQP and Ising Hamiltonians as degree-2 polynomials in Pauli operators

## mcp

- [MCP tool schema caching in Claude.ai connectors](notes/mcp/mcp-tool-schema-caching-in-claude-ai-connectors.md) - Claude.ai caches MCP schemas per session; disconnect+reconnect to force refresh
- [Security gates for MCP tools that bridge private to public](notes/mcp/security-gates-for-mcp-tools-that-bridge-private-to-public.md) - Server-side security gates: context-anchored secret scan, path traversal, cost-ordered pipeline

## networking

- [portless competitive landscape: no exact 1-to-1 competitor](notes/networking/portless-competitive-landscape-no-exact-1-to-1-competitor.md) - Quadrant map across reverse proxies, tunnels, and Tailscale; portless wins by being the only tool that explicitly aimed at the monorepo `.localhost` niche
- [portless vs Tailscale MagicDNS: not equivalent](notes/networking/portless-vs-tailscale-magicdns-not-equivalent.md) - portless is L7 application routing for one machine; MagicDNS is L3 cross-machine addressing; the naming overlap is superficial, the layer separation is what matters
- [SSH and Mosh, when each one wins](notes/networking/ssh-and-mosh-when-each-one-wins.md) - Why SSH dies on a wifi switch and Mosh does not; TCP-vs-UDP+SSP transport story, pick-each guide, two-side install recipe, UTF-8 + Apple Silicon PATH gotchas
- [Tailscale + NordVPN + iCloud Private Relay coexistence on iOS and macOS](notes/networking/tailscale-plus-nordvpn-plus-icloud-private-relay-coexistence-on-ios-and-macos.md) - Per-device design across Mac mini / Air / iPhone; conflict matrix and recovery paths; Mullvad-as-exit-node as the cleaner replacement for Nord
- [Tailscale VPN On Demand feature overview and rule semantics](notes/networking/tailscale-vpn-on-demand-feature-overview-and-rule-semantics.md) - iOS/macOS-only auto-connect on network change; "Except On home_wifi" + Cellular "Always" eliminates the "is Tailscale on?" cognitive overhead
- [When to add Tailscale to a personal dev surface](notes/networking/when-to-add-tailscale-to-a-personal-dev-surface.md) - Mesh VPN over WireGuard with proprietary control plane; collapses "reach my machine from anywhere" into a 5-minute SSO login; trade is metadata about device topology

## optimization

- [Operations Research and MILP for software engineers](notes/optimization/operations-research-and-milp-for-software-engineers.md) - OR is declarative problem solving with a solver (SQL planner / Z3 family); MILP is one technique inside mathematical optimization

## patterns

- [Pattern - Backends for Frontends (BFF)](notes/patterns/backend-for-frontend-pattern.md) - Dedicated backend per client type; avoids API bloat from serving diverse frontends
- [Redundant API pre-checks in wrapper functions](notes/patterns/redundant-api-pre-checks-in-wrapper-functions.md) - Wrapper checks file existence, then library re-checks internally; doubled API calls
- [Scope-boundary bugs, when the gate guards the wrong set](notes/patterns/scope-boundary-bugs.md) - A dedup, permission record, conformance gate, or metrics ledger consults a set that does not match the invariant's set; every error direction looks like a good outcome

## philosophy

- [Tao Te Ching + I Ching on timing and hidden preparation](notes/philosophy/tao-te-ching-i-ching-on-timing-and-hidden-preparation.md) - The hidden dragon still trains: wu wei is preparation during a red light, not idleness

## pkm

- [LLM Wiki pattern: compilation over retrieval](notes/pkm/llm-wiki-pattern-compilation-over-retrieval.md) - LLM compiles raw sources into interlinked wiki instead of re-deriving via RAG each time
- [Why knowledge notes need context, not just facts](notes/pkm/why-knowledge-notes-need-context-not-just-facts.md) - Default capture depth was TIL (shallow); changing default to Atomic Note fixed quality

## quantum

- [Complexity classes P NP BQP QMA explained](notes/quantum/complexity-classes-p-np-bqp-qma-explained.md) - Classical hierarchy (P, NP, NP-Complete, NP-Hard) extended for quantum with BQP and QMA; Shor lands factoring in BQP, BQP ⊆ NP is open
- [History and motivation of major quantum algorithms](notes/quantum/history-and-motivation-of-major-quantum-algorithms.md) - Shor / Grover / HHL / VQE-QAOA as a story of outsiders importing intuition from other fields; speedup requires structure; caveats are the whole story
- [Quantum superposition state and QFT for beginners](notes/quantum/quantum-superposition-state-and-qft-for-beginners.md) - Superposition is not "try all options in parallel"; speedup comes from interference canceling wrong answers; QFT is the engine inside Shor
- [State preparation is half the quantum algorithm](notes/quantum/state-preparation-is-half-the-quantum-algorithm.md) - Every algorithm has three stages; preparation is where exponential speedup claims die; HHL's dequantization was a preparation-cost story
- [Vietnamese terminology for quantum computing](notes/quantum/vietnamese-terminology-for-quantum-computing.md) - Working glossary: chồng chập for superposition, vướng víu for entanglement, when to keep English (algorithm names, acronyms)
- [What polynomial time actually means](notes/quantum/what-polynomial-time-actually-means.md) - Polynomial = highest exponent is fixed; n^10 is polynomial, 2^n is not; the threshold complexity theory draws between tractable and not
- [Why Quantum Computing Talks About Decision Problems](notes/quantum/why-quantum-computing-talks-about-decision-problems.md) - Decision problems are the assembly language of complexity theory; YES/NO falls out of the Born rule, makes apples-to-apples quantum-vs-classical comparison possible

## security

- [iCloud Advanced Data Protection: coverage, exclusions, and recovery model](notes/security/macos-icloud-advanced-data-protection-coverage-and-recovery.md) - ADP extends E2EE to almost everything in iCloud, but Mail/Contacts/Calendar stay Apple-readable forever; three-axis decision (coverage, recovery, operations)
- [Threat-model split: cross-tenant isolation vs per-agent damage containment](notes/security/threat-model-split-cross-tenant-isolation-vs-per-agent-damage-containment.md) - Two threats around AI-agent sandboxing look similar and need different solutions; "isolate from whom?" splits the conflation that produces wrong architectures
- [A git checkout mounted into an agent sandbox makes .git attacker-controlled; hand commits back as a bundle](notes/security/sandboxed-agent-git-checkout-bundle-handback.md) - Host git on an agent-writable checkout executes agent-chosen config (sshCommand, diff.external, filters, gitfile redirect, gh target); fix is a bundle fetched with fsck into a host-owned clone, never git inside the mount

## startup

- [Anatomy of software frauds](notes/startup/anatomy-of-software-frauds.md) - Three-layer fraud architecture: unlimited scapegoats, sales-driven culture, deceptive founding
- [Tap trung vao san pham](notes/startup/tap-trung-vao-san-pham.md) - When product is broken, sales and marketing accelerate failure; fix the product first
- [Tesla and GM - founders vs professional managers](notes/startup/tesla-gm-founders-vs-managers.md) - Steve Blank parallels Musk/Durant (visionary founders) vs Sloan (professional management)

## vietnam

- [The capital portfolio framework: beyond money](notes/vietnam/the-capital-portfolio-framework-beyond-money.md) - Seven capital forms (economic, trust, time, knowledge, network, symbolic, optionality); time is the only irreplaceable input
- [LKY on why Singapore can never build a Google: Vietnam comparison](notes/vietnam/lky-on-why-singapore-can-never-build-a-google-vietnam-comparison.md) - LKY's five constraints (size, brain drain, Confucian culture, comfort, takeovers); Vietnam inverts market size and risk culture, mirrors brain drain and scholar pull
- [LKY operating system: how to pick what to work on](notes/vietnam/lky-operating-system-how-to-pick-what-to-work-on.md) - Mid-career operating system: six mental models, eight-question filter, concentration over portfolio; 2055 question overrides everything
- [What young Vietnamese entrepreneurs should learn from LKY](notes/vietnam/what-young-vietnamese-entrepreneurs-should-learn-from-lky.md) - LKY-flavored founder advice for VN 20s-30s; drop billion-dollar framing, build defensible niches, Singapore as legal venue, plan for acquisition exit

## wealth

- [Anti-patterns that destroy trust permanently](notes/wealth/anti-patterns-that-destroy-trust-permanently.md) - Catalog of trust-killing behaviors; gossip and info leaks are unrecoverable
- [Enterprise trust ladder: vendor to strategic partner](notes/wealth/enterprise-trust-ladder-vendor-to-strategic-partner.md) - Five rungs from cold contact to strategic partner; pilot delivery is 60% process
- [How to sit at the table: the thesis](notes/wealth/how-to-sit-at-the-table-the-thesis.md) - Seek opportunity access and judgment transfer from elders, not money
- [The 12-month progression: deposit to partnership](notes/wealth/the-12-month-progression-deposit-to-partnership.md) - Trust builds in 4 phases over 12 months; most quit by month 3
- [The three gates: what elders screen for](notes/wealth/the-three-gates-what-elders-screen-for.md) - Invisible character, value, and role tests that gatekeep inner circles

## youtube

- [YouTube transcript extraction from cloud containers](notes/youtube/youtube-transcript-extraction-from-cloud-containers.md) - Node.js fetch ignores HTTPS_PROXY; fix with undici ProxyAgent for YouTube transcripts

## zed

- [Zed global agent rules live in the Rules Library, not AGENTS.md](notes/zed/zed-global-agent-rules-live-in-the-rules-library-not-agents-md.md) - No global file escape hatch; user-scope rules live in an LMDB database and need the paperclip icon to be default
