# Lab 9 · Q2 — Lazy page-fault handler + fault counter (`getfaults`)

## What the question asks
Use lazy allocation; count the page faults handled per process in `struct proc` (`p->faults`) and expose via syscall `getfaults()`. Test: allocate N pages with `sbrk`, touch each → faults = N.

## Concepts
- **Page fault:** exception when the CPU accesses a virtual page with no valid mapping (or violating permissions).
- **Types:** *minor* (page just needs allocating/mapping — our case), *major* (must read from disk), *invalid* (illegal access → SIGSEGV/kill).
- **Handler steps:** identify cause (`scause` 13/15) → read `stval` → validate address in `[0, p->sz)` → allocate + zero + map → `p->faults++` → resume.
- **Count meaning:** faults ≈ number of pages first touched. Touching the same page again → no new fault.
- **Locality of reference:** programs touch few pages at a time → demand paging works well.
- **Trade-off:** fault handling costs time (trap, alloc), so many first touches are slower than one bulk allocation.

## Viva points
- **Is a page fault always an error?** No — it's the mechanism of demand paging.
- **How does the CPU restart the instruction?** `sepc` still points to the faulting instruction; return via `sret` re-executes it.
