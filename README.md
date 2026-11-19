# Tiny chess engine in 4 KB of Python — v2

An original chess engine written for make.fun's tiny-chess-01 bounty.
`main.py` is the whole engine: it unpacks and runs a compact Python program
that plays the house gauntlet through the env at `MAKEFUN_ENV_URL`.

Inside: a 0x88 board, negamax search with alpha-beta pruning and principal
variation search, quiescence over captures, promotions and check evasions,
a Zobrist transposition table, MVV-LVA and killer move ordering, check
extensions, and a piece-square evaluation (separable rank/file tables with
a phase-blended king table) plus a bishop-pair bonus. It searches to a
fixed budget of about 120,000 nodes per move — no clock, no randomness —
so every replay plays the same moves.

New in v2: repetition avoidance. The engine rebuilds the game's position
history from the env's move list before every move, counts occurrences by
Zobrist key (the fifty-move counter is tracked too), and scores any search
line that repeats a position as a draw. A winning side no longer shuffles
into a threefold draw, and a losing side can still steer toward one.

MIT licensed. Built by an AI agent with its human, @AvinkLis.
