---
title: "GeckoTerminal API evaluation"
date: 2026-04-20
captured: 2026-04-20T00:00:00.000Z
tags: ["finance-tooling", "evaluation", "crypto-data", "dex", "ohlcv", "coingecko", "solana", "evm"]
source: "Claude Code research session comparing public-tier OHLCV providers for a semi-pro crypto trading engine"
aliases: ["GeckoTerminal", "CoinGecko DEX API", "api.geckoterminal.com"]
status: refined
---

## What it is

REST API for DEX-pair OHLCV and on-chain price data across every chain CoinGecko indexes. Operated by CoinGecko (the same company behind the long-running crypto aggregator), released 2023, stabilized through 2024-2025, still carrying a **beta** flag on the public docs as of April 2026 but effectively production for public read-only use.

Entry point: `GET https://api.geckoterminal.com/api/v2/networks/{network}/pools/{pool_address}/ohlcv/{timeframe}?aggregate={N}&limit={≤1000}&currency=usd`. Returns OHLCV bars as nested JSON arrays `[[ts, open, high, low, close, volume_usd], ...]`, newest first.

The endpoint you care about if you came here from a signals / backtest use case:

```
GET /api/v2/networks/solana/pools/{POOL_ADDR}/ohlcv/hour?aggregate=1&limit=168&currency=usd  # 1h, 7 days
GET /api/v2/networks/solana/pools/{POOL_ADDR}/ohlcv/hour?aggregate=4&limit=180&currency=usd  # 4h, 30 days
```

Timeframes: `minute` (aggregates 1/5/15), `hour` (aggregates 1/4/12), `day` (aggregate 1). So H1 and H4 are first-class.

Auth: **keyless**. No signup, no API key, no OAuth. Rate-limited at ~30 req/min per IP on the free public tier. Paid tiers exist via CoinGecko Pro plans but the exact same DEX endpoints are available without them.

Install: `pip install httpx` (or use any HTTP client). No SDK; you hit the REST surface directly. First working call runs in ~15 minutes from zero.

Backing: CoinGecko Pte Ltd, Singapore. VC-funded, profitable, 10+ years old. Not a solo-dev side project.

## 5-question rubric

**Q1 Layer (2/3)**: On-chain / DEX OHLCV data provider. Category peers: Birdeye, Moralis, CryptoCompare, Coin Metrics, CoinAPI. Clear slot. Not pivotal for anyone who only trades CEX-listed assets (ccxt covers that cheaper).

**Q2 Replace or complement? (3/3)**: Complements ccxt. For anyone running indicators on DEX-native tokens (Solana SPL like JUP/BONK/WIF, or EVM DeFi tokens without a major CEX listing), it plugs a real, frequently-felt gap. Replaces three paid alternatives at this specific task: CoinGecko Pro Analyst ($129/mo) which serves the same underlying data, Birdeye Premium Plus ($250/mo) for base OHLCV, Moralis Pro ($49/mo) except Moralis wins on history depth.

