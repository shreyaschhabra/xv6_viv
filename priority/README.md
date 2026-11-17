# Lab 7 · Q4 — Priority Scheduling with Aging (revisited)

## What the question asks
Implement a priority scheduler with **aging** and show that low-priority processes eventually run; demonstrate the **starvation** without aging.

## Concepts
- Same theory as `priority_aging/` (Lab 3): preemptive priority, lower number = higher priority, ties round-robin.
- **Starvation:** low-priority process never scheduled while high-priority ones keep coming.
- **Aging:** the longer a process waits in RUNNABLE state, the more its priority improves; once it runs, its wait counter resets (priority may be restored).
- **Experiment design:** run with aging off (low-priority job finishes last/never) vs on (finishes in bounded time); print priorities over time.
- **Priority inversion & priority inheritance** (extra): a low-priority task holding a lock temporarily inherits the waiter's priority (Mars Pathfinder bug).

## Viva points
- **Why aging fixes starvation?** Priority eventually reaches the top.
- **Trade-off:** aging slightly reduces strict priority guarantees.
- **Static vs dynamic priority?**
