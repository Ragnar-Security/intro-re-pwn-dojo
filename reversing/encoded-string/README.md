# Decode a String

## Background

To keep a password out of `strings`, a program can store it **encoded**: scrambled by a simple, reversible rule. A **decode loop** then turns it back into text when the program needs it. Reading that loop gives you the rule. In this challenge, each position in the string is scrambled with a different value, and more than one operation is involved, so the order of the steps matters.

The module lesson **"Concepts: encode loops, XOR, and lookup tables"** covers what you need: what a loop looks like in `-O0` assembly, how to count its rounds (`jl` versus `jle`), how a byte array is read (`movzx eax,BYTE PTR [rax+ADDRESS]`), and how to undo steps in reverse order with `& 0xff` in Python. The **worked example** (Steps 3 and 5) reverses a different loop end to end; its rule is not this one, so its script will not work here unchanged.

Two routes work. You can rebuild the loop in Python from what you read (static analysis), or let the program decode the password and look at memory in gdb (dynamic analysis). Try the first one: reading the loop is the skill this challenge is for.

## How to start

```bash
objdump -d -M intel /challenge/encoded-string | more    # type /<decode> and press Enter
objdump -s -j .rodata /challenge/encoded-string         # the stored bytes
```

Run `/challenge/encoded-string` once. It asks `Enter the password:`; a wrong password prints `nope`.

## Your task

The correct password is not stored as readable text. The program keeps an **encoded** (scrambled) byte array, **decodes** it back into text with a loop when it starts, and compares the result with your input. `strings` will not show the password.

Recover the readable password from `/challenge/encoded-string` and type it when the program asks.

## Useful tools

- `objdump -d -M intel /challenge/encoded-string` shows the decoding loop.
- `objdump -s -j .rodata /challenge/encoded-string` shows bytes stored in the read-only data section. Each row starts at an address that is a multiple of 16, so the array can begin in the middle of a row: count bytes from the array's own address, not from the start of the row.
- A short Python script is useful for applying the inverse operation to every byte.

## Hint ladder

1. Find the function named `decode`. Identify its loop counter, where it reads one byte of the encoded array, and where it writes one decoded byte. The byte read (`movzx eax, BYTE PTR [rax+ADDRESS]`) gives the array's address. The loop's `cmp DWORD PTR [rbp-0x4], N` check gives its length: the loop runs while the counter is at most `N`, so there are `N + 1` bytes.
2. Before the loop starts, `decode` stores three constants into local variables on the stack. Work out what the loop does with each one: one is a starting value, one is multiplied by the position, and together they make a different byte for each position. The order matters: the loop first XORs the encoded byte with that per-position byte, and then subtracts the third constant.
3. Extract the encoded array from `.rodata` and reproduce the loop in Python in the same order. The program works on single bytes, so every step keeps only the lowest 8 bits; in Python, add `& 0xff` after the per-position calculation and after the subtraction. Convert the resulting byte sequence to text and submit it.
