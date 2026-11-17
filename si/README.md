# Lab 1 · Q1 — Simple Interest Calculator (`si.sh`)

## What the question asks
Take **Principal, Rate, Time** as command-line arguments, print **SI = (P×R×T)/100** and **Total = P + SI**. If the argument count is not exactly 3, print a usage message and exit.

## Concepts you must know
| Concept | Plain explanation |
|---|---|
| Shebang `#!/bin/bash` | First line; tells the OS which interpreter runs the script. |
| Positional parameters | `$1 $2 $3` are the arguments; `$#` is how many were passed; `$0` is the script name. |
| Exit status | `exit 1` = error, `exit 0` = success. Checked by the caller with `$?`. |
| Integer arithmetic | Bash only does integers: `$(( P * R * T / 100 ))`. For decimals use `bc`. |
| `if [ $# -ne 3 ]` | Test on argument count; `-ne` = "not equal" (numeric). |

## Logic (steps)
1. Check `$# -ne 3` → print `Usage: ./si.sh P R T` → `exit 1`.
2. Store `$1,$2,$3` in named variables (readable code).
3. Compute `SI=$(( P*R*T/100 ))`, `TOTAL=$(( P + SI ))`.
4. Print both.

Example: `./si.sh 10000 5 2` → SI = 10000×5×2/100 = **1000**, Total = **11000**.

## Edge cases / why they matter
- Wrong number of arguments → usage message (stops silent wrong results).
- Non-numeric input → can be checked with a regex `[[ $x =~ ^[0-9]+$ ]]`.
- Integer division truncates (e.g. 1050/100 → 10); use `bc` for exact decimals.

## Viva points
- **Why `$(( ))` and not `expr`?** `$(( ))` is built into bash (no extra process); `expr` is an external program.
- **Difference between `$*` and `$@`?** `"$@"` keeps each argument separate; `"$*"` joins them into one string.
- **What does `chmod +x` do?** Gives execute permission so you can run `./si.sh`.
