# Learned

A personal knowledge base following the [Zettelkasten](https://en.wikipedia.org/wiki/Zettelkasten) methodology, maintained by LLMs using the [LLM Wiki](https://gist.github.com/karpathy/1dd0294ef9567971c1e4348a90d69285) pattern.

205 interlinked notes across 21 topics. Browse in [Obsidian](https://obsidian.md/) for graph view and backlinks, or read the [full index](index.md).

## Recent additions

- `2026-04-13` [Note to new design managers](leadership/note-to-new-design-managers.md) - Hardik Pandya's practical guide for new design managers
- `2026-04-13` [Why you need engineering managers](leadership/why-you-need-engineering-managers.md) - Charity Majors on coordination math and the EM role
- `2026-04-13` [Tập trung vào sản phẩm](startup/tap-trung-vao-san-pham.md) - Khi sản phẩm có vấn đề, tập trung sửa sản phẩm trước
- `2026-04-13` [A decade of remote work](leadership/a-decade-of-remote-work.md) - Viktor Petersson's 10 years of lessons running remote teams
- `2026-04-13` [Conway's law](engineering/conways-law.md) - Org structure constrains system design
- `2026-04-13` [Anatomy of software frauds](startup/anatomy-of-software-frauds.md) - Three-layer architecture of tech fraud and protection strategies
- `2026-04-13` [DevOps team topologies](engineering/devops-team-topologies.md) - Matthew Skelton's framework for DevOps team structures
- `2026-04-13` [Heisenberg developers](engineering/heisenberg-developers.md) - Measuring developers changes their behavior; autonomy over micromanagement
- `2026-04-13` [How to manage people smarter than you](leadership/managing-people-smarter-than-you.md) - Enable others' success instead of being the expert
- `2026-04-13` [Tesla and GM - founders vs managers](startup/tesla-gm-founders-vs-managers.md) - Steve Blank on the visionary-to-operator transition

## How it works

```
Obsidian Clipper (raw) ──┐
Claude Code (refined) ───┤──► this repo ──► Obsidian reads/lints/links
Claude AI skill (refined)┘
```

- **Ingest**: new knowledge arrives from web clips, coding sessions, or research conversations
- **Compile**: the LLM checks for overlaps, flags contradictions, updates cross-references and synthesis pages
- **Query**: ask questions against the compiled wiki; good answers get filed back as new notes
- **Lint**: periodic health checks for orphans, broken links, stale claims

The human thinks and curates. The LLM handles the bookkeeping.

## Topics

| Folder | Notes | Domain |
|--------|-------|--------|
| [engineering/](engineering/) | 91 | Software engineering practices, principles, languages |
| [life/](life/) | 25 | Life philosophy, habits, mindset, career wisdom |
| [cs/](cs/) | 9 | Computer science fundamentals |
| [dwarves-kit/](dwarves-kit/) | 9 | Dwarves Kit architecture and design |
| [leadership/](leadership/) | 15 | Management, negotiation, business leadership |
| [ai-tooling/](ai-tooling/) | 8 | AI developer tools and evaluations |
| [ai/](ai/) | 7 | AI concepts, memory systems, agent patterns |
| [diaspora/](diaspora/) | 6 | Vietnamese and Asian diaspora analysis |
| [claude-code/](claude-code/) | 5 | Claude Code hooks, skills, workflows |
| [history/](history/) | 5 | Chinese civilization, empires, historical patterns |
| [wealth/](wealth/) | 5 | Trust-building, business relationships |
| [geopolitics/](geopolitics/) | 3 | Oil crises, government structures |
| [health/](health/) | 2 | Wellness, nutrition |
| [investing/](investing/) | 2 | Compound interest, startup investing |
| [mcp/](mcp/) | 2 | Model Context Protocol |
| [pkm/](pkm/) | 2 | Personal knowledge management |
| [devtools/](devtools/) | 2 | Developer tools and config |
| [startup/](startup/) | 3 | Startup strategy, product focus, fraud patterns |
| [hiring/](hiring/) | 1 | Candidate assessment and hiring |
| [patterns/](patterns/) | 1 | Software patterns and anti-patterns |
| [youtube/](youtube/) | 1 | YouTube tooling |

## Documentation

- [Full note index](index.md) - catalog of all 205 notes with one-line summaries
- [Usage guide](_docs/guide.md) - how to add notes, use Obsidian, work with Claude
- [Architecture](_docs/architecture.md) - system design, folder conventions, operations model
- [Requirements](_docs/requirements.md) - feature tracker, design principles, scaling triggers
- [Changelog](_docs/changelog.md) - project decisions log
- [Operations log](log.md) - chronological record of ingests, queries, lints
