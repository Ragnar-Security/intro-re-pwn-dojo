# arithmetic

## Background

Programs do math with a few instructions: `add` (plus), `sub` (minus), `imul` (times), and `xor` (flip bits where the other number has a 1). Lesson page 3 ("The core instructions") shows each one with a real before-and-after example.

Everything here is **32-bit unsigned**: numbers from 0 up to about four billion that wrap around to 0 when they get too big, like a car odometer. In Python, copy that with `& 0xffffffff` (lesson page 1, "Hex in one minute").

An operation can be undone. If you know the result of `x xor K`, then `result xor K` gives you `x` back. Adding undoes subtracting, and subtracting undoes adding.

Your three numbers arrive through one `scanf` call. Its first argument (`rdi`) is the format text; the next ones (`rsi`, `rdx`, `rcx`, in that order) are the addresses of the variables to fill, set up with `lea` right before the call. So the `lea`s tell you which stack slot holds your first, second and third number (lesson page 3, "`lea` versus `mov`", and page 6, "The calling convention"). Write those slots down first.

## How to start

```
/challenge/arithmetic
objdump -d -M intel --disassemble=main /challenge/arithmetic
```

## The challenge

`/challenge/arithmetic` asks for three decimal numbers `a b c` on one line. It computes a result from them in several steps and compares it with a target stored in the program. If the result matches, it prints the flag.

There is more than one right answer. You only need one.

## Hints (open one at a time, stop as soon as you can continue)

1. Look inside `<main>`. Find which stack slot each of your three numbers lands in. Then, after the input is read, find where it multiplies two of them (`imul`) and adds the third.
2. The lines after that change one stack slot using numbers written in the instruction. Write down each operation and its number, in order. The last line compares that slot with the target.
3. Work backwards from the target: undo the last operation first, then the one before it.
4. You are free to pick `a` and `b`. Choose them so the multiplication is as easy as possible, then make `c` do the rest. Send your three numbers in decimal.
