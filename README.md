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

## Usage

Requires Python 3, no extra packages.

```python
from nqueens import NQueensCSP
import time

solver = NQueensCSP(20)

start = time.time()
solution = solver.solve_mac()          # or solve_bt() / solve_fc()
elapsed = time.time() - start

print(solution)                        # {column: row, ...}
print("constraint checks:", solver.constraint_checks)
print(f"time: {elapsed:.3f} s")
```

## Team

Team project, built with a classmate.mbined performance-summary table for the evaluated algorithms and board sizes.
