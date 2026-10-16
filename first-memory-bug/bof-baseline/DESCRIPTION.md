This program lets you leave a note for the next club meeting. It has a function called `win()` that prints the flag, but nothing in the program ever calls it. Make the program run it anyway.

Run `/challenge/bof-baseline`.

New idea for this challenge: the **return address**. When one function calls another, the CPU saves the address of the next instruction on the stack, so it knows where to continue when the called function finishes. When `main()` finishes, it jumps to whatever address is stored there. If you can overwrite that saved return address with the address of `win()`, `main()` "returns" into `win()`.

The bug: `main()` reads far more bytes into its note buffer than the buffer can hold (a **buffer overflow**). There is nothing else stored next to the buffer this time, so your bytes run past the end of `main()`'s variables, over a saved copy of the `rbp` register, and onto the saved return address.

You need two numbers from the binary:

1. The address of `win()`. The binary is built without **PIE** (position-independent executable), so `win()` is at the same address on every run.
2. How many filler bytes it takes to reach the saved return address.

There is no stack canary (a guard value that would catch the overflow). Your variant has its own buffer size, so work the numbers out from your binary.
