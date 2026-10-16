# ltrace-trace

## Background

`ltrace` runs a program and prints each **library call** (such as `strcmp`) with its arguments and return value. The lesson **"Library calls vs system calls: ltrace and strace"** walks through a real trace and shows how a `strcmp` line reveals both strings. `strace`, in the same lesson, shows the lower layer: system calls.

Start with the module's **Start here** lessons for the tools and vocabulary, then come back.

### How to start

```
/challenge/ltrace-trace
CLUB_TRACE_PASS=hello ltrace /challenge/ltrace-trace
```
Look for the `strcmp` line in the output. Remember that a traced run cannot print the flag.

## What this is about

Programs rarely do everything themselves. They call ready-made functions from a **library**, a shared collection of code; on Linux the C library provides functions like `printf` and `strcmp`. A call to one of those functions is a **library call**. `strcmp` compares two pieces of text (strings) and returns 0 when they are equal.

`ltrace` runs a program and prints each library call it makes, with the values passed in (the **arguments**) and the value returned.

An **environment variable** is a named value that a program can read when it starts. `NAME=value command` runs one command with that variable set.

## The challenge

`/challenge/ltrace-trace` reads the environment variable `CLUB_TRACE_PASS` and compares it with a secret text that it builds while running, so the secret never appears as one piece in the file. If they match, it prints the flag.

Under `ltrace`, the challenge runs without the special permission that lets it read the flag. So a traced run can show you the comparison but can never print the flag; run the challenge again without `ltrace` to collect it.

## Try it

```
/challenge/ltrace-trace
CLUB_TRACE_PASS=hello ltrace /challenge/ltrace-trace
```

## Hints (open one at a time, stop as soon as you can continue)

1. If the variable is not set at all, the program stops before it compares anything. Give it any value while you trace.
2. `ltrace -e strcmp` shows only `strcmp` calls. A line looks like `strcmp("hello", "THE_SECRET") = 1`: your guess is the first string, and the program's expected value is the second.
3. Run `/challenge/ltrace-trace` without `ltrace`, with `CLUB_TRACE_PASS` set to the complete expected text. Put it in single quotes so the shell passes it unchanged: `CLUB_TRACE_PASS='...' /challenge/ltrace-trace`.
