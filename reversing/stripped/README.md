# A Stripped Binary

## Background

A **stripped** binary has had its **symbol table** (the list of function and variable names) removed. The machine code is all still there, but tools such as `objdump` can no longer label functions like `main` for you, and gdb cannot `break main`.

You find `main` by following the program's startup code. The **entry point** (the first instruction the program runs) is a short routine that places the address of `main` in the `rdi` register and calls the C library function `__libc_start_main`, which later calls `main`. The module lesson **"Concepts: finding main in a stripped program"** explains this with a picture, and Step 2 of **"Worked example: reversing a toy password check"** does it on a program that is not this one. Once you have found `main`, the rest is reading one comparison.

## How to start

```bash
file /challenge/stripped                          # confirms it is stripped
readelf -h /challenge/stripped                    # look for "Entry point address"
objdump -d -M intel /challenge/stripped | more    # type / and the address (no 0x) to jump there
```

Run `/challenge/stripped` once to see how it behaves. It prints nothing and waits for you to type a hexadecimal number and press Enter. A wrong value prints `nope`.

## Your task

Run `/challenge/stripped` and provide the required value in hexadecimal.

## Useful tools

- `file /challenge/stripped` confirms the file type and that it is stripped.
- `readelf -h /challenge/stripped` shows the ELF entry-point address.
- `objdump -d -M intel /challenge/stripped` disassembles the machine code. In a stripped program, the names objdump prints in `< >` are guesses based on the nearest name it still knows; trust the addresses, not those names.

## Hint ladder

1. Start at the **entry point** (the first instruction the program runs) printed by `readelf`. That startup code eventually hands the address of `main` to the C library.
2. On x86-64 Linux, the first function argument is passed in the `rdi` register. Near the end of the startup code, look for an address placed in `rdi` just before the call into the C library.
3. Find that address in the disassembly and follow your input to the comparison. The program compares your number with an **immediate** (a constant written directly inside the instruction); the program reads your input as hexadecimal.
