# Lab 4 · Q3 — Multilevel Feedback Queue (MLFQ), 3 levels

## What the question asks
Add `queue_level` (0 highest–2) and `ticks_used`. New processes start at 0. Scheduler takes from the **highest non-empty queue** (RR inside a queue). Quantum: level 0 = 1 tick, level 1 = 2, level 2 = 4. If a process uses its **whole quantum without yielding** → demote (level++) and `yield()`. Test with CPU-bound (infinite loop) vs I/O-bound (frequent `sleep`) processes.

## Concepts
- **MLFQ:** several queues with different priorities; processes **move between queues** based on behavior (feedback). Needs no prior burst knowledge.
- **Rules:** (1) higher queue runs first; (2) same level → round-robin; (3) new job → top; (4) uses full quantum → demoted; (5) gives up CPU early (I/O) → stays.
- **Why it works:** I/O-bound/interactive jobs use little CPU per burst → remain high → good response time. CPU hogs sink → don't hurt interactive ones. It approximates SJF without knowing the future.
- **Increasing quantum for lower queues:** CPU-bound jobs get longer slices (fewer context switches).
- **Starvation fix (periodic boost):** every S ticks move all jobs to the top queue (your repo includes this). Also stops gaming (a job that yields just before its slice ends).
- **Reset `ticks_used`** whenever the process gets a new quantum/changes level.

## Viva points
- **Why demote?** Long CPU use signals a batch job.
- **How does MLFQ handle starvation?** Priority boost.
- **Parameters to tune?** Number of queues, quantum per level, boost period, promotion/demotion rules.
- **Real-world use:** Solaris, Windows, BSD, older Linux variants.
