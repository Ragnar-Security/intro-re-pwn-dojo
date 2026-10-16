# inspector-tree

## Background

Every process has a PID and records its parent's PID (PPID). `ps` and `pstree` show these links. The lesson **"Watching a running process: ps, pstree, lsof, maps"** reads a real `ps` and `pstree` output column by column.

Start with the module's **Start here** lessons for the tools and vocabulary, then come back.

### How to start

```
/challenge/inspector-tree --daemon &
pid=$!
pstree -p $pid
```
`pstree -p $pid` draws the tree starting at the daemon, with PIDs in parentheses.

## What this is about

A **process** is a running program, and Linux gives each one a number called its **PID** (process ID). A process can start another process, called its **child**; the one that started it is the **parent**. Every process records its parent's PID, called its **PPID**. Following those links from parent to child gives a **process tree**.

## The challenge

`/challenge/inspector-tree --daemon` starts a **daemon**, a program that keeps running in the background. The daemon starts one child process and waits.

Find the child's PID and run `/challenge/inspector-tree <child-pid>` while both are still running. If that process's parent really is the daemon, it prints the flag.

## Try it

```
/challenge/inspector-tree --daemon &
pid=$!
echo $pid
ps -ef --forest
```

`&` runs a command in the background and gives you your prompt back; `$!` is the PID of the last background command, so `$pid` holds the daemon's PID. `ps -ef` lists processes with columns `UID PID PPID ... CMD`; `--forest` indents each child under its parent. `pstree -p` draws the same tree with PIDs in parentheses.

When you are finished, `kill $pid` stops the daemon (its child keeps running unless you kill it too).

## Hints (open one at a time, stop as soon as you can continue)

1. Find the daemon's line in the listing first: it is the one whose command ends in `--daemon`.
2. The child is the process whose PPID column equals the daemon's PID. It is not running the challenge program.
3. Submit the child's PID: not the daemon's PID and not your shell's. PIDs change whenever you restart the daemon.
