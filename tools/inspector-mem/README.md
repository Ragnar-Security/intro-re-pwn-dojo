# inspector-mem

## Background

A process only owns certain ranges of addresses, called **mappings**. `/proc/<pid>/maps` lists them, one per line, with start, end and permissions. The lesson **"Watching a running process: ps, pstree, lsof, maps"** explains each column and how to work out a mapping's size from `END - START`. A mapping with no name is **anonymous** memory.

Start with the module's **Start here** lessons for the tools and vocabulary, then come back.

### How to start

```
/challenge/inspector-mem
/challenge/inspector-mem --daemon &
pid=$!
cat /proc/$pid/maps
```
The first command prints the size to look for. `maps` does not print sizes: work each one out as `END - START`.

## What this is about

A **process** is a running program. Each process sees memory as one long numbered list of bytes; a byte's number is its **address**, written in **hex** (base 16: digits `0-9`, then `a-f`). A process does not use every address. It uses ranges called **mappings**: one for its code, one for its data, one for its stack, and so on. Linux lists every mapping of a process in the file `/proc/<pid>/maps`, where `<pid>` is the **process ID** (PID), the number Linux gives each running process.

A mapping is **anonymous** when it is plain memory, not a copy of a file, so its line in `maps` ends with no file name.

## The challenge

`/challenge/inspector-mem --daemon` starts a **daemon**, a program that keeps running in the background. It sets aside one anonymous mapping that is readable and writable and has an unusual size. Run `/challenge/inspector-mem` with no arguments to see that size for your copy.

Find that mapping in the daemon's memory map and run `/challenge/inspector-mem <start-address>` with the address where it begins. Keep the daemon running while you submit. If the address is right, it prints the flag.

## Try it

```
/challenge/inspector-mem
/challenge/inspector-mem --daemon &
pid=$!
cat /proc/$pid/maps
```

`&` runs a command in the background and gives you your prompt back; `$!` is the PID of the last background command. When you are finished, `kill $pid` stops the daemon.

Each line of `maps` looks like `START-END PERMISSIONS OFFSET DEVICE INODE NAME`. `PERMISSIONS` is four letters: `r` read, `w` write, `x` execute, then `p` for private.

## Hints (open one at a time, stop as soon as you can continue)

1. Read `maps` from the daemon's PID, not from your shell's. `cat /proc/self/maps` shows `cat` itself, which is the wrong process.
2. The size of a mapping is `END - START`. Python can do the subtraction: `python3 -c "print(0x7f0000005000 - 0x7f0000000000)"`. `pmap -x $pid` also prints sizes, in kilobytes (1 kilobyte = 1024 bytes).
3. Keep only the lines with no name at the end and `rw-p` permissions, then compare each size with the one the program printed. Submit the START of the matching line. The address changes every time you restart the daemon.
