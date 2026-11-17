# Lab 10 · Q2 — `vtop` : virtual to physical + page-table details

## What the question asks
A syscall `vtop(va)` that returns the physical address **and** its flags (valid, writable, user), by walking the 3-level page table; also show two processes' same VA map to different PAs.

## Concepts
- Translation = split VA → three 9-bit indices + 12-bit offset → walk → `PA = frame<<12 | offset`.
- **Separate address spaces:** the same VA (e.g. `0x1000`) maps to **different frames** in different processes → isolation. Shared pages (shm, trampoline) map to the **same** frame.
- Returns -1 if not mapped (or lazily unallocated).
- **Inverted/hashed page tables, huge pages** — alternatives to mention (2 MB megapages are possible in Sv39 by making a level-1 PTE a leaf).

## Viva points
- **What does the MMU do on invalid PTE?** Raises page-fault exception.
- **Why is the offset copied unchanged?** Page = frame size.
- **Difference from `va2pa` (Lab 8)?** Also reports permission flags.
