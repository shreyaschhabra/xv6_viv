# Lab 3 · Q2 — Priority Scheduler with Aging (preemptive)

## What the question asks
Add `priority` (1 = highest … 20 = lowest, default 10) to `struct proc`; syscall `set_priority(pid, prio)`; scheduler always runs the RUNNABLE process with the **lowest number**; ties → round-robin. **Aging:** if a RUNNABLE process waits > 30 ticks, decrement its number by 1 (boost) and reset its wait counter.

## Concepts
- **Priority scheduling:** each process has a priority; CPU goes to the highest. **Preemptive** → if a higher-priority process becomes runnable, the running one is preempted at the next timer tick.
- **Starvation (indefinite blocking):** low-priority processes may never run if high-priority ones keep arriving.
- **Aging:** gradually increases the priority of processes that wait long → guarantees eventual execution. Here: every 30 ticks of waiting, priority number −1; counter reset.
- **Tie-break (round-robin):** among equal priorities, rotate so none monopolizes.
- **System call path:** user `set_priority()` → `usys.S` stub → `ecall` → `syscall()` table → `sys_set_priority()` → modifies `p->priority` (under `p->lock`).
- **Priority inversion** (bonus): a high-priority task waits on a lock held by a low-priority task.

## Algorithm
```
each timer tick: for every RUNNABLE p: p->wait++ ; if p->wait>30 && p->priority>1 { p->priority--; p->wait=0 }
scheduler: pick RUNNABLE with min(priority); on tie take next in round-robin order
```
## Expected result
High-priority finish first, but the priority-20 process still completes thanks to aging (printed wait/priority changes).

## Viva points
- **Starvation vs deadlock?** Starvation: process ready but never chosen; deadlock: processes wait for each other forever.
- **Why reset the wait counter after boosting?** So the process needs another 30 ticks before the next boost (gradual rise).
- **Preemptive vs non-preemptive priority?** Preemptive switches immediately when a higher one arrives.
