# Lab 2 · Q1 — fork(), wait() and process order (`forkchild.c`)

## What the question asks
Parent forks a child. Child prints "I am child" + PID and exits. Parent prints "I am parent" + PID and reaps the child. Observe print order; make the parent always print after the child using `wait()`.

## Concepts
- **`fork()`** creates a near-identical copy of the calling process. Returns **0 in the child**, the **child's PID in the parent**, **-1 on failure**. After fork, both run the same code from the next line.
- **Order is non-deterministic** without synchronization: the scheduler decides who runs first. Run it several times and the order can change.
- **`wait(&status)`** blocks the parent until a child terminates, and **reaps** it (collects exit status so the kernel can free its process-table entry).
- **Zombie:** child that has exited but is not yet reaped. **Orphan:** child whose parent exited first (adopted by `init`).
- **`getpid()`** returns own PID; `getppid()` the parent's.

## Logic
```
pid = fork();
if pid == 0   → child: print "I am child", getpid(); exit(0)
else          → parent: wait(NULL);   // forces child to finish first
                print "I am parent", getpid()
```
With `wait()` placed **before** the parent's print, the output is always child → parent.

## Viva points
- **Why are two outputs possible?** Parent and child are independent schedulable processes.
- **What does fork return?** 0 / child PID / -1.
- **What happens if parent never waits?** Child becomes a zombie until the parent exits.
- **Is memory shared after fork?** No — copy (copy-on-write in Linux); changes in one are invisible to the other.
