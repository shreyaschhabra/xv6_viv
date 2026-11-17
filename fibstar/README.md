# Lab 1 · Q3 — Fibonacci Series and Star Pattern (`fibstar.sh`)

## What the question asks
Read **N**. Print the first N Fibonacci terms (0, 1, 1, 2, 3, 5 …) with a loop, then a right-angled star triangle of N rows using **nested loops**.

## Concepts
- **Fibonacci:** each term = sum of previous two. Keep two variables `a=0, b=1`; in each iteration print `a`, then `next=a+b; a=b; b=next`.
- **Loops in bash:** `for ((i=0;i<N;i++))` (C-style) or `while [ $i -lt $N ]`.
- **Nested loop:** outer loop = row number `i` (1..N); inner loop prints `i` stars on that row; `echo` after the inner loop moves to a new line.
- **`echo -n`** prints without a newline, so stars stay on the same row.

## Logic
```
a=0; b=1
repeat N times: print a; t=a+b; a=b; b=t

for i in 1..N:
    for j in 1..i: echo -n "*"
    echo
```
N = 4 gives `*`, `**`, `***`, `****`.

## Complexity
Fibonacci: O(N) time, O(1) space. Pattern: O(N²) characters printed (1+2+…+N).

## Viva points
- **Why not recursion?** In bash it is slow (each call spawns a subshell if used with `$(...)`); a loop is simpler.
- **What if N = 0 or negative?** Validate input and print an error.
- **Why does the inner loop depend on the outer?** Row `i` needs exactly `i` stars — that is what makes it a triangle.
