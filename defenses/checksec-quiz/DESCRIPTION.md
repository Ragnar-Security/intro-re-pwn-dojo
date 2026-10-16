Three small programs sit in `/challenge`: `mystery-a`, `mystery-b`, and `mystery-c`. Each one was compiled with a different set of **defenses** (protections that make memory bugs harder to exploit).

Find out which defenses each program has, then run `/challenge/checksec-quiz` and answer its twelve questions: for each program, is **NX** enabled, does it use a **stack canary**, is it **PIE**, and which **RELRO** level does it have (none, partial, or full)? If every answer is right, the quiz prints the flag.

There is no bug to exploit here. This challenge is about reading a program's protections before you plan an attack.
