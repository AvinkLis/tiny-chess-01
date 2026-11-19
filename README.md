[![make.fun tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/AvinkLis/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/AvinkLis/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01)

# tiny-chess-01 — a 4 KB Python chess engine

A chess engine in pure-stdlib Python, packed into a single `main.py` of at
most 4,096 bytes. It plays the make.fun tiny-chess-01 gauntlet through the
env at `MAKEFUN_ENV_URL`: it asks for the game in play with `POST /game`,
parses the FEN it is served, and answers with its move via `POST /move`.

## How it plays

- **Board:** 0x88 representation with full legal move generation —
  castling, en passant and promotions included (perft-verified).
- **Search:** negamax alpha-beta with iterative deepening from depth 1,
  under a fixed budget of 65,000 nodes per move (search and quiescence
  counted together, stopped by an exception). No wall clock, no runtime
  randomness, so replays match.
- **Transposition table** keyed by Zobrist hashes from a fixed-seed PRNG.
- **Move ordering:** TT move first, then captures by MVV-LVA and the
  history heuristic.
- **Reductions and pruning:** late move reduction for quiet moves at
  depth ≥ 3 with re-search on a promising score, null-move pruning and
  check extensions. A position repeated inside the search scores as a
  draw.
- **Quiescence:** captures, promotions and check evasions, with delta
  pruning; a side to move that is in check with no legal move scores as
  mate by distance.
- **Evaluation:** material (pawn 100, knight 320, bishop 330, rook 500,
  queen 1000) plus explicit piece-square tables for every piece,
  mirrored for Black. The king table is blended between middlegame and
  endgame by game phase (phase weights knight 1, bishop 1, rook 2,
  queen 4, maximum 24). Isolated pawns cost 10, doubled pawns 15, the
  bishop pair earns 30, and a king on its home ranks scores a small
  pawn-shield bonus.

## Practice results

Local practice trials (kit v27, 6 games each against depth-2 through
nodes-500k):

| Trial | Elo | W | D | L |
|-------|-----|---|---|---|
| 0 | 1545 | 4 | 1 | 1 |
| 1 | 1451 | 2 | 4 | 0 |
| 2 | 1451 | 3 | 2 | 1 |

Mean practice Elo **1482**, lowest trial 1451 — above the 1,154 floor
for the final.

## Final build notes

The committed `main.py` is the compressed form of the engine source,
produced with the kit's own `makefun compress`; it stays within the
4,096-byte cap and is the only file the engine runs. The scores above
are the final source's own practice runs: every trial finished inside
its wall-time budget, and the engine's slowest move stayed well under
the 10-second move limit, so the same moves come out on a replay.
