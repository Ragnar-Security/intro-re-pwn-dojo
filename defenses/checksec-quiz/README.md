# checksec-quiz: hints

## Background

This challenge has no bug to exploit. You inspect three programs and answer a quiz about their defenses. You need to know four things, all explained with pictures in the module's **"Start here"** resources:

- **NX**: is the stack allowed to run as code?
- **Stack canary**: does a function check a secret value before returning?
- **PIE**: can the program's own code load at a different address each run?
- **RELRO**: how much of the table of library addresses is made read-only? (none, partial, or full)

The resource "Reading checksec, and checking by hand" shows real `checksec` output and how to confirm each answer with `readelf`. The three programs differ from each other, so check each one separately.

### How to start

```
ls -l /challenge
checksec --file=/challenge/mystery-a
/challenge/checksec-quiz
```

Run `checksec` on the other two programs, write the four answers for each in a table, then answer the quiz. To double-check a line, the hand method in the resource uses `readelf -lW`, `readelf -h`, and `readelf -d`.

Try the challenge first. Open one hint at a time and stop when you can continue on your own.

<details>
<summary>Hint 1: what are these defenses?</summary>

- **NX** (no-execute): memory that holds data, such as the stack, is marked as not runnable as code.
- **Stack canary**: a secret value placed on the stack before saved return information. The function checks it before returning and stops the program if it changed.
- **PIE** (position independent executable): the program can be loaded at a different address each run.
- **RELRO** (relocation read-only): after start-up, the table of library function addresses is made read-only. *Partial* protects part of it; *full* protects all of it.

The "Common Binary Defenses" resource for this module has a short summary.
</details>

<details>
<summary>Hint 2: which tool?</summary>

You do not need to read assembly. A tool called `checksec` reports all four defenses for a program. Try `checksec --help`. pwntools has one too: `pwn checksec <file>`.
</details>

<details>
<summary>Hint 3: reading the report</summary>

Run the tool once per program and write the four answers in a small table before you start the quiz. Each program has a different combination, so do not assume one program's answers apply to another. "No canary found", "NX disabled", and "No PIE" all mean the answer is **no**.
</details>

<details>
<summary>Hint 4: the quiz says one program is wrong</summary>

The quiz tells you which program has at least one wrong answer. Run the tool on that program again and compare each line with what you typed. RELRO has three possible answers, not two.
</details>
