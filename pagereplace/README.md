# Lab 10 · Q4 — Page Replacement: FIFO, LRU and Optimal

## What the question asks
Implement and compare **FIFO, LRU and Optimal** on a reference string for 3, 4, 5 frames; print faults and hit ratio.

## Concepts (builds on `pagerepl/`)
- **Optimal (OPT / MIN):** evict the page that will be **used farthest in the future** (or never). **Lowest possible faults**, but impossible to implement online → benchmark.
- **Results for `7 0 1 2 0 3 0 4 2 3 0 3 2 1 2 0 1 7 0 1` (20 refs):**

| Frames | FIFO | LRU | OPT |
|---|---|---|---|
| 3 | 15 | 12 | 9 |
| 4 | 10 | 8 | 8 |
| 5 | 9 | 7 | 7 |

- More frames → fewer faults (for LRU/OPT always; FIFO can break this → **Belady's anomaly**, `1 2 3 4 1 2 5 1 2 3 4 5`: 9 faults at 3 frames, 10 at 4).
- **FIFO** simple but ignores usage; **LRU** near-optimal using locality, costlier; **OPT** theoretical lower bound.
- **Compulsory (cold) faults:** the first access to each distinct page always faults (here 7 distinct pages ⇒ minimum 7).

## Viva points
- **Which algorithm do real OSes use?** LRU approximations (clock, aging).
- **Why does OPT matter if unusable?** Measures how good others are.
- **Fault rate vs frames graph:** decreases, flattening once the working set fits.
