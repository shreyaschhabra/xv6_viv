# Lab 8 · Q1 — Virtual → Physical Address Translation (`va2pa`)

## What the question asks
Add syscall `va2pa(va)` that walks the current process's page table and returns the **physical address**, or an error if not mapped.

## Concepts
- **Virtual memory:** each process sees its own contiguous address space; the MMU + OS map it to physical RAM → isolation, protection, easy sharing.
- **Paging:** memory split into fixed **4 KB pages** (virtual) and **frames** (physical). A virtual address = **page number + offset (12 bits)**.
- **Page table:** maps virtual page → physical frame (PTE has the frame number + flag bits: V valid, R/W/X, U user, A accessed, D dirty).
- **xv6 RISC-V (Sv39):** 39-bit VA, **3-level** page table. VA = `VPN[2] (9) | VPN[1] (9) | VPN[0] (9) | offset (12)`. Walk: root table → index by VPN[2] → next table → VPN[1] → leaf table → VPN[0] → PTE → `PA = (PTE >> 10) << 12 | offset`.
- **`walk(pagetable, va, alloc)`** returns the PTE address; `walkaddr()` additionally checks valid + user bits.
- **Why multi-level?** A flat table for 2³⁹ bytes is huge; multi-level allocates only for used regions.
- (x86 version in PDF: two-level, `walkpgdir`; same idea with 10|10|12 split.)

## Example
VA `0x3000` → VPN[0]=3, offset 0; if PTE points to frame at `0x87f2e000` → PA `0x87f2e000`.

## Viva points
- **What if PTE_V is 0?** Not mapped → page fault / return -1.
- **TLB?** A cache of recent translations; flushed (`sfence.vma`) when tables change.
- **Offset unchanged by translation — why?** Page and frame are the same size.
