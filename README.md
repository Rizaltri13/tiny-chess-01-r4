# tiny-chess-01 — round 4

A chess engine in 4 KB of Python for the make.fun bounty
[tiny-chess-01](https://make.fun/bounty/tiny-chess-01) ("Build the strongest
chess engine in 4 KB of Python").

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/Rizaltri13/tiny-chess-01-r4/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/Rizaltri13/tiny-chess-01-r4/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

## Design

`main.py` (4,044 bytes) is a self-unpacking LZMA/base85 payload. Inside is a
complete negamax engine:

- **Search.** Alpha-beta with iterative deepening under a fixed
  20,000-node budget (fully deterministic: no wall clock, no randomness),
  principal-variation search, late-move reductions, null-move pruning,
  check extensions, and a transposition table with mate-score adjustment.
- **Quiescence.** Captures and promotions, plus full check evasions, so the
  horizon never stands pat while in check.
- **Move ordering.** Transposition-table move, then MVV-LVA captures and
  promotions, then a history heuristic for quiet moves.
- **Evaluation.** Material (100/320/330/500/900) plus CPW piece-square
  tables stored as a compact base85 blob, with a tapered endgame king table
  blended in by game phase. In-search repetition and fifty-move positions
  score as draws, mirroring the house rules.
- **Board.** 0x88 bytearray board with an exact legality filter (castling
  transit squares included), snapshot make/unmake.

The engine plays over the environment HTTP API (`MAKEFUN_ENV_URL`,
`POST /game`, `POST /move`) in UCI move notation.

## License

MIT — see `LICENSE`.
