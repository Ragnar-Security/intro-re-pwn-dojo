# endianness

## Background

A **byte** is the smallest piece of memory with its own address; two hex digits make one byte (for example `ef`). A 64-bit number takes 8 bytes. **Endianness** is the order those bytes are stored in memory. x86-64 CPUs are **little-endian**: the *least* important byte comes first. So the number `0x1122334455667788` is stored as the bytes `88 77 66 55 44 33 22 11`.

A **hex dump** shows memory as raw bytes, from the lowest address up. When a dump shows `88 77 66 55 ...`, the number is those bytes in reverse order. Reverse whole bytes (pairs of hex digits), never single digits.

Lesson page 7 ("Byte order") draws this out and shows the same 8 bytes in gdb both ways. Lesson page 10 explains how to read an `objdump -s` dump.

## How to start

```
/challenge/endianness
objdump -d -M intel --disassemble=main /challenge/endianness
objdump -s -j .rodata /challenge/endianness
```

The last command dumps the `.rodata` section (read-only data: the part of the file for constants) as rows. Each row starts with the address of its first byte, then 16 bytes in hex, then the same bytes as text.

## The challenge

`/challenge/endianness` keeps a secret 64-bit number in its read-only data. It asks for the secret as a hex number (no spaces; a leading `0x` is allowed). If you type the right number, it prints the flag.

## Hints (open one at a time, stop as soon as you can continue)

1. In `<main>`, find where the program loads the secret from memory into a register (`rax`). It loads from a fixed address written as a number.
2. Look that address up in the `.rodata` dump. The row's address is on the left; the bytes follow in order from low to high address. Count 8 bytes from your address (your address may fall in the middle of a row).
3. Those 8 bytes are laid out little-endian, but you need to type a normal number, so you need to change the order of the bytes.
4. Write the 8 bytes down, reverse their order, and join them into one hex number. Python can do it for you: `hex(int.from_bytes(bytes.fromhex("..."), "little"))`.
