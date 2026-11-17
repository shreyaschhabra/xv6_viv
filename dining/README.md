# Lab 5 · Q4 — Dining Philosophers

## What the question asks
5 philosophers sit around a table with 5 forks (one between each pair). A philosopher needs **both** adjacent forks to eat. Implement it without deadlock.

## Concepts
- **Naive solution:** everyone picks left fork then right → if all pick left simultaneously, each waits forever for the right one → **deadlock** (circular wait).
- **Four deadlock conditions:** mutual exclusion, hold & wait, no preemption, **circular wait**. Break any one.
- **Common fixes:**
  1. **Limit to N−1 philosophers** at the table (semaphore `room = 4`) — circular wait impossible.
  2. **Resource ordering / asymmetry:** odd philosophers take left first, even take right first (or always lower-numbered fork first).
  3. **Pick both forks atomically** under one mutex (all-or-nothing) — breaks hold & wait.
  4. **Monitor** with states THINKING / HUNGRY / EATING and a `test(i)` function (eats only if neighbours are not eating).
- **Starvation** can still occur in some fixes; fair queueing handles it.
- Each fork = a semaphore initialized to 1.

## Viva points
- **What does the problem model?** Many processes competing for multiple shared resources.
- **Which condition does your solution break?** (Say yours: e.g. circular wait via ordering.)
- **Deadlock vs starvation?** Deadlock = nobody progresses; starvation = someone never gets a turn.
