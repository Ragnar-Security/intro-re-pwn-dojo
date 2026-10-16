# objdump-disasm

## Background

`objdump -d -M intel` turns a program file back into assembly. You do not need to understand every instruction. You are looking for `cmp` instructions in `main` and the number written inside one of them (an **immediate**). The lesson **"Reading a file without running it"** has an annotated listing of a small `main` and explains the columns, `[rbp-0x14]`, `call ...@plt` and `cmp` followed by a jump.

Start with the module's **Start here** lessons for the tools and vocabulary, then come back.

### How to start

```
/challenge/objdump-disasm
objdump -d -M intel /challenge/objdump-disasm | grep -n -A3 -B3 cmp
```
Find the lines belonging to `main` (they follow the `<main>:` label) and read the `cmp` lines.

## What this is about

A compiled program is a list of **instructions**, small commands for the CPU. **Assembly** is the readable text form of those instructions, and a **disassembler** turns a program file back into assembly. `objdump -d` is a disassembler.

`main` is the C function where the program's own code starts. A number written straight into an instruction, like the `0x10` in `cmp eax,0x10`, is called an **immediate**. `cmp` compares two values, and the conditional jump right after it (such as `jne`, "jump if not equal") picks which code runs next based on that result.

## The challenge

`/challenge/objdump-disasm` asks for a secret number in **hex** (base 16: digits `0-9`, then `a-f`). The secret is an immediate in one of `main`'s `cmp` instructions. Type the right number and it prints the flag.

Your copy has its own number, so a number from someone else's copy will not work.

## Try it

```
/challenge/objdump-disasm
objdump -d -M intel /challenge/objdump-disasm | more
```

`-M intel` picks the Intel style of writing assembly, which puts the destination first: `cmp eax,0x10` compares `eax` with `0x10`.

## Hints (open one at a time, stop as soon as you can continue)

1. In `less`, type `/<main>:` and press Enter to jump to `main`. Ignore everything before it; that is setup code.
2. `main` has more than one `cmp`. The first one checks that reading your input worked at all (`scanf` reports how many values it read).
3. The comparison against your number is the one with a large hex immediate. Type its digits, with or without `0x`.
