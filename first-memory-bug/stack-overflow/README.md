# stack-overflow: background and hints

## Background

This is the first overflow in the module: you change a **variable**, not a return address.
You need four ideas from the module's **Start here** lesson:

- **Concept 1, the stack frame:** local variables sit at `[rbp-N]`. A smaller N is a higher
  address.
- **Concept 2, buffer overflow:** `read` fills the buffer from its lowest address upward and
  does not stop at the end of the buffer.
- **Concept 3, neighbouring variables:** bytes that run past the buffer land on the next
  variable up, and the program then uses your value.
- **Concept 5, little-endian:** the door code is a 4-byte number (`DWORD`), so pack it with
  `p32(value)` or `struct.pack("<I", value)`.

Step 4 of the lesson's worked example does the same thing to a toy program, with different
numbers. The method is in "The method, for any program in this module": the filler is the
buffer's distance below `rbp` minus the door code's distance below `rbp`.

### How to start

```bash
/challenge/stack-overflow                                  # run it once; type a short name
checksec --file=/challenge/stack-overflow                  # confirm: no canary, no PIE
objdump -d -M intel /challenge/stack-overflow | more       # then type /<main>: and press Enter
```

In `main`, find `call ... <read@plt>`, then work through the hints below.

---

## Hints

Open one hint at a time. Stop as soon as you can continue on your own.

The hints use `objdump`. If you prefer a graphical tool, Ghidra shows the same information.

---

## Hint 1: find where your input goes

<details>
<summary>Show hint 1</summary>

Print the disassembly and find the `main` function:

```bash
objdump -d -M intel /challenge/stack-overflow | more
```

Inside `more`, type `/<main>:` and press Enter to jump there. Space shows the next page and `q` quits.

Look for `call ... <read@plt>`. `read` copies bytes from your input into memory. It takes three arguments, which are set up in registers right before the call:

- `edi`: where to read from (`0` means your input, called standard input)
- `rsi`: where in memory to put the bytes (the badge buffer)
- `edx`: the maximum number of bytes to accept

</details>

---

## Hint 2: find the buffer's position

<details>
<summary>Show hint 2</summary>

`rbp` is a register that points at a fixed spot in the current function's stack area (its **stack frame**). Local variables are written as `[rbp-N]`: "N bytes below `rbp`".

A few lines before the `read` call there is an `lea rax,[rbp-0x...]`. `lea` computes an address without reading memory, and that address ends up in `rsi`. So that `[rbp-0x...]` is where the badge buffer **starts**. Write the number down.

Compare it with the `mov edx,...` value. Can `read` accept more bytes than the buffer has room for?

</details>

---

## Hint 3: find the door code

<details>
<summary>Show hint 3</summary>

After `read` returns, the program decides whether to open the door. Look for a `cmp DWORD PTR [rbp-0x...],0x...` instruction followed by a jump.

- `DWORD` means a 4-byte value. That 4-byte variable is the door code.
- The `[rbp-0x...]` part is where the door code lives.
- The last number is the value it is compared with: the value you need.

The door code is closer to `rbp` (a smaller N) than the buffer. Smaller N means a **higher** address, and `read` fills memory from low addresses to high ones. That is why overflowing the buffer reaches the door code.

</details>

---

## Hint 4: draw the stack

<details>
<summary>Show hint 4</summary>

Sketch the two variables using your numbers in place of `B` and `D`:

```
higher addresses
  [rbp-D]  door code (4 bytes)      <- you want your value here
  ...      unused gap
  [rbp-B]  first byte of the buffer <- read() starts writing here
lower addresses
```

The number of filler bytes is the distance from where `read` starts writing to where the door code starts. Work it out from `B` and `D`, not from how big the buffer was declared in C: the compiler may leave an unused gap between variables, and those bytes count too.

</details>

---

## Hint 5: write the value the way the CPU stores it

<details>
<summary>Show hint 5</summary>

x86-64 is **little-endian**: a multi-byte number is stored lowest byte first. The 4-byte value `0x11223344` sits in memory as the bytes `44 33 22 11`.

You cannot type raw bytes like that on a keyboard, so let Python build the input. `struct.pack("<I", value)` turns a number into its 4 little-endian bytes (`<` = little-endian, `I` = 4-byte unsigned integer):

```python
import struct, subprocess

filler = ...    # your answer from hint 4
value  = ...    # the number from the cmp in hint 3

payload = b"A" * filler + struct.pack("<I", value)
result = subprocess.run(["/challenge/stack-overflow"], input=payload, capture_output=True)
print(result.stdout.decode(errors="replace"))
```

If the door stays locked, read the door code it prints. If some of your `A` bytes (`0x41`) show up in it, your filler is too long. If your value appears only partly or not at all, re-check the distance from hint 4.

</details>
