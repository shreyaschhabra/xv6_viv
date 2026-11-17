# Lab 1 · Q2 — Leap Year and Number Classification (`leapyear.sh`)

## What the question asks
Read a **year** → say if it is a leap year. Read a **number** → say **even/odd** and **positive/negative/zero**.

## Concepts
- **Leap year rule:** divisible by 4 **and** (not divisible by 100 **or** divisible by 400).
  - 2024 ✔ (÷4, not ÷100) · 1900 ✘ (÷100 but not ÷400) · 2000 ✔ (÷400).
  - *Why:* a solar year is ≈365.2425 days; the 100/400 exceptions correct the over-counting of adding a day every 4 years.
- **Modulo `%`** gives the remainder. `year % 4 -eq 0` means divisible by 4.
- **Even/odd:** `n % 2 == 0` → even. (Negative numbers: `-3 % 2` is `-1` in bash, so compare with `0`, not `1`.)
- **Sign check:** compare with 0 using `-gt`, `-lt`, `-eq`.
- **`read -p "prompt" var`** — reads user input into a variable.
- **Logical operators in `[[ ]]`:** `&&` (and), `||` (or).

## Logic
```
read year
if (year%4==0 && (year%100!=0 || year%400==0)) → Leap else Not Leap
read n
if n%2==0 → Even else Odd
if n>0 → Positive, elif n<0 → Negative, else Zero
```

## Viva points
- **Is 1900 a leap year?** No. **2000?** Yes.
- **Difference between `[ ]` and `[[ ]]`?** `[[ ]]` is a bash keyword: supports `&&`, `||`, pattern/regex matching and needs less quoting; `[ ]` is the older POSIX `test` command.
- **`-eq` vs `==`?** `-eq` is numeric comparison; `==` / `=` is string comparison.
