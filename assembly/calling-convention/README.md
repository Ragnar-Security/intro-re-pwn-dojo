# calling-convention

## Background

When one function calls another, the two need an agreement about where the inputs go. On 64-bit Linux the agreement is called the **System V calling convention**. The first six whole-number arguments are passed in these registers, in this order:

| 1st | 2nd | 3rd | 4th | 5th | 6th | return value |
|---|---|---|---|---|---|---|
| `rdi` | `rsi` | `rdx` | `rcx` | `r8` | `r9` | `rax` |

For 32-bit values (`int`) the same registers are named `edi`, `esi`, `edx`, `ecx`, `r8d`, `r9d` (and `eax` for the return value). `edi` is not a different register from `rdi`, only its lower half (lesson page 2).

Two habits of compiled code matter here, and lesson page 6 ("The calling convention") shows both on real output:

- The caller may fill the registers in any order. **The register name tells you the argument's position**, not which `mov` came first.
- At `-O0`, the called function's first job is to copy each argument register into its own stack slot (`mov DWORD PTR [rbp-0x..], edi`). Those lines tell you which slot holds which argument.

## How to start

```
/challenge/calling-convention
objdump -d -M intel --disassemble=check /challenge/calling-convention
objdump -d -M intel --disassemble=main /challenge/calling-convention
```

## The challenge

`/challenge/calling-convention` reads six decimal numbers, one per line, and passes them to a function called `check` in a different order than you typed them. `check` has four tests; if all pass, the program prints the flag. Both the order and the test constants are different in every copy of the challenge.

## Hints (open one at a time, stop as soon as you can continue)

1. There are two functions to read: `<main>` and `<check>`. Start with `<check>` and write down its four tests. Name the arguments first, second, ... sixth.
2. At the top of `<check>`, each register is stored to a stack slot (`mov DWORD PTR [rbp-0x..], edi` and so on). That tells you which slot holds which argument. Then match each test to the slots it reads.
3. In `<main>`, each `scanf` call stores one of your numbers into one stack slot (the `lea` right before it names the slot). Calls happen in the order you type. Right before `call check`, a few `mov`s load slots into registers; some registers are copied into other registers, so follow them in order. Trace the register copies top to bottom: the last few `mov`s overwrite registers loaded a few lines above them, so read what each register holds at the moment of the `call`, not where it was first loaded.
4. Join the three tables: typing position, slot, register, test. Then pick values that satisfy each test. The tests show constants in hex but you must type decimal: `python3 -c 'print(0x1f4)'` converts one. Two of the tests relate two arguments, so you are free to choose one of them.
