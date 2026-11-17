# Lab 4 · Q1 — SRTF (preemptive SJF)

## What the question asks
Add `burst_time` and `remaining_time`; `set_burst()` sets both. Scheduler picks smallest `remaining_time`. On every timer interrupt (`trap()`), decrement the running process's remaining time; if another RUNNABLE has a **strictly smaller** remaining time → `yield()`. Test: staggered arrivals, a short newcomer preempts a long runner.

## Concepts
- **SRTF = preemptive SJF.** Always runs the job closest to finishing.
- **Preemption** is driven by the **timer interrupt**: hardware tick → trap handler → decide to `yield()` → scheduler picks again.
- **Remaining time** = burst − time already executed.
- **Strictly smaller** avoids pointless context switches on equal values.
- Gives the **minimum average waiting time** overall, but has **more context-switch overhead** and **starves long jobs**.

## Example
P1 arrives t=0 burst 8; P2 arrives t=1 burst 4. At t=1 P1 has 7 left > 4 → P2 preempts. P2 runs 1–5, then P1 resumes.

## Viva points
- **SJF vs SRTF?** SJF waits for the running job to end; SRTF preempts.
- **Why is strict `<` used?** Prevents thrashing between equal jobs.
- **Disadvantage?** Starvation, requires accurate burst prediction, frequent switches.
