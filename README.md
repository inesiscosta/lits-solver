# LITS (Nuruomino) Solver

A solver for **LITS** (ヌルオミノ, *Nuruomino*), Nikoli's tetromino logic puzzle, modelled as a constraint satisfaction problem and solved with depth-first search, the minimum-remaining-values heuristic and forward checking.

Built for the Artificial Intelligence course at Instituto Superior Técnico (2024/2025). There's an [interactive write-up](https://inesiscosta.com/writing/nuruomino) where you can watch it solve puzzles step by step.

## The Puzzle

The grid is split into regions. Shade exactly four cells in every region so that:

1. **One tetromino per region.** The shaded cells of each region form an L, I, T or S piece.
2. **No matching neighbours.** Two pieces of the same shape can't share an edge.
3. **One connected area.** All shaded cells must be connected orthogonally.
4. **No 2×2 blocks.** No 2×2 square can be fully shaded.

## How it Works

The puzzle is a search problem (`Nuruomino`, a subclass of the course's `search.Problem`). A state is a partly filled board plus the bookkeeping needed to prune it.

- **Precomputed domains.** Every orientation of every tetromino is tried at every position. Placements that fit entirely inside a region are kept, giving each region its list of candidate actions.
- **Variable ordering (MRV).** The next region to fill is the one with the fewest remaining valid placements, so dead ends show up early.
- **Forward checking.** After each placement, the candidate lists of the neighbouring regions are filtered with `Action.is_currently_valid`. Placements that would make a 2×2 block, touch a piece of the same shape, or cut a region off from the rest are dropped.
- **Connectivity.** The solver keeps a region adjacency graph that is updated as pieces go down. A state whose graph is no longer connected is a dead end and is abandoned right away.
- **Search.** `depth_first_tree_search` from `search.py`. The goal is reached when every region is filled and the graph is still connected.

### Interactive Write-Up/Demo

For a deeper walkthrough, see **[inesiscosta.com/writing/nuruomino](https://inesiscosta.com/writing/nuruomino)**. Along with explaining the approach in more detail, it lets you:

- **Play the puzzle**, with the rules checked as you go.
- **Watch this exact code run** in your browser through Pyodide, line by line.
- **See the data structures change** at every step, including the board, the adjacency graph and each region's remaining placements.

## Usage

Requires Python 3.9+ and NumPy.

```bash
pip install -r requirements.txt
python3 src/nuruomino.py < tests/test01.txt
```

Input is a square grid of region ids, separated by tabs. Output is the same grid with each piece's cells replaced by its letter:

```
Input                   Output
1  1  2  2  3  3        L  L  S  2  3  3
1  2  2  2  3  3        L  2  S  S  3  3
1  3  3  2  3  5        L  3  3  S  3  I
3  3  3  3  3  5        3  3  3  T  3  I
4  4  4  3  3  5        L  L  L  T  T  I
4  3  3  3  3  5        L  3  3  T  3  I
```

## Tests

```bash
python3 test_nuruomino.py
```

This runs the solver on every `tests/testNN.txt` and compares the result with `tests/testNN.out`. All 13 cases pass.
