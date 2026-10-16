# Recover an XOR Key

## Background

**XOR** (exclusive OR, `^` in Python, `xor` in assembly) compares two numbers bit by bit: a result bit is 1 where the two inputs differ. Its useful property is that XOR with the same value twice gives back what you started with: `x ^ mask ^ mask == x`. So if a program checks `input ^ mask == target`, you can work backwards from the target.

This program keeps its function names, so you can go straight to `<main>`. Both constants you need are **immediates**: numbers written inside instructions, such as the `0x3` in `and edx,0x3`. The module lessons cover what you need: the XOR section of **"Concepts: encode loops, XOR, and lookup tables"**, and **"How to read the output"** for reading a line like `cmp DWORD PTR [rbp-0x4],0x...`.

## How to start

```bash
objdump -d -M intel /challenge/xor-key | more    # type /<main> and press Enter
python3 -c 'print(hex(0xf0 ^ 0x3c))'             # XOR two numbers in Python (example values)
```

Run `/challenge/xor-key` once. It prints nothing and waits for a hexadecimal number. A wrong value prints `nope`.

## Your task

The program reads a hexadecimal number, combines it with a hidden mask using **XOR** (exclusive OR), and compares the result with a target.

Your job is to recover the two constants from `/challenge/xor-key` and calculate an input that reaches the success path.

## Useful tools

- `objdump -d -M intel /challenge/xor-key` shows the instructions in a readable x86-64 syntax.
- Python's `^` operator performs XOR on integers.

## Hint ladder

1. Find `<main>` in the disassembly and trace what happens after the input function returns.
2. Look for an `xor` instruction with a constant operand, then for the later `cmp` instruction. Those constants are the mask and target.
3. XOR is its own inverse: if `input ^ mask == target`, apply XOR with the mask to the target. Submit the computed result in hexadecimal.
