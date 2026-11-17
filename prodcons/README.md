# Lab 5 · Q2 — Producer–Consumer (Bounded Buffer)

## What the question asks
A producer puts items into a fixed-size buffer; a consumer removes them. Use synchronization so the producer **waits when full**, the consumer **waits when empty**, and buffer access is safe.

## Concepts
- **Bounded-buffer problem:** classic synchronization problem — shared circular buffer of size N with `in` and `out` indices.
- **Three semaphores** (or sleep/wakeup in xv6):
  - `mutex = 1` — mutual exclusion on the buffer.
  - `empty = N` — number of free slots (producer waits on it).
  - `full = 0` — number of filled slots (consumer waits on it).
- **Semaphore:** integer with atomic `wait(P)` (decrement, block if <0/0) and `signal(V)` (increment, wake one waiter).
```
Producer: wait(empty); wait(mutex); buf[in]=item; in=(in+1)%N; signal(mutex); signal(full)
Consumer: wait(full);  wait(mutex); item=buf[out]; out=(out+1)%N; signal(mutex); signal(empty)
```
- **Order matters:** waiting on `mutex` *before* `empty/full` can **deadlock** (producer holds mutex while waiting for space; consumer can't get the mutex to free it).
- xv6 primitives: `sleep(chan, lock)` / `wakeup(chan)` are the kernel's condition-variable-like tools; **always re-check the condition in a `while` loop** (spurious wakeups).

## Viva points
- **What bug without sync?** Overwriting unread items, reading empty slots, corrupted indices.
- **Why circular buffer?** Reuse slots with `% N`; no shifting.
- **Semaphore vs mutex?** Mutex = binary, owned by locker; semaphore = counting, signalled by anyone.
