# Lab 7 · Q3 — Deadlock Detection + Banker's in xv6 (kernel-level)

## What the question asks
Add syscalls to the xv6 kernel to **record resource allocation/requests** of processes, **detect deadlock** (graph/matrix method) and use **Banker's algorithm** to decide safe requests.

## Concepts
- See `bankers/` and `deadlockdetect/` (Lab 6) for the algorithms; here they live **inside the kernel** with tables in kernel memory protected by a lock.
- **Detection algorithm (matrices):** `Work=Available`; find unfinished i with `Request[i] ≤ Work`; assume it finishes (`Work += Allocation[i]`); processes never finishable at the end = **deadlocked**.
- **Avoidance (Banker's):** grant only if resulting state is safe.
- **Kernel concerns:** syscall argument fetching (`argint`), locks on tables, user↔kernel copy (`copyout`) to return results, keeping tables updated on process exit (release its resources).

## Viva points
- **Detection vs avoidance cost/benefit?** Detection: no advance info, lets deadlock occur; avoidance: needs Max claims, more conservative.
- **What to do after detecting deadlock?** Kill a victim / preempt resources.
- **Why must exit release held resources?** Otherwise the tables leak and falsely show deadlock.
