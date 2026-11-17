# makefun-tiny-chess-01

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/Keso1234/makefun-tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/Keso1234/makefun-tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A tiny original chess engine for the make.fun tiny-chess-01 bounty: one
self-contained `main.py` (4,094 bytes), Python 3.13 stdlib only, no model,
no copied code. It plays the kit's 16-game gauntlet over the env HTTP API
and is fully deterministic: a fixed node budget ends the search, never a
clock; the only randomness is a seeded PRNG for the Zobrist keys.

## The engine (v11)

- 0x88 board with incremental Zobrist hashing.
- Negamax with alpha-beta, iterative deepening, PVS at the root,
  and aspiration windows from depth 3.
- Fixed node budget of 250,000 nodes per move, counting search and
  quiescence together.
- Quiescence over captures, promotions and check evasions, with a
  cheap capture filter.
- Transposition table, null-move pruning, late-move reductions, check
  extensions, killer moves and a history table for move ordering.
- Evaluation: material plus per-piece piece-square tables, bishop-pair
  bonus and a small tempo bonus, all incremental.
## Local practice results

Local kit-env-and-scorer measurements, 16 games each. Dev gate is
trial-0 only (floor 1,154); sunfish baseline mean is 1,360.

| instance | Elo | W-D-L |
|----------|-----|-------|
| trial-0 | 1396 | 7-2-7 |
| trial-1 | 1362 | 7-1-8 |
| trial-2 | 1173 | 5-0-11 |
| mean | 1310 | |

The v1 entry scored 1,087 locally on trial-0 (997 on the server).
The slowest uncontended move took 7.6 s of the 10 s limit.

## Trial-2 re-run

The first trial-2 run shared this VM with trial-1 and lost two games
on time (Elo 1,131); a clean sequential re-run scored 1,173 with no
time loss. The server runs each trial uncontended, so the clean
figures are representative, and the dev gate is trial-0 only (1,396
here). Verification: perft passes, two fresh-process self-plays
agree, and every Elo figure comes from a full 16-game trial scored
with the kit scorer.

## Running it locally

Start the kit env, then run the engine against it:

```
MAKEFUN_ENV_URL=http://127.0.0.1:8081 python3 -P -s main.py
```

`main.py` is an LZMA-compressed payload plus a two-line unpacker.

Built for make.fun - try the bounties yourself: https://make.fun/r/taaana65
