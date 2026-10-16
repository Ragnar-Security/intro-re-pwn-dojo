# fmt-leak: Hint Ladder

## Background
This challenge uses the bug explained in the module's "Start here" resources: the guard hands your text to `printf` as the format string. Read "Format markers, from zero" and "Where printf finds its values" first. Three facts are enough:
- A marker such as `%p` prints the next value, even if nobody passed one.
- Those missing values come from registers and then from the stack, where the guard keeps its own local variables.
- `%N$p` prints value number N directly, counting from 1.

The secret is a 64-bit number held in a local variable, so you can read it the same way the worked example reads its own variable.

## How to start
1. Run the challenge: `/challenge/fmt-leak`
2. At "Say something", type `hello`, then `%p`, and compare the two replies.
3. Type a run of `%p` markers (`%p.%p.%p.%p`), then longer ones, and look at what each value could be.

Try the challenge first. Open one hint at a time and stop when you have enough to continue.

## Words used here
- **Format string**: the first argument of `printf`. It is a template. Special markers such as `%d` or `%p` tell `printf` to insert a value there.
- **Argument**: a value `printf` fills a marker with. Normally the programmer passes them after the template.
- **Stack**: the area of memory a running function uses for its local variables.
- **`%p`**: a marker that prints a value as a pointer, in hexadecimal (base 16, digits `0-9a-f`, shown with a `0x` start).

<details><summary>Hint 1: what does "repeats" mean?</summary>

Type `hello` and look at the reply. Now type `%p` and look again. Did the guard repeat your text exactly?

</details>

<details><summary>Hint 2: where do the numbers come from?</summary>

You gave `printf` a marker but no value to fill it in with. `printf` does not know that, so it takes whatever value happens to be in the place where an argument would be. Part of that place is the stack of the function that called `printf`, which holds the guard's local variables.

</details>

<details><summary>Hint 3: looking at more than one value</summary>

You can type several markers at once, for example `%p.%p.%p`. Each marker shows the next value. Most will look like addresses or small numbers. You are looking for a long, random-looking 64-bit number.

</details>

<details><summary>Hint 4: picking one value directly</summary>

Writing `%7$p` means "show argument number 7". You can try argument numbers one after another instead of typing a very long line. The guard keeps asking, so you can try as many times as you like in one run, and the secret does not change during a run.

</details>

<details><summary>Hint 5: the method</summary>

Probe argument numbers in order (1, 2, 3, ...). Look for the one that is neither `(nil)`, a small number, nor an address that starts with `0x7f` or `0x40`. Type it, with or without the `0x`, at the "What is the secret" prompt. A disassembler or debugger can also show you how far the secret is from the start of the guard's local variables, but trying numbers works too.

</details>