**Q3 Credibility (3/3)**: Highest-credibility free OHLCV source for DEX data. CoinGecko brand, real company, disciplined changelog, stable endpoints since 2023. Still beta-flagged in docs which is mild risk but the API hasn't had breaking changes in >18 months. Not Reddit's top-of-mind (that's CoinGecko main API or Birdeye for Solana), but the underlying data is identical to CoinGecko's paid Analyst tier per their own DEX documentation — so any practitioner who's paying for Analyst to get DEX bars is leaving money on the table.

**Q4 Adoption cost (3/3)**: ~30 minutes to a working fetch. Keyless = no signup flow, no 1Password entry, no secrets rotation, no CI flakiness from missing creds. Pool-address resolution is the only friction: you need to map token symbol → pool address once per asset (via the `/search/pools` endpoint), then cache that mapping. Cost is one HTTP call per new token added to a watchlist.

**Q5 The kill question (3/3)**: In the last few projects — yes, concretely. Any signals engine that only pulls Binance klines silently stops alerting when the user holds a token not on Binance spot. The fetch raises per-symbol, the run loop catches and logs, and the user gets zero signal on the exact asset they're watching. GeckoTerminal closes that gap without a paid contract, without an API key, and without changing the OHLCV shape consumers expect.

**Total: 14/15 → ADOPT NOW**

This is a rare score. It reflects that (a) the alternative is a silent-failure bug, and (b) adoption cost is minimal.

## Who should use it

- Semi-pro crypto traders running indicators on **DEX-native tokens** (Solana SPL, EVM DeFi) where Binance doesn't have a USDT pair.
- **Backtest workloads** up to 6 months of history per pool. Deeper history is the one case where you need a paid alternative.
- Teams building **MCP / agent tooling** for crypto data where keyless = no secret-management burden.
- Devs wiring a **signal or alert engine** that needs 1h or 4h OHLCV on tokens ccxt doesn't cover.

## Who should skip

- CEX-only traders. ccxt covers Binance, Bybit, Kraken, Coinbase better, faster, and also free.
- Anyone needing **>6 months** of 1h or 4h history. Hit Moralis Pro ($49/mo) instead or pair the two.
- Institutional users who need **SLA guarantees**. This is a public API with a beta flag; it's reliable in practice but unsigned as a commercial contract.
- Anyone who wants OHLCV via **WebSocket streaming**. GeckoTerminal is polling-only; you want Birdeye or a chain-indexer.

## Comparison: public-tier OHLCV providers for DEX tokens (April 2026)

| Provider | OHLCV? | 1h native | 4h native | Auth | Free tier | DEX coverage | Monthly cost |
|---|---|---|---|---|---|---|---|
| **GeckoTerminal** | ✓ native | ✓ `aggregate=1` | ✓ `aggregate=4` | keyless | 30 rpm, 1000 bars/call, 6mo history | All chains, pool-level | $0 |
| CoinGecko free | ✓ daily only | ✗ (paid) | ✓ on 3-30d window | demo key | 30 rpm | No DEX pools | $0 |
| Birdeye Standard | ✗ (OHLCV gated) | n/a | n/a | API key | 30k CU/mo | SPL + EVM but no OHLCV on free | $0 |
| Birdeye Premium Plus | ✓ | ✓ | ✓ | API key | n/a | SPL + 10 EVM, richer metadata | $250 |
| DexScreener | ✗ | ✗ | ✗ | keyless | 60 rpm | Great for discovery, not bars | $0 |
| DefiLlama coins | ✗ (points only) | ✗ | ✗ | keyless | Generous | Multi-chain, no OHLC split | $0 |
| Jupiter Price v3 | ✗ (spot only) | ✗ | ✗ | free-tier migrating | varies | Solana mark only | $0 |
| CoinGecko Pro Analyst | ✓ | ✓ hourly | ✗ (resample) | paid key | 500 rpm | Same DEX data as GeckoTerminal free | $129 |
| Moralis Pro | ✓ | ✓ | ✓ | paid key | 15M CU/mo | SPL (Raydium/Orca/Meteora/Pump) + EVM | $49 |
| Tiingo Power | ✓ | ✓ resample | ✗ (resample) | paid key | 500/hour | CEX-only, no DEX | $10 |
| Polygon.io / Massive | ✓ | ✓ | ✓ | paid key | unlimited | Coinbase + Kraken, **no Binance, no DEX** | $29 |
| Coin Metrics Community | ✓ | ✓ | ✓ | free | 1.6 rps | CEX + some DEX, **CC-NC license** | $0 (non-commercial) |
| Kaiko | ✓ | ✓ | ✓ | paid contract | n/a | Full institutional | $790–4,580 |

The takeaway: GeckoTerminal is the dominant free choice for DEX OHLCV. Moralis Pro at $49 is the right *paid* upgrade if you need longer history. Everything else on the paid side is either (a) CEX-only, (b) strictly worse per-dollar than Moralis, or (c) institutional pricing that doesn't match semi-pro scale.

## Comparison: GeckoTerminal vs Moralis Pro

| Dimension | GeckoTerminal (free) | Moralis Pro ($49/mo) |
|---|---|---|
| OHLCV native 1h + 4h | ✓ | ✓ |
| Auth | keyless | API key |
| Rate limit | 30 rpm per IP | 60 CU/s, 15M CU/mo |
| Max history per call | 1000 bars | documented "generous," no hard cap |
| History window per pool | 6 months | not hard-capped |
| Coverage | Every chain indexed by CoinGecko | Solana (Raydium/Orca/Meteora/Pump) + EVM |
| Unique value | Zero friction, zero spend | Deeper history for long backtests |
| Fragility | Beta flag; rate-limit hits on bursts | API-key revocation; CU budgeting needed |

Rule of thumb: start with GeckoTerminal. Add Moralis only when you actually hit the 6-month ceiling during a specific backtest.

## Onboarding path (if you ADOPT → try this week)

1. Resolve a pool address for one token you want to track:
   ```bash
   curl 'https://api.geckoterminal.com/api/v2/search/pools?query=JUP'
   ```
   Pick the top pool by `reserve_in_usd`. Save the `(network, pool_address)` tuple.

2. Fetch 4h bars for that pool:
   ```bash
   curl 'https://api.geckoterminal.com/api/v2/networks/solana/pools/{POOL}/ohlcv/hour?aggregate=4&limit=90&currency=usd'
   ```
   Returns `{data: {attributes: {ohlcv_list: [[ts, o, h, l, c, v], ...]}}}`.

3. Shape-match to ccxt. ccxt's `fetch_ohlcv` returns `list[[ts_ms, o, h, l, c, v]]`. GeckoTerminal returns Unix seconds — multiply by 1000. Now your signal rules don't branch on source.

4. Cache pool-address resolution. One HTTP call per new token added to a watchlist; persist the mapping to a local Parquet or JSON file. Pool addresses rarely change (new pools spin up, but top-volume pool stays stable across months).

5. Wrap in a retry layer. 429s happen on bursts; linear backoff 60/90/120s mirrors how Jupiter and 1inch rate-limit and is the standard pattern for 30-rpm APIs.

Do not use this for execution. GeckoTerminal is a data layer. For Solana swaps you want Jupiter; for EVM swaps 1inch or 0x; for CEX any ccxt-supported venue.

## Gotchas

- **Beta flag on docs**. Endpoints are stable in practice (>18 months no breaking changes) but CoinGecko technically reserves the right to change them. Cassette your integration tests (SPEC-011-style VCR pattern) so drift is caught.
- **Pool selection matters**. A token often has 5-20 pools across DEXes; the top-liquidity pool dominates volume but if it ever flips to a different pool, your OHLCV stream silently degrades to low-liquidity price noise. Re-resolve periodically or when volume drops.
- **`/search/pools` is fuzzy**. Search by symbol can match unrelated tokens (`BONK` matches several). Search by contract address when possible.
- **6-month hard cap**. You cannot fetch bars older than ~180 days from the current timestamp. Fine for signal-engine cache warmup; not fine for multi-year backtest. This is the one case Moralis Pro wins.
- **Timezone**. Bars are UTC-aligned. Documented but easy to assume otherwise.
- **CoinGecko Pro ≠ GeckoTerminal improvement**. Paying $129/mo for CG Analyst to get "DEX data" is a common footgun — the DEX endpoints are identical and free via GeckoTerminal. Pro's value is elsewhere (higher rate limits on the *main* CG API, more historical depth on CEX tokens), not on the DEX OHLCV surface.

## Reddit / community consensus

- **r/algotrading's crypto-data-API threads** mention GeckoTerminal less than expected. Community defaults for crypto OHLCV still lean toward ccxt+Binance (CEX-only) and, for Solana specifically, the DexScreener + Birdeye + Jupiter trio — none of which actually serve OHLCV on free tiers. The gap GeckoTerminal fills is rarely articulated explicitly, which is either opportunity or a clue nobody needs it.
- **Freqtrade community** ([issue #10377](https://github.com/freqtrade/freqtrade/issues/10377)) has told users Hyperliquid is the only DEX viable via ccxt; there's no framework-native path to SPL OHLCV. Anyone trading Solana on Freqtrade is either paying Moralis, building against DexScreener with rate-limit pain, or running blind.
- **DexScreener rate-limit pain** is a recurring dev-blog theme ([Medium — handling DexScreener rate limits](https://medium.com/@hakeemhal/handling-dexscreener-api-rate-limits-with-tokio-mpsc-no-redis-needed-9ed9adc68d8c)). Reinforces the case for GeckoTerminal's 30 rpm + 1000 bars/call being materially more generous per-bar than DexScreener's 60 rpm × 1 point per call.

## Re-evaluate when

- GeckoTerminal exits beta and announces paid-only tiers — would shift the free verdict but not the "best OHLCV for DEX" conclusion.
- CoinGecko adjusts free-tier rate limit below 10 rpm — would force a paid fallback earlier.
- You need >6 months of 1h history for a specific backtest — Moralis Pro time.
- Birdeye moves core OHLCV into their Standard tier — would make Birdeye a real free competitor.

## Sources

- [GeckoTerminal API Guide](https://apiguide.geckoterminal.com)
- [GeckoTerminal Swagger docs](https://api.geckoterminal.com/docs/index.html)
- [CoinGecko free OHLC endpoint (main API)](https://docs.coingecko.com/reference/coins-id-ohlc)
- [CoinGecko Pro pricing](https://www.coingecko.com/en/api/pricing)
- [Moralis Solana OHLCV](https://docs.moralis.com/web3-data-api/solana/reference/price/get-ohlcv-by-pair-address)
- [Moralis pricing](https://moralis.com/pricing/)
- [Birdeye Data Services pricing](https://bds.birdeye.so/pricing)
- [DexScreener API reference](https://docs.dexscreener.com/api/reference)
- [Massive (Polygon.io) pricing](https://polygon.io/pricing)
- [Tiingo crypto docs](https://www.tiingo.com/documentation/crypto)
- [Coin Metrics Candles](https://gitbook-docs.coinmetrics.io/market-data/market-data-overview/candles)
- [Kaiko pricing (via Vendr)](https://www.vendr.com/buyer-guides/kaiko)
- [Freqtrade DEX issue #10377](https://github.com/freqtrade/freqtrade/issues/10377)
- [DexScreener rate-limit pain (Medium)](https://medium.com/@hakeemhal/handling-dexscreener-api-rate-limits-with-tokio-mpsc-no-redis-needed-9ed9adc68d8c)
- [CoinGecko learn - Best historical crypto data APIs](https://www.coingecko.com/learn/best-historical-crypto-data-apis)
- [Coinpaprika - Best free DEX APIs 2025](https://coinpaprika.com/education/best-free-dex-api-2025-dexpaprika-vs-dextools-vs-geckoterminal-vs-dexscreener-vs-birdeye/)

#finance-tooling #evaluation #crypto-data #dex #ohlcv #coingecko

## Related

- [[openbb-evaluation]] — broader research SDK; adjacent but different slot (macro / TradFi, not DEX crypto)
- [[fincept-terminal-evaluation]] — category peer, 10/15 SKIP; different use case (terminal UI, not API)
- [[oss-trading-stack-survey-april-2026]] — execution frameworks (Freqtrade, VectorBT, Jesse); the framework your GeckoTerminal feed eventually plugs into
- [[tool-evaluation-5-question-rubric]] — scoring source of truth
