# Lab 5 · Q3 — Readers–Writers Problem

## What the question asks
Shared data read by many processes and written by some. Allow **many readers at once**, but a **writer needs exclusive access**.

## Concepts
- **Rules:** reader–reader OK; reader–writer ✘; writer–writer ✘.
- **First readers–writers (reader preference) solution:**
```
Reader: wait(mutex); readcount++; if readcount==1 wait(wrt); signal(mutex);
        ...read...
        wait(mutex); readcount--; if readcount==0 signal(wrt); signal(mutex);
Writer: wait(wrt); ...write...; signal(wrt);
```
  - `mutex` protects `readcount`; `wrt` gives writers exclusive access; the **first reader locks out writers, the last reader lets them in**.
- **Starvation:** with reader preference, a steady stream of readers starves writers. **Second variant (writer preference)** blocks new readers when a writer waits — then readers may starve.
- **Real use:** databases, file systems (read/write locks).

## Viva points
- **Why is `readcount` protected by its own mutex?** `readcount++` is itself a race.
- **Which variant did you implement and what is the downside?** Reader preference → writer starvation.
- **Fair solution?** Use a queue/turnstile so arrival order is respected.
