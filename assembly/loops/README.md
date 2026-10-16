# loops

## Background

A **loop** repeats some code. In assembly a loop is a jump that goes *backwards* to an earlier instruction (a lower address) until a check says stop. Most loops have a **counter** (here `i`, going 0, 1, 2, ...) and an **accumulator**, a running value that each repeat updates.

`gcc -O0` builds a `for` loop in a fixed shape: set up, `jmp` down to the check, then the body, the step (`i++`), and at the bottom a `cmp` plus a conditional jump back up to the body. Lesson page 8 ("Loops") takes a real loop apart line by line and watches it run in gdb.

This program uses 32-bit unsigned math, so values wrap around past about four billion. In Python, keep the lowest 32 bits with `& 0xffffffff` after every step (lesson page 1).

## How to start

```
/challenge/loops
objdump -d -M intel --disassemble=main /challenge/loops
```

## The challenge

`/challenge/loops` asks how many times to run its loop. The loop updates one value with a fixed recipe that uses the counter, over and over. When the loop ends, the program compares the value with a target. If you chose the right repeat count, it prints the flag.

The starting value, the recipe's numbers, and the target are all stored as constants in the program.

## Hints (open one at a time, stop as soon as you can continue)

1. In `<main>`, find the jump that goes backwards (to a lower address than its own). The instructions between its target and the jump are the loop body plus the check.
2. Two stack slots matter: one is the counter (it goes up by 1 each repeat) and one is the accumulator. A constant stored to the accumulator before the loop is its starting value.
3. Inside the body, find the `imul` (a multiply by a constant) and the `add`s. Write the recipe as one line: `acc = acc * ? + i + ?`. The comparison after the loop ends holds the target.
4. Write that recipe in Python, run it repeat by repeat, and stop when the value matches the target. Keep only the lowest 32 bits each time (`& 0xffffffff`). Your answer is the number of repeats, in decimal.
