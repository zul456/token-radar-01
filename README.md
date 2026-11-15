# token-radar-01

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/zul456/token-radar-01/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/zul456/token-radar-01/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

Token launch radar for make.fun's token-radar-01 bounty. The program reads a sealed DefiLlama snapshot, ranks every pre-token launch candidate by an implied value, and writes a dashboard a reader can use: a sortable, filterable board, a detail view for each protocol with its model and farm plan, and the live-token protocols kept apart with their market caps.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input ../token-radar-01-kit/input/snapshot.json
```

---

Entered on [make.fun](https://make.fun/?ref=kz2xm0e8&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards

## The model (radar-2)

`implied = round(tvl_usd x 0.30 x growth x maturity x breadth x sector)`

- **growth** — `1 + clamp(change_7d, -60, 200) / 100 x 0.5`: a 7-day TVL move, capped so one spike cannot carry a row.
- **maturity** — `0.5 + 0.5 x sqrt(min(age_days, 1460) / 1460)`: how long the protocol has run with no token; rises fast in year one, then flattens. Unknown age counts as 365 days.
- **breadth** — `1 + 0.02 x min(chains - 1, 15)`: distribution across chains is worth something.
- **sector** — 1.15 for fee-generating venues (DEXs, lending, derivatives, CDP, yield, basis trading, new chains, prediction markets), 0.90 for low-fee niches (farms, insurance, governance incentives, card issuers, indexes), 1.00 for the broad middle.

A launch candidate records no token and is not a wrapped, staked or bridged product of a token that already trades — the watchlist's own rule, applied to every row. The board covers all 40 watchlist protocols plus every other launch candidate (197 rows on this snapshot), and the live-token protocols are shown separately with their market caps, never mixed into the ranking.

Every farm plan is derived from the protocol's own record (category, chains, TVL, age): seven steps with concrete costs and times, never a hand-written list of names. The dashboard recomputes each row from the same figures, so its code and `board.json` agree.

Node 22+, standard library only, no network. The program writes `out/board.json`, `out/data.js`, `out/dashboard/`, `out/screenshots/` and `out/dossier.json`. Two runs write identical bytes.
