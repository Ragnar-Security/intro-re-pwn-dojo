The club lab door asks you to scan your badge. It reads your badge name into a small piece of memory, then checks a secret door code. You never get to type the door code... or do you?

Run `/challenge/stack-overflow`. It reads your input and then opens the door (and prints the flag) only if a hidden number called the **door code** holds one exact value.

A few terms you will need:

- **Stack**: the area of memory where a function keeps its local variables. Variables that are declared next to each other usually end up next to each other on the stack.
- **Buffer**: a fixed-size block of memory that holds data, here an array of characters that holds your badge name.
- **Buffer overflow**: writing more bytes into a buffer than it can hold. The extra bytes do not disappear; they land on top of whatever sits next to the buffer in memory.
- **Disassembly**: the program's machine code shown as human-readable assembly instructions. `objdump -d -M intel /challenge/stack-overflow` prints it.

The bug: the program lets you send more bytes than the badge buffer can hold, and the door code lives right after the buffer. Send exactly enough filler bytes to fill the gap, then the door code's value.

When the door stays locked, the program tells you what the door code currently holds. Use that to check where your bytes landed.

Everything you need is in the binary. There is no stack canary (a guard value that would detect the overflow) and no PIE (addresses do not change between runs). Your variant has its own buffer size and its own door code, so work it out from your binary.
