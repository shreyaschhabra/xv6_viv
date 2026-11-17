# Lab 6 · Q4 — Synchronization + Deadlock Avoidance combined

## What the question asks
Combine a synchronization mechanism (locks/semaphores protecting shared resources) with deadlock avoidance (Banker's check before granting). Processes request resources through a controlled "manager"; the manager grants only if the state remains safe, otherwise the process waits.

## Concepts
- **Two separate problems:** *synchronization* keeps shared data consistent (mutual exclusion); *deadlock avoidance* keeps processes from waiting forever.
- **Design:** a **resource manager** with a lock protecting the Available/Allocation/Need tables. Request → take lock → run Banker's check → grant (update tables) or block (`sleep`) → release lock. On release of resources → `wakeup` waiters to re-check.
- **Why the tables need a lock:** two concurrent requests could each see "safe" individually but together be unsafe (check-then-act race).
- **Sleep/wakeup (xv6):** a blocked process sleeps on a channel and is woken when resources are freed; it must re-test its request in a loop.
- Result: no data races, no deadlock; possible waiting (reduced concurrency) is the trade-off.

## Viva points
- **Why is the safety check done under the lock?** To keep the check and the allocation atomic.
- **Can a safe-state check still block a process forever?** Possibly (starvation); mitigate with FIFO queuing.
- **Difference between lock and semaphore** (mutual exclusion vs counting/signalling).
