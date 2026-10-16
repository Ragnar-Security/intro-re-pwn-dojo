# fmt-read: Hint Ladder

## Background
This challenge builds on `fmt-leak`, so do that one first. Read "Format markers, from zero" and "Where printf finds its values" in the module's "Start here" resources. Key facts:
- `%s` takes a value from the argument slots, treats it as an **address**, and prints the text found there.
- The argument slots include the stack, and your typed line is stored on the stack too.
- `%N$p` shows slot number N, which lets you find which slot starts your own text.

The guard tells you where the secret text is stored. The puzzle is how to make `%s` use that address.

## How to start
1. Run the challenge: `/challenge/fmt-read` and note the address the guard prints.
2. Type `AAAAAAAA` followed by several `%p` markers (`AAAAAAAA.%p.%p.%p.%p.%p.%p.%p.%p`).
3. Look for `0x4141414141414141` in the reply and count which marker printed it.

Try the challenge first. Open one hint at a time and stop when you have enough to continue.

## Words used here
- **Format string**: the first argument of `printf`. It is a template. Markers such as `%d` or `%s` tell `printf` to insert a value there.
- **`%s`**: a marker that treats its value as an *address* and prints the text found there.
- **`%p`**: a marker that prints its value as a pointer in hexadecimal (base 16, digits `0-9a-f`).
- **Stack**: the area of memory a running function uses for its local variables. Your typed line is stored there.
- **Little-endian**: a number is stored with its smallest byte first.

<details><summary>Hint 1: the unsafe repeat</summary>

Type `%p` and see what the guard prints back. It is filling the marker with something you did not give it.

</details>

<details><summary>Hint 2: what does %s need?</summary>

`%s` takes an address and prints the text stored there. If you can make `%s` pick up the address the guard told you, it prints the secret text. The question is how to put that address where `%s` will look.

</details>

<details><summary>Hint 3: your own input is on the stack</summary>

The line you type is saved on the stack, and the markers read values from the stack too. So the markers can read bytes you typed. Try typing `AAAAAAAA` followed by several `%p.%p.%p...` markers. The ASCII letter `A` is byte `0x41`, so look for `0x4141414141414141` in the output.

</details>

<details><summary>Hint 4: numbering the markers</summary>

`%7$p` shows argument number 7 directly. Find the number whose output is your `A` bytes. That tells you how many steps from `printf` your typed line begins.

</details>

<details><summary>Hint 5: the method</summary>

Build the line like this: a `%N$s` marker, padded so it takes exactly 8 bytes, then the address as 8 raw bytes in little-endian order. Choose N so that it points at the 8-byte slot that holds that address (the slot after the one where your line starts). Python's pwntools has `p64()` to turn an address into 8 bytes. Your line stops being read at a newline, so check the address bytes do not contain one.

</details>
