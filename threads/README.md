# Lab 7 · Q1 — User-level Threads: `clone()` and `join()` in xv6

## What the question asks
Add syscalls `clone(fn, arg, stack)` and `join()` so a process can create threads that **share the parent's address space** but have their own stack and registers, and wait for them to finish.

## Concepts
- **Process vs thread:** a process has its own address space + resources; **threads of one process share code, data, heap and open files** but each has its **own stack, registers and PC**.
- **Why threads:** cheaper creation and context switches, easy data sharing, parallelism on multicores.
- **How `clone` works in xv6:** like `fork`, except the new `proc` **uses the same page table** (no copy), its **trapframe** is set so it starts at `fn(arg)` with `sp` = the new user stack, and a `ret` goes to an exit stub. Shared page table → reference counting/care when freeing memory (free only when the last thread exits).
- **`join()`:** like `wait()`, but for threads (same pagetable) — sleeps until a child thread is ZOMBIE, then reaps it and returns its id.
- **Stack:** caller allocates (e.g. `malloc` one page); stacks grow downward, so pass the **top** address.
- **Race conditions:** shared globals need locks (leads to Q2).

## Viva points
- **What is shared / private?** Shared: code, globals, heap, files. Private: stack, registers, thread id.
- **User-level vs kernel-level threads?** User-level managed by a library (fast, but one blocking call blocks all); kernel-level scheduled by the OS (true parallelism).
- **Why must memory be freed carefully?** Threads share one page table.
