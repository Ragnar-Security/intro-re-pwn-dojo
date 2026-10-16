# file-readout

## Background

Compiled Linux programs are **ELF** files, and `file` and `readelf -h` tell you what kind. The lesson **"Reading a file without running it"** explains the `Type:` line (`EXEC`, `DYN`, `REL`) and why `x86-64` is not enough to tell files apart. After you find the real program, the second half is the same job as `objdump-disasm`: read `main` with `objdump -d -M intel`.

Start with the module's **Start here** lessons for the tools and vocabulary, then come back.

### How to start

```
file /challenge/candidate_0
readelf -h /challenge/candidate_0 | grep -E 'Type|Machine'
```
Repeat for each candidate, or use a loop: `for f in /challenge/candidate_*; do echo $f; readelf -h $f | grep Type; done`.

## What this is about

**ELF** is the file format Linux uses for compiled code. Several different kinds of ELF file exist:

- an **executable** is a complete program you can run;
- a **shared library** (also called a shared object) holds code that other programs load while they run; running it directly does nothing useful;
- a **relocatable object** (a `.o` file) is half-built code that still has to be combined with other pieces before it can run.

The **architecture** is the kind of CPU the code is written for. These files are all for **x86-64**, the 64-bit Intel and AMD CPUs used in most laptops and servers.

The `file` command reads the first bytes of a file and says what it is. `readelf -h` prints the **ELF header**, the table at the start of an ELF file; its `Type:` line names the kind of file.

## The challenge

`/challenge/` holds `candidate_0` through `candidate_7`. Exactly one is an x86-64 executable. Find it by looking at the files, not by running them. Then solve it like `objdump-disasm`: it asks for a secret number in hex, and the number is an immediate in one of `main`'s `cmp` instructions.

Which candidate is real, and the number, differ between copies of this challenge.

## Try it

```
file /challenge/candidate_*
readelf -h /challenge/candidate_0
```

## Hints (open one at a time, stop as soon as you can continue)

1. Every candidate is "ELF 64-bit" and "x86-64", so those words cannot tell them apart. Look at the word that names the kind of file.
2. In `readelf -h`, the `Type:` line says `EXEC` only for an executable. `DYN` is a shared library and `REL` is a relocatable object.
3. Once you know which file is real, disassemble it with `objdump -d -M intel` and read `main`, as in `objdump-disasm`. Skip the `cmp` that checks whether reading your input worked.
