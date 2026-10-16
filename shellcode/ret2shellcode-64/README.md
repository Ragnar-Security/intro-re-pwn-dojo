# Return to Shellcode (64-bit)

## Background

Do **Test Shellcode** first: this challenge reuses the shellcode you wrote there and adds the step of getting the program to jump to it. From the module's **"Start here"** resources you need:

- **"Using a leaked address to jump to your code"**: the stack picture, why ASLR forces you to read the address in the same run, and `p64()`.
- **"Executable memory and NX"**: why this only works because the stack is executable here.
- **"Bad bytes: newline and NUL"**: input arrives through `fgets`, which stops at the newline byte `0x0a`. That applies to your shellcode *and* to the address you append. NUL bytes are fine.
- From **Your First Memory Bug**: overwriting a saved return address, and measuring the distance to it with `cyclic` / `cyclic_find`.

Your shellcode has to fit **before** the saved return address, together with any padding. Print `len(sc)` and compare it with the distance you measure. Short shellcode is easier.

### How to start

```bash
/challenge/ret2shellcode-64                 # type a few letters; note the "buff sits at" line
/challenge/ret2shellcode-64                 # run it again: the address is different
checksec --file=/challenge/ret2shellcode-64 # NX disabled, no PIE, no canary
```

Then open it with `objdump -d -M intel /challenge/ret2shellcode-64` and find the function that calls `fgets`.

## The challenge

A **buffer overflow** happens when a program copies more input into a buffer than the buffer holds; the extra bytes spill into whatever sits next to it in memory. On the **stack** — the region where functions keep their local variables — the thing next to a local buffer is the **saved return address**: the spot the CPU jumps back to when the function finishes.

This program:

1. prints the **address of its own buffer** on the stack (a **leak** — normally that address is secret and randomized by **ASLR**, the address-space randomization the kernel applies on every run),
2. reads more into the buffer than the buffer can hold. How much more is a fact about this binary, not a secret: disassemble it (`objdump -d /challenge/ret2shellcode-64`) and subtract the buffer's stack offset (the `[rbp-N]` you see with the buffer) from the size passed to `fgets` — the difference is the number of bytes that spill past the buffer's reserved space. The distance from the buffer's *start* to the saved return address — the number your payload actually needs — you measure in Hint 2.

The binary was compiled with an **executable stack** (`-z execstack`; check with `checksec`: "NX disabled") and **no PIE**, so code you place in the buffer can actually be executed if you get the program to jump to it. That technique is called **ret2shellcode**: return into shellcode you planted.

Everything runs as **root** (the binary is setuid) and `/flag` is readable only by root. Goal: make the program run *your* code, and have that code print `/flag`.

The challenge is at `/challenge/ret2shellcode-64`.

`python3` with **pwntools**, and `checksec`, are already installed inside the challenge environment you run the dojo in — they may not exist on your own machine, so work there rather than assuming your laptop has them.

## Hints

<details>
<summary>Hint 1</summary>

Run it first. Two lines matter: the leaked buffer address, and the prompt asking for input. Note that the leaked address changes on every run — that is ASLR — but the *offset* from the buffer to the saved return address never changes, because it is decided by the program's layout at compile time.
</details>

<details>
<summary>Hint 2</summary>

Measure that offset yourself. pwntools' `cyclic()` generates a pattern where any 4-byte subsequence identifies its position; send a few hundred pattern bytes, let the program crash, and find the offset with `cyclic_find()`. Run the program *under gdb* to catch the crash (`gdb /challenge/ret2shellcode-64`, then `run`) — core files are often not written in containers, so don't rely on them.

One x86-64 wrinkle: the overwritten return address is an ASCII pattern, which is not a valid kernel address, so the CPU faults **on the `ret` instruction itself** — `$rip` shows the `ret`'s address, not your pattern, and `cyclic_find` on it fails. The overwritten value is the 8 bytes at the stack pointer at that moment: `x/gx $rsp` in gdb, then feed that value to `cyclic_find`.
</details>

<details>
<summary>Hint 3</summary>

Your payload: shellcode + padding up to the measured offset + the leaked buffer address (as 8 little-endian bytes, `p64()` in pwntools). The saved return address then points back into the buffer — at your shellcode. Keep the shellcode at the *start* of the buffer so you only need the one leaked address, and keep it short: after the jump, `push`es grow the stack *downward from just above the buffer*, so a long run of pushes walks back into the buffer and clobbers your code — starting the shellcode at the lowest address buys the most headroom.
</details>

<details>
<summary>Hint 4</summary>

`fgets` stops at a newline, so if the leaked address happens to contain the byte `0x0a`, the send is cut short. Don't fight it: just run the exploit again — a fresh run rolls a new address. (A handful of retries is normal.)
</details>

<details>
<summary>Hint 5 (stuck?)</summary>

For the shellcode itself, the same syscalls as the previous challenge work: open/read/write `/flag`, or `execve("/bin/sh")` after `setreuid(geteuid(), -1)`. pwntools' `shellcraft.cat("/flag")` is a complete open/read/write payload — assemble it with `asm()` and prepend the `setreuid` snippet so a spawned shell keeps root.
</details>
