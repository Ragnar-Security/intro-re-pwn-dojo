# Follow Multiple Functions

## Background

A **function pointer** is a stored address of a function. A program can keep several of them in an array (a **table**) and run "entry number N" with an **indirect call**: a `call` whose target comes from a register or from memory, such as `call rdx`, instead of a fixed target like `call ADDRESS <some_function>`. Each entry is one 8-byte address, so entry N sits 8 × N bytes after the start of the table.

This table starts out empty. It lives in a section that is all zeros in the file, and a **constructor** fills it in: a function the C runtime calls automatically before `main`. So you cannot read the table's contents from the file; you read the code that writes them, or look at the table while the program runs.

The module lesson **"Concepts: encode loops, XOR, and lookup tables"** shows what a table read looks like in assembly (the "Lookup tables" section), and **"Ghidra and gdb in this dojo"** lists the gdb commands for looking at memory.

## How to start

```bash
nm /challenge/multi-func                              # every function and global, with its address
objdump -d -M intel /challenge/multi-func | more      # read <main>, then <init_table>
```

Run `/challenge/multi-func` once. It prints nothing and waits for a number from 0 through 7. A decoy entry prints a message.

## Your task

This program builds a table of eight function pointers and calls the entry selected by your number. Seven entries print a decoy message; one reaches the flag reader.

Analyze `/challenge/multi-func` and enter the winning table index from 0 through 7.

## Useful tools

- `objdump -d -M intel /challenge/multi-func` disassembles every named function.
- `nm /challenge/multi-func` lists symbols and addresses.
- GDB can inspect the table after program startup if you prefer a live view.

## Hint ladder

1. In `<main>`, find the **indirect call**: a `call` whose target is a register or memory value instead of a fixed function name. Work backward to see that your number selects one table element.
2. The table is filled before `main` runs, by a **constructor** (a function the C runtime calls automatically at startup) named `init_table`. Disassemble that function and follow its writes to `TABLE`.
3. Match the address written by the special final table write to the function that reaches `print_flag`, then convert that table offset into an index.
