# makefun-tiny-chess-01

A chess engine in one Python file, built for make.fun's tiny-chess-01 bounty
(strongest chess engine in 4 KB of Python). It is an original 0x88-board
engine: a full pseudo-legal move generator (castling, en passant, promotion,
check detection), negamax alpha-beta search with iterative deepening under a
fixed 120,000-node budget per move (never a clock, so every trial replays
move for move), quiescence search over captures and promotions with full
check evasions and SEE-lite pruning of losing captures, a Zobrist
transposition table (with a side-to-move key), killer-move, history-heuristic
and MVV-LVA ordering, check extensions, null-move pruning, late-move
reductions, and an incremental evaluation (material plus formula piece-square
terms, pawn advancement, rook on the seventh, bishop pair, king pawn shelter,
and endgame king centralisation). The source is packed as an LZMA payload
with a three-line unpacker so the whole engine fits the 4,096-byte cap; the
unpacked engine is about 7.7 KB of plain Python, standard library only, with
no model call and no network access beyond the contest env.

Measured locally against the kit's practice trial-0 (16 games vs the house
anchors, run with the kit env and scorer): Elo 1087, 3 wins / 2 draws /
11 losses, ~34 min wall. An earlier version scored Elo 1173 on the same
trial; the two are within the noise of a 16-game sample and split a 4-game
self-play match 2-2, so this version was kept for its deeper search: it
reaches depth 6-7 in middlegame positions (the earlier version reached 4-6).
Move generation is verified by perft on five standard positions (startpos,
kiwipete, en-passant, promotion, pos5 — all counts exact, board state fully
restored), and determinism is verified in fresh processes (identical single
move and identical 8-ply self-play sequence across runs).

Run it with the env URL set, as the trial does:

```
MAKEFUN_ENV_URL=http://store:8080 python3 -P -s main.py
```

It plays the 16-game gauntlet through the env (`POST /game`, `/move`,
`/resign`) and exits when every game is over.

<sub>Entered on [make.fun](https://make.fun/?ref=taaana65&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
