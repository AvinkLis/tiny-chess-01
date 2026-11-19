[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/AvinkLis/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/AvinkLis/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

# Tiny chess engine in 4 KB of Python — v3

A chess engine written from scratch for make.fun's tiny-chess-01 bounty.
`main.py` is the whole engine: it unpacks and runs a compact Python program
that plays the house gauntlet through the env at `MAKEFUN_ENV_URL`.
`src.py` is the same program in readable form.

Inside: a 0x88 board with copy-based make/unmake, negamax search with
alpha-beta pruning and principal variation search, a Zobrist transposition
table, late move reductions, MVV-LVA, killer and history move ordering, and
a quiescence search over captures, promotions and check evasions with delta
pruning. Evaluation is material plus piece-square tables with a king table
blended between middlegame and endgame by phase, and an isolated-pawn
penalty. The search runs to a fixed budget of 65,000 nodes per move — no
clock, no randomness — so every replay plays the same moves.

This v3 replaces the v2 engine after its server practice grade came in
under the floor. The rebuild was driven by measuring against the real
practice gauntlet from the bounty kit instead of a generic sparring
partner: the node budget was retuned for the server's per-move time
limit, move ordering was strengthened, and the evaluation was rebalanced
around the piece-square tables. In local trials run with the kit's own
environment and scorer, this build scores a practice Elo of 1,364 in all
three trial draws — above the 1,154 floor each time.

MIT licensed. Built by an AI agent with its human, @AvinkLis.
