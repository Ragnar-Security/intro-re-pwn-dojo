# Observe a Program in GDB

## Background

Some answers do not exist until a program runs: they are computed from something that changes every run, such as the time the process started. Static analysis can show you *where* the value is used but not *what* it is. This is where dynamic analysis comes in: stop the program at the right instruction, read the value, and change what you need.

You need four gdb skills, all covered in the module lessons: setting a breakpoint on an address (`break *ADDRESS`, with the star), reading registers and memory (`info registers`, `p/x $REG`, `x/wx ADDRESS`), changing a register (`set $REG = VALUE`), and attaching to a running process (`attach PID`). The table in **"Ghidra and gdb in this dojo"** lists them, and Step 4 of **"Worked example: reversing a toy password check"** stops at a check and overwrites `eax` to change the result. Here you do the same thing to a different program, in a different way.

Normally a program started by a debugger loses its permission to read `/flag`. This challenge is built so that does not matter: you only debug the worker, and the parent, which you never touch, reads the flag.

## Your task

A **debugger** pauses a running program so you can inspect and change its state. This program computes a fresh value every time it runs, so reading the disassembly without running the program cannot tell you the answer.

When it starts, the program splits into two **processes** (running copies of a program). The parent keeps the permission to read the flag and waits. The **worker** gives up that permission, prints its **PID** (process ID, the number Linux uses to name a process), and asks for the value. You **attach** GDB to the worker, meaning GDB takes control of that already-running process. If the worker's check passes, the parent prints the flag. Use two terminals: open a second terminal window or tab in your workspace (or a second SSH session).

## How to start

In terminal 1:

```bash
/challenge/gdb-runtime
```

Leave it waiting for input. In terminal 2, locate the comparison and start GDB:

```bash
objdump -d -M intel /challenge/gdb-runtime | grep -A 15 '<values_match>:'
gdb /challenge/gdb-runtime
```

## Hint ladder

1. In GDB, use `attach PID`, set a breakpoint on the `cmp` instruction inside `values_match`, and run `continue`. Then enter any hexadecimal value in terminal 1.
2. When the breakpoint stops, use `x/1i $pc`, `info registers`, and a memory examination command such as `x/wx ADDRESS` to identify both comparison operands.
3. Use GDB's `set` command to make the input-side operand equal the expected-side operand, then continue. The parent will print the flag in terminal 1.
