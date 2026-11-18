# Token Radar

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/Hendr1010/token-radar-entry/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/Hendr1010/token-radar-entry/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

A token launch radar: it reads a sealed DefiLlama snapshot of 758 protocols, finds the 198 genuine pre-token launch candidates, ranks them by an implied value derived from the snapshot's own figures, and hands in a web dashboard with a ranked board, a detail view and farm plan for each protocol, and the category benchmarks the model is built from.

## Model (radar-2)

Implied value follows from the snapshot alone — no protocol name is ever special-cased:

```
benchmark  = category median market-cap / TVL among live protocols in this snapshot,
             shrunk toward the global median (0.17x) with 3 pseudo-observations,
             clamped to [0.05, 1.5]; a category needs 3+ live comps, else the global median
growthMult = 1 + clamp(change_7d, -50, 100) / 100 x 0.5
maturity   = 0.6 + min(age_days, 1095) / 1095 x 0.4   (age unknown: 365 days)
implied    = round(tvl_usd x benchmark x growthMult x maturity)
```

The board covers all 40 watchlist protocols plus 158 early candidates below the $50M floor, every one passing the watchlist's own filter (no token on record, not a wrapped, staked or bridged product, not an exchange product, not a versioned pool). Live-token protocols are set aside with their market caps shown — they set the benchmarks, they are never mixed into the ranking.

Top of the board: Steakhouse Financial ($367M implied), Sentora Curator ($353M), Gauntlet ($219M), USDD ($145M), Veda ($109M).

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <kit input dir>
```

Node 22, standard library only, no network, deterministic (two runs write identical bytes). Writes `board.json`, `data.js`, `dashboard/` and `screenshots/` to the output dir, plus `dossier.json`.

## License

MIT — see [LICENSE](LICENSE).
