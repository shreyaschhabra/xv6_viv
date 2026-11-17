# Lab 7 · Q2 — Semaphores in xv6 (`sem_init`, `sem_wait`, `sem_signal`)

## What the question asks
Implement kernel semaphores (table of up to 16) and syscalls to create, wait on and signal them; use them to protect a shared counter between threads.

## Concepts
- **Semaphore (Dijkstra):** integer `S` with two **atomic** operations:
  - `wait(P)`: `S--`; if `S<0` block (sleep).
  - `signal(V)`: `S++`; if `S≤0` wake one waiter.
  - `S=1` → binary semaphore (mutex); `S=N` → counting semaphore (N identical resources).
- **Kernel implementation:** `struct sem { struct spinlock lock; int value; }` array `sem[MAX_SEM]`. `sem_wait`: acquire lock; `while(value<=0) sleep(&sem, &lock)`; `value--`; release. `sem_signal`: acquire; `value++`; `wakeup(&sem)`; release.
- **Why `sleep(chan, lock)` takes the lock:** it atomically releases the lock and sleeps → no **lost wakeup**.
- **Spinlock vs sleeplock:** spinlock = busy waits (short, interrupts off); semaphore/sleeplock = process sleeps (long waits).
- **Test:** N threads doing `counter++` 1000 times → expected N×1000 *with* semaphore, wrong value without.

## Viva points
- **Why must wait/signal be atomic?** Otherwise two processes can both pass the check.
- **Semaphore vs mutex vs spinlock.**
- **What is a lost wakeup?** Signal arrives between "check condition" and "go to sleep"; prevented by holding the lock.
