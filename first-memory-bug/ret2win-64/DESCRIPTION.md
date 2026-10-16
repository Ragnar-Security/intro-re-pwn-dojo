Same idea as `bof-baseline`, one step more realistic. This time the bug is not in `main()` but in a helper function, `vuln()`, and your input is read by `scanf("%s", ...)`, a very common way C programs read a word of text.

Run `/challenge/ret2win-64`. It asks for your name and greets you. Somewhere in the binary there is a `win()` function that prints the flag. Nothing calls it. Make `vuln()` return into it.

Two things are different from `bof-baseline`:

- **The function you overflow is `vuln()`.** Its saved return address points back into `main()`. Overwrite it with the address of `win()`.
- **`scanf("%s")` stops at whitespace.** `%s` reads one "word": it stops at a space, tab, or newline, and it never checks how big the buffer is (that is the bug). It does not stop at a zero byte, so zero bytes in your input are fine, but a whitespace byte in the middle of your input would cut it short. Send a newline at the very end so `scanf` knows your word is finished.

Terms from earlier challenges you will use here: **stack** (where local variables live), **buffer overflow** (writing past the end of a buffer), **saved return address** (where a function jumps back to when it finishes), **little-endian** (numbers stored lowest byte first), and **PIE** (this binary has none, so `win()` has a fixed address).

There is no stack canary. Your variant has its own buffer size, so work the numbers out from your binary.
