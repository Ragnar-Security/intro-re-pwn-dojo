# Simple Crackme

## Background

A **crackme** is a small program made for practicing reverse engineering: it asks for a password, and your job is to work out which one it accepts. This one stores the password as an **encoded** byte array (scrambled with XOR so `strings` does not show it), decodes it into a local buffer with a loop, and compares the result with what you typed.

The module lesson **"Concepts: encode loops, XOR, and lookup tables"** covers the three things to find: the loop (and how many rounds it runs), the address the loop reads its bytes from, and the value each byte is XORed with. **"How to read the output"** explains how to read bytes out of `objdump -s`. The **worked example** reverses a different password check end to end, with a different rule; use it for the method, not the numbers.

## How to start

```bash
strings /challenge/simple-crackme                            # first look: the messages, but not the password
objdump -d -M intel /challenge/simple-crackme | more         # type /<check_password> and press Enter
objdump -s -j .rodata /challenge/simple-crackme              # the stored bytes
```

Run `/challenge/simple-crackme` once. It asks `Enter the password:`; a wrong password prints `nope`.

## Your task

This program reads a password, decodes a hidden byte array, and compares the result with your input.

Recover the password from `/challenge/simple-crackme`.

## Useful tools

- `strings /challenge/simple-crackme` is a quick first look, but the answer is encoded.
- `objdump -d -M intel /challenge/simple-crackme` shows the decoding loop.
- `objdump -s -j .rodata /challenge/simple-crackme` shows stored bytes.
- A few lines of Python can decode the bytes once you understand the loop.

## Hint ladder

1. Find `<check_password>` and follow how it fills its local buffer before the comparison.
2. Every encoded byte is combined with the same one-byte value using **XOR** (exclusive OR, the `^` operator in Python). Find that value in the loop, the address the loop reads bytes from, and the loop's limit, which tells you how many bytes there are.
3. Extract the array bytes and XOR each one with the recovered byte. Interpret the result as text and submit the complete decoded string.
