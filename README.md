# NuruominoSolver

## Overview

**NuruominoSolver** is an AI-powered tool to solve Nuruomino (LITS) puzzles. Developed for an Artificial Intelligence course, it models the puzzle as a constraint satisfaction problem.

---

## Problem Description

Nuruomino (LITS) is a logic puzzle played on a grid divided into regions. The objective is to fill each region with one tetromino (L, I, T, or S shape), following these rules:

- **Each region** must be filled with a tetromino.
- **No two identical tetrominoes** can be orthogonally adjacent.
- **All filled cells** must form a single connected area.
- **No 2×2 block** can be fully filled.

---

## Solution Approach

- **Constraint Satisfaction:** The solver uses backtracking and constraint propagation to efficiently assign tetrominoes to regions, pruning invalid configurations early.
- **Connectivity Checks:** Ensures all filled cells are connected and no forbidden 2×2 blocks are formed.

---
