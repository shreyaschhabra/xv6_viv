# Lab 6 · Q1 — Banker's Algorithm (deadlock avoidance)

## What the question asks
Given `n` processes and `m` resource types with **Available, Max, Allocation** matrices, compute **Need = Max − Allocation**, check whether the system is in a **safe state**, print a safe sequence, and decide whether a new **request** can be granted.

## Concepts
- **Deadlock avoidance:** grant a request only if the system stays *safe*. Needs advance knowledge of each process's maximum demand.
- **Safe state:** there exists an order (safe sequence) in which every process can get its remaining need and finish. **Unsafe ≠ deadlock**, but it *may* lead to one.
- **Safety algorithm:**
  1. `Work = Available`, `Finish[i] = false`.
  2. Find i with `Finish[i]==false` and `Need[i] ≤ Work`.
  3. `Work += Allocation[i]`, `Finish[i]=true`; repeat. If all finish → safe.
- **Resource-request algorithm** for process i asking `Request`:
  1. If `Request > Need[i]` → error (exceeded claim).
  2. If `Request > Available` → must wait.
  3. **Pretend** to allocate (Available−=, Allocation+=, Need−=), run safety check. Safe → grant; else **roll back** and wait.
- Complexity: O(m·n²).

## Viva points
- **Why is it called Banker's?** A bank never lends so much that it can't satisfy all customers' maximum credit eventually.
- **Limitations:** fixed number of processes/resources, must know Max in advance, processes must return resources in finite time.
- **Safe vs unsafe vs deadlock?** Deadlock ⊂ unsafe.
