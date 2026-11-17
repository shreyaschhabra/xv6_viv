# Lab 10 · Q1 — `memstats` : per-process memory statistics

## What the question asks
Add a syscall returning stats for a process: total virtual size, number of **resident (mapped) pages**, number of page faults, etc., and a user program printing them.

## Concepts
- **Virtual size** (`p->sz`) vs **resident set size (RSS)** = pages actually mapped in physical memory. With lazy allocation RSS < virtual size.
- Count RSS by **walking the page table** and counting valid user PTEs (`PTE_V && PTE_U`).
- **Working set:** pages a process actively uses in a time window; basis for deciding frame allocation and detecting **thrashing** (more time paging than executing).
- Stats are stored in `struct proc` and copied to user with `copyout`.

## Viva points
- **Virtual vs resident vs physical?**
- **What is thrashing and how avoided?** Working-set model, page-fault-frequency control.
- **Why not count `sz/4096`?** Not every virtual page is mapped (lazy, guard page).
