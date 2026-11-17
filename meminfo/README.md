# Lab 8 · Q2 — `getmeminfo()` : memory statistics syscall

## What the question asks
Return info such as **total free memory / number of free pages** and **memory used by the current process** (`proc->sz`) to user space.

## Concepts
- **Physical memory allocator (`kalloc.c`):** xv6 keeps a **linked list of free 4 KB pages** (`kmem.freelist`) guarded by `kmem.lock`. `kalloc()` pops one page, `kfree()` pushes it back.
- **Free pages count:** walk the free list under the lock and count nodes × 4096 bytes.
- **Process memory size:** `p->sz` = bytes of user address space (code+data+heap+stack); pages = `PGROUNDUP(sz)/PGSIZE`.
- **Returning data:** simple integer through the syscall return value, or `copyout()` a struct to a user pointer.
- **Why the lock:** another CPU may allocate/free while counting.
- Related: `top`/`free` in Linux read `/proc/meminfo`.

## Viva points
- **Page size?** 4096 B. **Who manages free frames?** `kalloc`.
- **Why is the count only a snapshot?** It may change right after.
- **Internal fragmentation?** Last page of a process is partially used.
