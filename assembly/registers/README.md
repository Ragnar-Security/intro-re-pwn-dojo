# registers

## Background

When a program runs, the CPU does its math in tiny, very fast storage slots called **registers**. A register holds one number at a time. Programs are made of **instructions**, which are single small commands such as "add this to that". **Assembly** is the readable text form of those instructions. A **disassembler** (`objdump`) turns a compiled program back into assembly so you can read it.

Numbers in assembly are usually written in **hex** (base 16: digits `0-9` then `a-f`, with `0x` in front). A number written directly inside an instruction, like the `0x64` in `add eax,0x64`, is called an **immediate**.

For this challenge you need four instructions and one idea:

- `mov`, `add`, `sub`, `shl` and `xor` (module lesson page 3, "The core instructions").
- Intel syntax puts the destination first: `add eax, 0x10` means `eax = eax + 0x10` (page 1).
- `DWORD PTR [rbp-0x4]` means "the 4-byte value stored 4 bytes below the address in `rbp`", which is where `-O0` code keeps a local variable (page 5).

If any of those words are new, read lesson pages 1 to 3 of this module first. The worked example on page 9 walks through code shaped like this one line by line.

## How to start

```
/challenge/registers
objdump -d -M intel --disassemble=main /challenge/registers
```

The first command runs the program so you see what it asks. The second prints only the `main` function in Intel style. (`objdump -d -M intel /challenge/registers | more` shows the whole program a screen at a time; Space for more, `q` to quit.)

## The challenge

`/challenge/registers` starts with a number, then applies four operations to it: an add, a left shift, an xor, and a subtract. It asks you for the final number, in hex without a `0x` in front. If you type the right value, it prints the flag.

You never see the starting number or the operation values unless you read the assembly.

## Hints (open one at a time, stop as soon as you can continue)

1. Only read inside `main`. The first few calls in it (`geteuid`, `setreuid`, `printf`, `fflush`, `scanf`) are setup and reading your input; the part you want comes after `scanf`.
2. After the input is read, look for a short run of lines that all touch the same stack slot, written `DWORD PTR [rbp-0x..]`, and each end in a hex number (a shift by 1 may show a plain `1` instead of `0x1`).
3. The first of those lines (`mov`) stores the starting value. The next lines are the operations, in the order the program runs them: `add` adds, `shl` shifts bits left (shifting left by N multiplies by 2 to the power N), `xor` flips bits, `sub` subtracts.
4. Do the operations in the same order in Python (`python3`, then `hex(...)`). The program uses 32-bit numbers, so keep only the lowest 32 bits: `& 0xffffffff`. Type the result without the `0x`.
