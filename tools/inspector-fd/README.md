# inspector-fd

## Background

A **file descriptor** is the small number a process uses for a file it has opened. Linux lists them as links in `/proc/<pid>/fd/`; the target of each link is the file's path. The lesson **"Watching a running process: ps, pstree, lsof, maps"** shows a real listing, including what fd 0, 1 and 2 point at. `lsof -p <pid>` shows the same information.

Start with the module's **Start here** lessons for the tools and vocabulary, then come back.

### How to start

```
/challenge/inspector-fd --daemon &
pid=$!
ls -l /proc/$pid/fd
```
Ignore entries 0, 1 and 2; they are the terminal.

## What this is about

A **process** is a running program, and Linux gives each one a number called its **PID** (process ID). When a process opens a file, Linux gives it a small number called a **file descriptor** for that open file. Numbers 0, 1 and 2 are the standard input, output and error of the terminal; files the program opens itself get 3 and up.

Linux shows every file descriptor of a process as an entry in the folder `/proc/<pid>/fd/`. Each entry is a **symbolic link**: a small file that points at another path, the way a shortcut does.

## The challenge

`/challenge/inspector-fd --daemon` starts a **daemon**, a program that keeps running in the background. It creates a file with a new random name each time it starts, keeps that file open, and waits.

Find the full path of that file and run `/challenge/inspector-fd <path>` while the daemon is still running. If the daemon really has that file open, it prints the flag.

## Try it

```
/challenge/inspector-fd --daemon &
pid=$!
ls -l /proc/$pid/fd/
```

`&` runs a command in the background and gives you your prompt back; `$!` is the PID of the last background command. When you are finished, `kill $pid` stops the daemon.

## Hints (open one at a time, stop as soon as you can continue)

1. Look at the daemon's `fd` folder, not your shell's (`/proc/self/fd` is whatever program is reading it).
2. In `ls -l` output, the part after `->` is where a link points. `readlink /proc/$pid/fd/3` prints just that part.
3. Submit the path the link points to, starting with `/`, not the descriptor number. Restarting the daemon creates a new file name.
