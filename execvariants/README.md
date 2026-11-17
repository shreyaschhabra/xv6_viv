# Lab 2 · Q2 — exec family: `execl`, `execlp`, `execvp` (running `ls -l`)

## What the question asks
Write three programs that each replace themselves with `ls -l` using `execl`, `execlp`, `execvp`; the output must equal the shell's `ls -l`.

## Concepts
- **`exec` replaces the current process image** (code, data, stack) with a new program. The **PID stays the same**. On success it **never returns**; it returns -1 only on error.
- Naming letters: **l** = arguments as a **list** (`"ls","-l",NULL`), **v** = arguments as a **vector/array** (`char *args[]`), **p** = search the **PATH** for the program, **e** = pass a custom environment.

| Call | Args | PATH search | Example |
|---|---|---|---|
| `execl` | list | no (full path needed) | `execl("/bin/ls","ls","-l",NULL)` |
| `execlp` | list | yes | `execlp("ls","ls","-l",NULL)` |
| `execvp` | vector | yes | `execvp("ls", args)` |

- `argv[0]` (the first "ls") is the program's own name by convention; the list must end with `(char*)NULL`.
- Any code after a successful `exec` is never executed (the old image is gone). Put `perror("exec")` after it to catch failure.

## Viva points
- **fork vs exec?** fork duplicates a process; exec replaces the program in the current process. Shells combine them: fork → child execs → parent waits.
- **Why does the new process keep open files?** exec preserves file descriptors (unless close-on-exec) — this is how shell redirection works.
- **When use execvp over execl?** When the argument count is only known at runtime (array) and you want PATH lookup.
