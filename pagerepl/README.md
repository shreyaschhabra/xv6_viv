# Lab 9 · Q3 — Page Replacement: FIFO vs LRU

## What the question asks
Simulate a fixed number of frames (3) with a reference string; count page faults for **FIFO** and **LRU**. Expected for `7 0 1 2 0 3 0 4 2 3 0 3 2 1 2 0 1 7 0 1`: **FIFO = 15, LRU = 12**.

## Concepts
- **Why replacement:** RAM frames are limited; when a fault occurs and no free frame exists, pick a **victim** page to evict.
- **FIFO:** evict the **oldest loaded** page (queue). Simple; may evict a hot page; suffers **Belady's anomaly** (more frames can cause more faults — string `1 2 3 4 1 2 5 1 2 3 4 5`: 9 faults with 3 frames, 10 with 4).
- **LRU:** evict the page **unused for the longest time** (uses past as predictor of future by locality). Needs a timestamp or stack/list updated on **every reference**; it's a *stack algorithm* → no Belady's anomaly.
- **Hit ratio** = 1 − faults / references. FIFO: 5/20 = 25 % hits; LRU: 8/20 = 40 %.
- **Hardware approximations:** reference bit / **clock (second-chance)**, aging.

## Mini trace (3 frames, first refs): 7,0,1 → faults(3); 2 → evicts 7 (FIFO) ...

## Viva points
- **Why not Optimal in practice?** Needs future knowledge; used as a benchmark.
- **What is Belady's anomaly?** FIFO fault count can rise when frames increase.
- **Cost of exact LRU?** Updating order on each memory access — too expensive in hardware.
