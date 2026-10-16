# Intro to Reverse Engineering and Binary Exploitation

This is an optional practice dojo for UMD Cybersecurity Club members. It starts with small C and assembly exercises, then introduces tools for examining programs, simple memory bugs, shellcode, format strings, and common binary defenses.

You do not need previous reverse engineering experience. Work through the modules in order. Each challenge includes a short explanation and a hint ladder. Try a challenge before opening its hints, and stop reading when you have enough information to continue.

A **binary** is a compiled program. **Reverse engineering** means learning what a binary does by examining it instead of reading its source code. **Binary exploitation** means using a programming mistake to make a binary behave in an unintended way.

The workspace includes common tools such as `file`, `strings`, `objdump`, `checksec`, GDB with pwndbg, and Python with pwntools. Commands run in a Linux terminal. Challenge programs are 64-bit Linux ELF files.

These exercises are practice, not a grade. Ask for help when you get stuck, share techniques with other club members, and do not use these skills on systems without permission.
