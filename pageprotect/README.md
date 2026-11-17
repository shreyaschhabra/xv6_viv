# Lab 10 · Q3 — Page protection : `pageinfo` and `setreadonly`

## What the question asks
`pageinfo(va)` returns the page's flags; `setreadonly(va)` clears `PTE_W` so a later write **faults and the process is killed**. Demonstrate that a write after `setreadonly` crashes.

## Concepts
- **Protection bits** in PTE: R, W, X, U — hardware enforces them on every access.
- **`setreadonly`:** find PTE via `walk()`, `*pte &= ~PTE_W`, then flush TLB (`sfence.vma`) so the old writable translation isn't cached.
- **Write to read-only page** → store page fault (`scause 15`) → kernel kills process ("usertrap: unexpected scause").
- **Uses:** code pages read-only, **copy-on-write** (fork shares pages read-only; the first write faults, kernel copies the page), guard pages, `mprotect()` in Linux.
- **Principle:** least privilege → bugs/attacks can't corrupt code or read-only data.

## Viva points
- **Why flush the TLB?** Stale entry would still allow writes.
- **How does COW use this?** Mark shared pages read-only; on write-fault, copy and make writable.
- **Difference between a protection fault and a not-present fault?** PTE valid but permission wrong vs PTE invalid.
