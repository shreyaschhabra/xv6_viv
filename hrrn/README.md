# Lab 4 · Q2 — HRRN (Highest Response Ratio Next, non-preemptive)

## What the question asks
Add `ctime` and `burst_time`; `set_burst()`. Scheduler computes for each RUNNABLE: **RR = ((wait + burst) × 100) / burst** with `wait = ticks − ctime`; picks the **highest**, runs it to completion.

## Concepts
- **Response ratio** = (W + S) / S. Starts at 1 for a new process and grows as it waits. Short jobs have a high ratio quickly; long jobs' ratios keep **rising while waiting** → **no starvation**.
- It is a compromise between **FCFS** (fair, ignores length) and **SJF** (favors short, starves long).
- Non-preemptive; needs burst estimates.
- ×100 trick: kernels avoid floating point, so scale integers to keep precision.

## Example (t = 10)
A: W=8, S=2 → (10×100)/2 = 500 · B: W=9, S=20 → (29×100)/20 = 145 → A runs first. After B has waited much longer its ratio will overtake newer short jobs.

## Viva points
- **Why no starvation?** W increases while waiting, so the ratio increases without bound.
- **Cost?** Computing ratios for every ready process at each decision (O(n)).
- **Compared with aging?** HRRN's ratio is a built-in aging mechanism.
