# Lab 5 · Q1 — Peterson's Algorithm (mutual exclusion for 2 processes)

## What the question asks
Two xv6 processes (parent/child) update a **shared counter** that lives in a shared memory page. Without protection the final count is wrong (race condition). Protect the critical section with **Peterson's algorithm**, using a shared `flag[2]` and `turn`.

## Concepts
- **Race condition:** result depends on the order of interleaved operations. `counter++` is really *load → add → store*; two processes can load the same old value and one update is lost.
- **Critical section (CS):** code touching shared data. Requirements: **mutual exclusion**, **progress**, **bounded waiting**.
- **Peterson's algorithm** (software-only, 2 processes i and j):
```
flag[i] = 1;          // I want to enter
turn = j;             // but you go first if you also want
while (flag[j] && turn == j) ;   // busy-wait
   ... critical section ...
flag[i] = 0;          // exit
```
- Why it works: if both want in, `turn` can hold only one value (whoever wrote it last yields), so exactly one proceeds.
- **Shared memory in xv6:** processes have separate address spaces, so a page is mapped into both (kernel shared-page syscall like `shm_get`) — fork alone gives each process a *copy*.
- **Busy waiting (spinlock-like):** wastes CPU.
- **Memory ordering caveat:** modern CPUs/compilers can reorder writes; real code needs a memory barrier (`__sync_synchronize()`) and `volatile` so Peterson works.

## Viva points
- **Does it satisfy all 3 requirements?** Yes (ME, progress, bounded waiting – at most one turn).
- **Limitations?** Only 2 processes; busy waiting; needs memory barriers on modern hardware.
- **Why not just use a normal variable as a lock?** Test-and-set would not be atomic → both may enter.
- **Hardware alternative?** Atomic instructions: test-and-set, compare-and-swap (`amoswap` on RISC-V).
