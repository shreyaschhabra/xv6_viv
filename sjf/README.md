# Lab 3 · Q3 — SJF Scheduler (non-preemptive) with declared burst time

## What the question asks
Add `burst_time` to `struct proc`; syscall `set_burst(pid, bt)`; scheduler picks the RUNNABLE process with the **smallest burst_time** and runs it to completion. Processes with burst 0/-1 (not set) are scheduled **last**.

## Concepts
- **CPU burst:** the length of time a process runs before it blocks/finishes.
- **SJF:** shortest next burst first. **Provably optimal** for minimum *average waiting time* (among non-preemptive algorithms).
- **Problem:** the OS can't know the future burst. Real systems **estimate** with exponential averaging: `τ(n+1) = α·t(n) + (1−α)·τ(n)`. Here the process itself declares it via `set_burst()`.
- **Starvation:** a long job may wait forever if short jobs keep arriving.
- **Non-preemptive:** a running job is never interrupted by a shorter new arrival (that's SRTF, next lab).

## Example
Bursts P1=6, P2=8, P3=7, P4=3 (all at t=0): order P4,P1,P3,P2 → waits 0,3,9,16 → avg 7 (FCFS would give 10.25).

## Viva points
- **Why optimal?** Moving a short job ahead of a long one reduces the short job's wait more than it increases the long job's.
- **Drawbacks?** Needs burst knowledge; starvation of long jobs.
- **Why treat unset burst as last?** Unknown/zero would otherwise look "shortest" and wrongly win.
- **FCFS vs SJF vs Priority** comparison: FCFS = arrival, SJF = shortest burst, Priority = importance (SJF is priority = 1/burst).
