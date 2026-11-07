# N-Queens CSP Solver

Solves the **N-Queens problem** (place N queens on an N×N board so that no two attack each other) as a **constraint satisfaction problem (CSP)**, and compares three search algorithms by running time and number of constraint checks. Built for a university Artificial Intelligence course at Imam Mohammad Ibn Saud Islamic University.

## How the problem is modeled

- **Variables:** one per column (`0 … N-1`)
- **Domains:** the rows a queen in that column can take
- **Constraints:** no two queens share a row, and no two queens share a diagonal (`|row₁ − row₂| ≠ |col₁ − col₂|`)

## Algorithms

| Algorithm | Method | Variable order |
|---|---|---|
| **Backtracking (BT)** | `solve_bt()` checks each new queen against those already placed | static, column by column |
| **Forward Checking (FC)** | `solve_fc()` removes conflicting rows from future columns after every placement and backtracks as soon as a domain becomes empty | **MRV** heuristic |
| **Maintaining Arc Consistency (MAC)** | `solve_mac()` runs **AC-3** after every placement to keep all remaining columns arc-consistent | **MRV** heuristic |

- **MRV (Minimum Remaining Values):** picks the column with the fewest valid rows left, with deterministic tie-breaking.
- **Random value order:** row values are shuffled, so repeated runs explore different parts of the search space.
- **Measurements:** each solver counts its constraint checks, and a time limit of 20 minutes stops very long runs.

## How to run

Requires Python 3, no extra packages.

```bash
python main.py
```

`main.py` runs all three algorithms on boards of size **4, 8, 16, 32 and 64**. For each run it prints the time, the number of constraint checks and the solution, draws the board when N ≤ 32, and ends with a summary table. Plain backtracking is skipped for N ≥ 20 because it would take too long.

Example board (N = 4):

```
 □  ♕  □  □
 □  □  □  ♕
 ♕  □  □  □
 □  □  ♕  □
```

## Sample results

From one sample run of `main.py`. Times and checks change from run to run because the row order is random.

| N | BT checks | FC checks | MAC checks | FC time | MAC time |
|---|---|---|---|---|---|
| 8 | 453 | 28 | 658 | 0.0001 s | 0.0005 s |
| 16 | 147 | 139 | 4,154 | 0.0007 s | 0.0028 s |
| 32 | skipped | 931 | 33,735 | 0.010 s | 0.036 s |
| 64 | skipped | 2,016 | 250,535 | 0.025 s | 0.521 s |

- **FC with MRV** was the fastest and solved N = 64 in a fraction of a second.
- **MAC** prunes the most, but AC-3 checks every pair of remaining columns after each move, so it makes many more constraint checks.
- **Plain BT** is fine for small boards but grows too slowly to use on large ones.

## Files

| File | Contents |
|---|---|
| [`nqueens.py`](nqueens.py) | the `NQueensCSP` class: BT, FC, MAC, AC-3 and MRV |
| [`main.py`](main.py) | runs the experiments, prints boards and the summary table |

## Team

Team project, built with a classmate.
