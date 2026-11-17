# Lab 3 · Q1 — FCFS Scheduler (non-preemptive) in xv6

## What the question asks
Replace xv6's round-robin with **First-Come-First-Served**: add `ctime` (creation time, from global `ticks`) to `struct proc`, set in `allocproc()`. In `scheduler()` pick the RUNNABLE process with the **smallest ctime** and let it run until it finishes/sleeps. Test: 3 CPU-bound processes forked at staggered times; completion order = creation order.

## Concepts
- **CPU scheduler:** picks which ready process gets the CPU. Goals: throughput, turnaround, waiting, response time, fairness.
- **FCFS:** ready queue is a FIFO queue. **Non-preemptive** — once running, a process keeps the CPU until it exits or blocks (I/O/sleep). No timer-based switching.
- **`ticks`:** xv6's global timer counter incremented by the timer interrupt; used as the clock.
- **Convoy effect:** one long CPU-bound job at the front makes all short jobs wait → high average waiting time.
- In xv6, default round-robin is preemptive: the timer interrupt calls `yield()`. To make FCFS non-preemptive, the timer path must **not** yield (or yield is ignored for the running process).

## Algorithm in scheduler()
```
loop over proc table:
    among RUNNABLE, remember the one with minimum ctime
run it (swtch) ; it returns only when process exits/sleeps
```
## Example
Arrivals A(t=0, 10 ticks), B(t=1, 3), C(t=2, 2): order A,B,C. Waiting: A=0, B=9, C=11 → avg 6.67.

## Pros / Cons
✔ Simple, fair in arrival order, no starvation. ✘ Poor average waiting time, convoy effect, bad for interactive jobs.

## Viva points
- **Is FCFS preemptive?** No.
- **Can FCFS starve a process?** No — every process eventually reaches the front.
- **Why record ctime in allocproc()?** That function is where a new process is born in xv6.
