# Lab 1 · Q4 — Palindrome and Prime Checker using Functions (`palprime.sh`)

## What the question asks
Define two functions: `is_palindrome` (reads same forwards and backwards) and `is_prime`. Read a value; always check palindrome; if the value is purely numeric also check prime.

## Concepts
- **Functions in bash:** `name() { ... }`; arguments are `$1`, `$2` *inside* the function; return a status with `return 0` (true) / `return 1` (false). Call as `if is_prime "$n"; then ...`.
- **Palindrome:** reverse the string (`rev` command or loop) and compare with the original. `131` → `131` ✔; `123` → `321` ✘.
- **Prime:** a number > 1 with no divisor other than 1 and itself. Test divisors from 2 up to **√n** (if n has a factor larger than √n, it also has one smaller). 0 and 1 are not prime.
- **Numeric check:** `[[ $x =~ ^[0-9]+$ ]]` (regex: only digits).
- **`local`** keyword keeps function variables from leaking into the main script.

## Logic
```
is_palindrome(s): rev = reverse(s); return (s == rev)
is_prime(n):      if n<2 → false; for d=2; d*d<=n → if n%d==0 → false; true
main: read v; is_palindrome v; if v numeric → is_prime v
```
Example: `131` → "131 is a Palindrome, 131 is a Prime number".

## Required extras for all Lab-1 scripts
`script -a session_log.txt` (records the terminal session), `history > history.txt`, and the outputs of `date`, `uname -a`, `whoami` — proof of authorship.

## Viva points
- **Why check only up to √n?** Factors come in pairs (a×b = n); one of them is always ≤ √n. Cuts time from O(n) to O(√n).
- **Return value vs output of a function?** `return` gives only an exit status 0–255; to "return" text, `echo` it and capture with `$(...)`.
- **Is "Madam" a palindrome?** Only if case is ignored — convert with `tr 'A-Z' 'a-z'` first.
