# inspector-strings

## Background

A compiled program keeps the text the programmer typed, such as messages and passwords, as plain readable bytes. `strings` prints every readable run of characters in a file without running it. Most lines are names of library functions; the lesson **"Reading a file without running it"** shows what a typical `strings` listing looks like and how to filter it with `grep`.

Start with the module's **Start here** lessons for the tools and vocabulary, then come back.

### How to start

```
/challenge/inspector-strings
strings /challenge/inspector-strings | grep -n -i club
```
Run the program once with no arguments to see what it asks for. Then look for a line you recognise from that message.

## What this is about

A **binary** is a compiled program: the file you run. Any fixed text the programmer typed into the source code, such as messages and passwords, is copied into the binary as plain bytes. The `strings` tool prints every run of readable characters it finds in a file, so it shows that text without running the program.

## The challenge

`/challenge/inspector-strings` contains one line of secret text that starts with `CLUB_DISCOVERY_`. Run the program with that whole line as its only **argument** (the words you type after the program name). If the text matches, it prints the flag.

Your copy of the challenge has its own secret text, so text from someone else's copy will not work.

## Try it

```
/challenge/inspector-strings
strings /challenge/inspector-strings | more
```

Inside `less`, type `/` and a word to search, `q` to quit.

## Hints (open one at a time, stop as soon as you can continue)

1. `strings` prints hundreds of lines, most of them names of library functions. You only need one of them.
2. Pipe the output into `grep` (`... | grep SOMETHING`) to keep only the lines that contain a word you already know is in the secret.
3. Copy the complete line, from the first character to the last, and put it in quotes after the program name.
