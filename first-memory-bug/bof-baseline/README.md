# bof-baseline: background and hints

## Background

This time there is no variable to change. You overwrite `main()`'s **saved return address**
so that `ret` jumps into `win()`. From the module's **Start here** lesson you need:

- **Concept 1, the stack frame:** above the buffer sit the saved `rbp` (8 bytes, at `[rbp]`)
  and then the return address (8 bytes, at `[rbp+8]`).
- **Concept 4, `ret`:** it loads whatever 8 bytes are at the top of the stack into `rip`,
  with no checks.
- **Concept 5, little-endian:** an address is 8 bytes; pack it with `p64(address)` or
  `struct.pack("<Q", address)`.

Steps 5 and 6 of the lesson's worked example do this to a toy program, with different
numbers. If you want to check your count by experiment, the cyclic pattern in Step 5 works
here too. Remember the x86-64 wrinkle: after a crash, gdb usually stops **at** the `ret`, and
the overwritten value is at `x/gx $rsp`, not in `rip`.

### How to start

```bash
checksec --file=/challenge/bof-baseline                    # no canary, no PIE: win() has a fixed address
nm /challenge/bof-baseline | grep win                      # where win() is
objdump -d -M intel /challenge/bof-baseline | more         # then type /<main>: and press Enter
```

---

## Hints

Open one hint at a time. Stop as soon as you can continue on your own.

---

## Hint 1: find win()

<details>
<summary>Show hint 1</summary>

`nm` lists the named functions and variables in a binary, with their addresses:

```bash
nm /challenge/bof-baseline | grep win
```

The first column is the address (in hex) of `win()`. The `T` means it is code. Write the address down; it goes at the end of your input.

</details>

---

## Hint 2: find the buffer

<details>
<summary>Show hint 2</summary>

```bash
objdump -d -M intel /challenge/bof-baseline | more
```

Type `/<main>:` and press Enter (Space shows the next page, `q` quits). Find `call ... <read@plt>`. A few lines before it, an `lea rax,[rbp-0x...]` computes the address that `read` writes your input to. That `[rbp-0x...]` is where the note buffer starts: so many bytes below `rbp`.

Use the number from the `lea`, not the buffer size you might guess from the prompt. The compiler often rounds a buffer's space up, and only the disassembly tells you the real position.

</details>

---

## Hint 3: what sits above the buffer

<details>
<summary>Show hint 3</summary>

`rbp` points at the top of `main()`'s own variables. Right at `rbp` is a saved copy of the caller's `rbp` (8 bytes). Right above that is the saved return address (8 bytes on a 64-bit system).

```
higher addresses
  [rbp+8]  saved return address   <- put win()'s address here
  [rbp]    saved rbp (8 bytes)    <- filler
  ...
  [rbp-B]  first byte of the note <- read() starts writing here
lower addresses
```

`read` fills memory from low addresses to high ones, so your bytes cover the buffer first, then the saved `rbp`, then the return address.

</details>

---

## Hint 4: count the filler

<details>
<summary>Show hint 4</summary>

Your filler has to cover everything from the start of the buffer up to (but not including) the saved return address. Using the diagram in hint 3, add up the bytes between `[rbp-B]` and `[rbp+8]`.

A common mistake is forgetting the 8 bytes of saved `rbp`: that puts `win()`'s address on top of the saved `rbp` instead of the return address.

</details>

---

## Hint 5: send the address as bytes

<details>
<summary>Show hint 5</summary>

x86-64 is **little-endian**: numbers are stored lowest byte first. An address is 8 bytes. `struct.pack("<Q", address)` turns a number into its 8 little-endian bytes (`Q` = 8-byte unsigned integer). The pwntools library offers the same thing as `p64(address)`.

```python
import struct, subprocess

filler = ...    # your answer from hint 4
win    = ...    # the address from hint 1, e.g. written as 0x40....

payload = b"A" * filler + struct.pack("<Q", win)
result = subprocess.run(["/challenge/bof-baseline"], input=payload, capture_output=True)
print(result.stdout.decode(errors="replace"))
```

If the program prints its goodbye message and nothing else, or crashes with no flag, your filler length is off. Re-check hints 2 to 4.

</details>
