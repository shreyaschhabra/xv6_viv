# Lab 8 · Q3 — `vmprint()` : print a page table

## What the question asks
Print the page table of a process (all 3 levels) showing indices, PTEs and physical addresses — in `exec()` for pid 1 or via a syscall.

## Concepts
- Recursive traversal of the 3-level tree: each table has **512 entries** (9 bits). For each entry with `PTE_V`: print its index, PTE value, PA. If it has no R/W/X flags it points to a **lower-level table** → recurse with `..` indentation; otherwise it's a **leaf (real mapping)**.
```
page table 0x87f6e000
 ..0: pte 0x21fd9c01 pa 0x87f67000
 .. ..0: pte 0x21fd9801 pa 0x87f66000
 .. .. ..0: pte 0x21fda01b pa 0x87f68000   (code: R,X,U)
```
- **Flag bits:** V=valid, R/W/X=permissions, U=user accessible, G=global, A=accessed, D=dirty.
- Reading the output: low entries = user code/data/stack; high entries (index 255 → trampoline & trapframe) near the top of the address space.
- Useful for understanding the **layout of a process**: text, data, guard page (no U bit), stack, heap, trapframe, trampoline.

## Viva points
- **How to tell a leaf PTE?** Any of R/W/X set.
- **Purpose of the guard page?** Catches stack overflow (page fault).
- **Why 512 entries per table?** 4 KB / 8-byte PTE = 512.
