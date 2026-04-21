---
title: "FinceptTerminal evaluation"
date: 2026-04-19
captured: 2026-04-19T00:00:00.000Z
tags: ["finance-tooling", "evaluation", "bloomberg-alternative", "qt", "agpl"]
source: "Claude Code session evaluating whether to integrate into a private trading engine"
aliases: ["Fincept Terminal", "fincept"]
status: refined
---

## What it is

Open-source Bloomberg-alternative desktop terminal. C++20 + Qt6 + embedded Python 3.11. Dual-licensed AGPL-3.0 + commercial. ~4.7k stars, daily commits, built by Fincept Corporation (India).

Install: prebuilt binaries for Windows / Linux / macOS arm64 (v4.0.2). Source build needs Qt 6.8.3, MSVC 19.38 / GCC 12.3 / Apple Clang 15.0, CMake 3.27.7 pinned exactly.

Claims: 19k+ instruments, 100+ data connectors (Polygon / Kraken / Yahoo / FRED / IMF / World Bank / government APIs), 37 investor-persona AI agents (Buffett, Graham, Lynch, Munger, Klarman, Marks...), full CFA Level 1-3 curriculum as Python modules, DCF / VaR / Sharpe / portfolio-optimization / derivatives-pricing, 16 broker integrations (mostly Indian retail: Zerodha, Angel One, Upstox; plus IBKR, Alpaca, Tradier, Saxo).

Real-time trading is limited to a subset of crypto (Kraken / HyperLiquid WebSocket) and equity broker passthroughs. Not an execution framework in the Freqtrade / Jesse sense.

## 5-question rubric

**Q1 Layer (2/3)**: Research terminal / data viewer. Parallel to TradingView, OpenBB, Bloomberg. Sits outside the 8-layer AI dev stack; it's a financial tooling category. Clear slot, not pivotal.

**Q2 Replace or complement? (2/3)**: Claims to replace Bloomberg ($27k/yr) for equity analysts and fund managers. For a generic semi-pro trader, *complements* TradingView for macro data browsing (FRED + government APIs in one window is nice). Doesn't plug a felt gap unless the trader already feels "I need a macro dashboard."

**Q3 Credibility (2/3)**: 4.7k stars, active commits, dual-license with **contributor CLA assigning Fincept Corp perpetual commercial rights** on contributions. Classic open-core pattern. Solo-corp backing; one funder away from priority pivot. Commercial tier at $10.2k/yr targeting businesses. Medium credibility: the product is real, the business model extracts value from contributors.

**Q4 Adoption cost (3/3)**: <30 min for a prebuilt binary on macOS arm64 / Windows x64 / Linux x64. 1-2 weekends for a source build with exact version pins. Score the binary path.

**Q5 The kill question (1/3)**: What specific failure in a recent project would this have prevented? For a crypto-focused or event-driven trader: none. Signal quality + risk discipline are the bottleneck, not dashboard access. For an equity-research trader mixing macro + fundamentals: maybe, if they currently hop between 3-4 paid tools.

**Total: 10/15 → BOOKMARK**

Revisit in 30 days or when use case shifts to equities + macro research.

## Who should actually use it

- Equity traders / fund managers currently paying Bloomberg or multiple paid data feeds → genuine cost-saver if the data coverage holds up.
- University finance / econ programs → $799/mo for 20 accounts looks reasonable for a CFA curriculum wrapper.
- Anyone wanting one Qt desktop app for macro + equity research in one window.

## Who should skip

- Crypto-focused traders (even semi-pro). TradingView charting remains better; exchange APIs are already direct.
- Anyone embedding into a private trading stack. AGPL-3.0 §13 network-service clause makes code extraction radioactive if any network-facing component exists downstream.
- Retail Bloomberg-feature tourists. Installing 37 investor personas and a CFA curriculum doesn't make you Warren Buffett.

## License gotchas

- **AGPL-3.0 §13** triggers if any derivative is served over a network → must publish all source of that service under AGPL. Fatal for private SaaS / private trading bots with Telegram / API interfaces.
- **CLA on contributions** = Fincept Corp gets perpetual commercial rights to your patches. Don't upstream bug fixes expecting BSD-like freedom.
- **"Internal company use" requires commercial** per their own docs/COMMERCIAL_LICENSE.md — even $0-revenue startups, even no external distribution. Personal use on a personal account is clean; work laptop running it for personal trades is gray.
- **Fincept Data API** separate from the software; free tier for individuals, $10.2k/yr for commercial (65k credits/mo). Free external connectors (Yahoo / FRED / Kraken public) have no such restriction.

## Onboarding path (if you BOOKMARK → try)

1. Download the prebuilt binary for your OS. Skip source build unless you have a specific reason.
2. Don't sign up for Fincept Data API initially. Configure free connectors (FRED, Yahoo, Kraken public).
3. Set up one macro dashboard (10Y yields, DXY, sector indexes) + one equity research tab.
4. Do not connect any exchange API keys. Keep the tool read-only; execute elsewhere.
5. After 2 weeks: kept it open ≥3x unprompted AND made ≥1 decision informed by something hard to get elsewhere? Keep. Otherwise delete.

## Sources

- https://github.com/Fincept-Corporation/FinceptTerminal
- `LICENSE` file (AGPL-3.0 text + dual-license notice + CLA)
- `docs/COMMERCIAL_LICENSE.md` (pricing + commercial-use triggers)
- Marketing pitch that surfaced the tool: Sean Donahoe's post claiming "alpha on the table" (audience = equity fund managers and retail equity traders, not crypto H4)

#finance-tooling #evaluation #bloomberg-alternative #qt #agpl #research-terminal

## Related

- [[tool-evaluation-5-question-rubric]] - the rubric used to score this (scored 10/15 = BOOKMARK)
- [[ai-dev-stack-8-layer-model-with-tool-evaluations-march-2026]] - the 8-layer model is AI-dev focused; financial terminals need their own layer framing (future synthesis opportunity)
- [[how-the-bond-market-controls-housing-stocks-and-jobs]] - macro research is the use case where this tool adds the most value
