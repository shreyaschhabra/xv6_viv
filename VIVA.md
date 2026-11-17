# VIVA Quick Revision — OS Lab (Labs 1–10)

## Lab 1 · Shell scripting
- Shebang `#!/bin/bash` picks the interpreter. `$#` = arg count, `$1..` = args, `$?` = last exit status.
- `[ ]` is POSIX test; `[[ ]]` is the bash keyword (supports `&&`, `||`, regex). `-eq` is numeric, `==` is string.
- Leap year: ÷4 and (not ÷100 or ÷400). Prime: test divisors up to √n.
- A function `return`s only a status 0–255. To return text, `echo` it and capture it with `$(...)`.

## Lab 2 · Processes
- `fork()` returns 0 in the child, the child's PID in the parent, and -1 on error. Output order is non-deterministic unless the parent calls `wait()`.
- Zombie = exited child not yet reaped. Orphan = parent exited first (adopted by init).
- `exec` replaces the process image and keeps the PID. It returns only on error.
- exec letters: l = list, v = vector, p = PATH search, e = environment.
- Shell = read → fork → exec → wait.

## Lab 3 · Basic scheduling
- FCFS: non-preemptive, FIFO order. Convoy effect.
- Priority: preemptive. Starvation is fixed by aging.
- SJF: optimal average waiting time among non-preemptive algorithms. Needs burst knowledge. Long jobs can starve.

## Lab 4 · Advanced scheduling
- SRTF = preemptive SJF. Switch only when the remaining time is strictly smaller.
- HRRN: ratio = (W+S)/S. It grows while a job waits, so there is no starvation.
- MLFQ: new jobs start at the top queue. Using a full quantum demotes the job. Quanta are 1/2/4. A periodic boost prevents starvation.

## Lab 5 · Synchronization
- Race condition: `counter++` is load → add → store.
- Critical section needs mutual exclusion, progress and bounded waiting.
- Peterson: `flag[i]=1; turn=j; while(flag[j] && turn==j);`. Works for 2 processes only. Needs memory barriers on modern CPUs.
- Producer–consumer semaphores: mutex=1, empty=N, full=0. Taking mutex before empty/full can deadlock.
- Readers–writers: reader preference starves writers.
- Dining philosophers: the naive solution has circular wait. Fixes: N−1 at the table, ordering, atomic pickup, or a monitor.

## Lab 6 · Deadlock
- Four conditions: mutual exclusion, hold and wait, no preemption, circular wait.
- Safe state ⇒ no deadlock. Unsafe ≠ deadlock. Deadlock ⊂ unsafe.
- Banker's: Need = Max − Allocation. Safety algorithm with Work/Finish. For a request: pretend to allocate, check safety, roll back if unsafe.
- Detection: wait-for graph, DFS (grey node = cycle). A cycle means deadlock only with single-instance resources.
- Prevention by resource ordering breaks circular wait.
- Prevention vs avoidance vs detection: rules in advance / safe-state check at each request / find and recover.

## Lab 7 · Threads, semaphores
- Threads share code, data, heap and files. Each has its own stack and registers.
- `clone` shares the page table. `join` is like `wait` for threads.
- Semaphore: P decrements and blocks if there is no unit. V increments and wakes a waiter. Both are atomic.
- `sleep(chan, lock)` releases the lock atomically, which prevents a lost wakeup.
- Aging fixes starvation. Priority inheritance fixes priority inversion.

## Lab 8–10 · Memory
- Page = 4 KB. VA = VPN + 12-bit offset. The offset is unchanged by translation.
- Sv39: 3 levels × 9 bits, 512 PTEs per table (4 KB / 8 B).
- PTE flags: V, R, W, X, U, A, D. A PTE with R/W/X set is a leaf. Otherwise it points to the next table.
- Walk: `PA = (PTE>>10)<<12 | offset`. The TLB caches translations. Flush with `sfence.vma` after PTE changes.
- Lazy allocation: `sbrk` only raises `p->sz`. A fault (scause 13/15, address in `stval`) allocates, zeroes and maps the page. The instruction then retries.
- Page faults are not always errors. Minor = allocate. Major = read from disk. Invalid = kill.
- Virtual size vs RSS (resident pages). Thrashing = more time paging than running.
- Protection: clear PTE_W, then a write faults. Copy-on-write uses the same mechanism.

## Page replacement numbers
Ref string: `7 0 1 2 0 3 0 4 2 3 0 3 2 1 2 0 1 7 0 1`

| Frames | FIFO | LRU | OPT |
|---|---|---|---|
| 3 | 15 | 12 | 9 |
| 4 | 10 | 8 | 8 |
| 5 | 9 | 7 | 7 |

- Belady's anomaly (FIFO only): `1 2 3 4 1 2 5 1 2 3 4 5` gives 9 faults with 3 frames and 10 with 4.
- LRU and OPT are stack algorithms, so they have no anomaly.
- OPT needs the future, so it is only a benchmark.

## Note
The PDFs mention x86 (`cr2`, `walkpgdir`). The implementation is RISC-V xv6 (Sv39). The concepts are the same. On x86 it is a 2-level 10|10|12 split, and the fault address is in CR2.
