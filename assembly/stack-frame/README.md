# stack-frame

## Background

Each function gets its own area of memory called a **stack frame**, which holds its local variables. The register `rbp` (the *base pointer*) marks the frame, and variables are found at a fixed distance below it, written like `[rbp-0x50]` (the memory 0x50 bytes below `rbp`).

For this challenge you need three facts about arrays on the stack:

- An array is one block of memory. **Element 0 has the lowest address**; each later element sits higher.
- Each `int` takes 4 bytes, so element `n` is `4*n` bytes above element 0.
- To fill element `i`, compiled code takes the array's start address (`lea rax,[rbp-0x..]`) and adds `i * 4` to it. You will see `movsxd` (widen the index), `shl ...,0x2` (multiply by 4) and `add`.

Lesson page 5 ("The stack and stack frames") draws a real frame and an example array. Lesson page 3 explains `lea` (it computes an address, it does not read memory).

## How to start

```
/challenge/stack-frame
objdump -d -M intel --disassemble=main /challenge/stack-frame
```

## The challenge

`/challenge/stack-frame` reads a list of decimal numbers into a local array. It then checks a single element of that array against a fixed constant. If that element is right, it prints the flag. Which element it checks is different in every copy of the challenge.

## Hints (open one at a time, stop as soon as you can continue)

1. The program asks for a list of numbers. Find in `<main>` the loop that calls `scanf` once per number (look for the backward jump). Before each call, it works out the address of the next array slot.
2. In that loop, the address computed with `lea rax, [rbp-...]` is the start of the array: element 0. Write down its offset. The `cmp` that ends the loop tells you how many numbers the array holds.
3. After the loop, find the `cmp` against a constant, and the `mov` right before it that loads the slot being checked. Write down that slot's offset.
4. The difference between the two offsets, divided by 4, is the element number counting from 0. Your input needs the constant (in decimal) at that position, counting from 1 in what you type. Any value works for the other positions.
