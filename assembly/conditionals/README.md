# conditionals

## Background

An `if` in a program becomes two instructions: `cmp A, B` (compare, which records whether A is smaller, equal or bigger in the **flags**) and a **conditional jump** such as `ja` or `je` that either jumps somewhere else or keeps going, depending on that result. This program uses unsigned numbers, so its jumps are the unsigned family:

| Jump | Means |
|---|---|
| `je` / `jne` | jump if equal / not equal |
| `ja` / `jae` | jump if above / above or equal |
| `jb` / `jbe` | jump if below / below or equal |

Two things trip up almost everyone, and lesson page 4 ("Flags and conditional jumps") shows both on real compiler output:

- At `-O0` the jump usually **skips** the code inside the `if`, so the condition in the assembly is the opposite of the one in the C. Read each jump as "when do we leave?"
- The compiler may write "at least N" as "above N-1", so the constant you see can be one less than you expect.

`and eax, 0x3` keeps only the lowest two bits of `eax`. It is how a program asks "what is this number's remainder when divided by 4?"

## How to start

```
/challenge/conditionals
objdump -d -M intel --disassemble=main /challenge/conditionals
```

Try a few numbers first and note which message you get. The messages tell you which check you failed.

## The challenge

`/challenge/conditionals` reads one hex number and runs it through five checks in a row. Each failed check prints a short message (for example "too small"). Only numbers that pass all five reach the flag.

## Hints (open one at a time, stop as soon as you can continue)

1. In `<main>`, look for the `cmp` / jump pairs after the `scanf` call. Each one lines up with one of the messages: each failure path calls `puts` to print one.
2. Two checks compare against limits (too small, too big). One check uses `and` first, then compares to a small number: that is a pattern the low bits of your number must match. Two checks near the end form a window.
3. The jump decides what counts as passing: for example `ja` jumps past the failure message only when the number is above the constant. Write the five conditions as plain inequalities.
4. Pick a number that is inside the window and also matches the low-bits pattern. Remember the compiler sometimes writes "greater than or equal to N" as "greater than N-1", so check your edge values by running the program.
