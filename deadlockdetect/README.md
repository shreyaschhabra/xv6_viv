# Lab 6 · Q2 — Deadlock Detection (Resource Allocation / Wait-For Graph)

## What the question asks
Given processes and resources with allocation/request edges, build the graph and **detect a cycle** using DFS; report which processes are deadlocked.

## Concepts
- **Resource Allocation Graph (RAG):** nodes = processes (circles) and resources (squares). Edge **R→P** = allocated; **P→R** = requested.
- **Wait-For Graph (WFG):** collapse resources: edge **Pi → Pj** if Pi waits for a resource held by Pj.
- **Cycle test:** with **single-instance** resources, **cycle ⇔ deadlock**. With multiple instances a cycle is necessary but not sufficient (use a detection algorithm similar to Banker's with *Request* instead of *Need*).
- **DFS cycle detection:** three colours — white (unvisited), grey (on current path), black (done). Meeting a **grey** node = back edge = cycle. Complexity O(V+E).
- **Recovery options:** terminate all/one-by-one deadlocked processes, or preempt resources (rollback), choosing a victim by cost to avoid starvation.
- **Detection vs avoidance:** detection lets deadlock happen then fixes it (run periodically); avoidance (Banker's) prevents it.

## Viva points
- **Four necessary conditions?** Mutual exclusion, hold & wait, no preemption, circular wait (all must hold).
- **When to run detection?** Periodically or when CPU utilization drops.
- **Why can DFS give a cycle path?** Keep a parent/stack array to print the cycle.
