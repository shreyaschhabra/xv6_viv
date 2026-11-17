# Lab 6 · Q3 — Deadlock Prevention by Resource Ordering

## What the question asks
Give each resource a number; every process must request resources **only in increasing order**. Show that this prevents deadlock (compare with an un-ordered run that deadlocks).

## Concepts
- **Prevention** = make sure at least one of the 4 conditions can never hold. Resource ordering **breaks circular wait**.
- **Why it works:** a cycle P1→P2→…→P1 would need some process to wait for a *lower*-numbered resource than one it holds — impossible when everyone climbs upward. A total order has no cycles.
- **Example:** two locks A(1), B(2). Thread 1 locks A then B; Thread 2 also locks A then B (not B then A) → no deadlock.
- **Lock hierarchy** is the same idea in practice (used in the Linux kernel's locking rules, e.g. xv6 documents lock order).
- **Costs:** programmers must know all resources in advance; may force holding resources longer than necessary → lower utilization.
- Other prevention ideas: request all resources at once (breaks hold & wait), allow preemption (breaks no-preemption).

## Viva points
- **Which condition is broken?** Circular wait.
- **Prevention vs avoidance vs detection?** Prevention: design rules; avoidance: dynamic safe-state checks; detection: find and recover.
- **What if a process needs a lower resource after a higher one?** It must release the higher ones first and re-acquire in order.
