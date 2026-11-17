# Lab 9 · Q1 — Page-table printing / `pttest`

## What the question asks
Implement a syscall that prints a process's page-table entries (valid pages, physical frame, flags) and a test (`pttest`) that allocates memory and shows how the table changes.

## Concepts
- Same translation theory as `va2pa/` + `vmprint/`: 3-level Sv39 tree, 512 entries/level, PTE = frame number + flags.
- **Demonstration idea:** print table → `sbrk`/`malloc` more memory → print again → **new leaf PTEs appear**; with lazy allocation they appear only after touching.
- **Interpreting flags:** `PTE_U` set → user page; no `PTE_U` → kernel/guard page; `PTE_W` clear → read-only (text); `PTE_X` → code.
- **Page table cost:** each level-table is one 4 KB page; sparse regions need no tables.

## Viva points
- **How many memory accesses for one translation without TLB?** 3 table reads + 1 data access (4 on Sv39).
- **What is a TLB hit?** Translation cached → no table walk.
