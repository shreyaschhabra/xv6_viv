# Lab 8 · Q4 — Lazy Allocation (lazy `sbrk`)

## What the question asks
Change `sbrk(n)` so it **only increases `p->sz`** without allocating memory; allocate a physical page **when the process first touches it** (page-fault handler).

## Concepts
- **Eager vs lazy allocation:** eager allocates and zeroes pages at `sbrk` time (wasteful if never used); lazy defers until the first access → faster startup, saves memory for sparse big arrays.
- **Page fault flow (RISC-V):** access to unmapped page → trap with `scause = 13 (load) / 15 (store)` and faulting address in **`stval`** → in `usertrap()`: check `va < p->sz` (and above the stack guard) → `kalloc()` a page, `memset` to 0, `mappages(va rounded down, PTE_R|W|U)` → return and **retry the instruction**. Otherwise kill the process.
- **Must also fix:** `uvmunmap` (skip unmapped pages), `uvmcopy` for fork (skip pages not present), and syscalls that use user pointers (`copyin/copyout`/`walkaddr`) to allocate on demand.
- **Negative `sbrk`:** shrink eagerly.
- Same idea as **demand paging**.

## Viva points
- **Why zero the page?** Security (no leftover data) and C expects zeroed bss.
- **Which register holds the faulting address?** `stval` (CR2 on x86).
- **Out of memory in handler?** Kill the process.
