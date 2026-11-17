# Lab 2 · Q3 — `runcmd.c` : run a command with one argument

## What the question asks
`./runcmd <command> <arg>` → fork a child, exec the command in it, parent waits, reaps and prints "Command successfully completed". Wrong number of arguments → error.

## Concepts
- **`argc / argv`:** `argc` = number of arguments (including program name). `./runcmd sleep 10` → `argc=3`, `argv[1]="sleep"`, `argv[2]="10"`. Require `argc == 3`.
- This is the **core of a shell**: *read → fork → exec → wait*.
- Use **`execvp(argv[1], args)`** with `args = {argv[1], argv[2], NULL}` so the command is looked up in PATH.
- **Parent waits** with `wait()`/`waitpid()`; check status with `WIFEXITED(status)` and `WEXITSTATUS(status)` to confirm success (exit code 0).
- If `exec` fails in the child, print an error and `exit(1)` — otherwise the child would continue running the parent's code.

## Flow
```
check argc → fork
child : execvp(cmd, [cmd,arg,NULL])
parent: wait → print "Command successfully completed"
```

## Foreground vs background
Foreground: shell waits for the child. Background (`cmd &`): shell does **not** wait; it prints the prompt immediately (and must reap later, e.g. with `WNOHANG` or a `SIGCHLD` handler).

## Viva points
- **Why fork before exec?** exec destroys the caller; fork keeps the shell alive.
- **Why must the program check argc?** Accessing a missing `argv[2]` is undefined behavior.
- **What is a zombie here?** If the parent skips `wait()`, the finished command remains a zombie.
