# Linux C Debugging and Binary Analysis Lab

This repository demonstrates how a simple C program is compiled, executed,
disassembled, and debugged on a Linux system.

The goal of this project is to understand how high-level C code is translated
into assembly instructions and how the CPU executes those instructions at
runtime.

---

## Project Objectives
- Compile C programs using GCC
- Inspect ELF binaries using objdump
- Analyze assembly code (AT&T and Intel syntax)
- Debug programs using GDB
- Inspect CPU registers and memory
- Understand instruction flow and control structures

---

## Program Description
The program prints **"Hello, world!"** ten times using a loop.
Although simple at the source-code level, it provides a good example for
studying how loops, conditionals, and function calls are implemented at the
machine level.

---

## Compilation and Execution

```

#include <stdio.h>

int main()
{
        int i;
        for(i=0; i < 10; i++)                   //loop 10 times.
        {
                puts("hello, world!\n");        //ptus the string to the output.
        }
        return 0;                               //tells the OS the program exited without errors.
}
```
### How to run
```bash 
gcc simple.c
ls -l a.out
./a.out
```
### script
[simple.c](https://github.com/user-attachments/files/24804016/simple.c)

### Screenshots
<img width="1366" height="768" alt="simProg" src="https://github.com/user-attachments/assets/8750ba1e-41ca-4c5d-aedc-fe7f11fab052" />

### Binary Disassembly with Objdump

AT&T Syntax
```
objdump -D a.out | grep -A20 main.:
```
Intel Syntax
```
objdump -M intel -D a.out | grep -A20 main.:
```
Only the instructions related to the main() function are displayed to keep
the output readable
------
Intel syntax follows the format:
```
instruction <destination>, <source>
```
Example:
```
mov rbp, rsp
sub rsp, 0x10
```
## Screebshot
<img width="1366" height="768" alt="simpCompINT" src="https://github.com/user-attachments/assets/131696a4-9207-461c-8453-80149661cf68" />

---
### Debugging with GDB
GDB is used to inspect the program before execution begins.
```
gdb -q ./a.out
break main
run
info registers
```
A breakpoint is set at the start of main() so execution pauses before any
instructions are executed.

### CPU Registers Observed

- RAX, RBX, RCX, RDX – General-purpose registers

- RSP – Stack Pointer

- RBP – Base Pointer

- RSI / RDI – Source and Destination index registers

- RIP – Instruction Pointer
- EFLAGS – Status and comparison flags

The RIP register points to the current instruction being executed and is
critical for understanding control flow.

### Screenshot
<img width="1366" height="768" alt="simpGDB2" src="https://github.com/user-attachments/assets/7e712d45-46f8-4861-bef4-05623413a51a" />
<img width="1366" height="768" alt="simpGDB1" src="https://github.com/user-attachments/assets/4a9e0741-290b-46a4-b023-0c2794d4c591" />

---
### Compiling with Debug Symbols

Debug symbols are included using the -g flag:
```
gcc -g simple.c
gdb -q ./a.out
```
### This allows:

- Viewing source code in GDB
- Disassembling functions with context
- Stepping through instructions
  
### Screenshot
<img width="1366" height="768" alt="simpcom-1" src="https://github.com/user-attachments/assets/a00b6f4d-0444-48c4-96c4-80cfd1ed48e0" />
<img width="1366" height="768" alt="simpcom-" src="https://github.com/user-attachments/assets/943c1c54-3f4a-4077-82a1-20b48bf14ac4" />

---
### Memory Inspection and Endianness

Memory is examined using the x (examine) command
```
x/i $rip
x/8xb $rip
x/8xh $rip
x/8xw $rip
```
x86 systems use little-endian byte order, meaning the least significant
byte is stored first. This explains why values appear reversed when viewed
using different memory sizes.

### Screenshot<img width="1366" height="768" alt="simpcom shd " src="https://github.com/user-attachments/assets/cf6bede6-cfd4-481a-aced-0d231f9d506d" />
<img width="1366" height="768" alt="simpcomp wor" src="https://github.com/user-attachments/assets/cd9940ff-d416-4add-8e47-859b198006ef" />
<img width="1366" height="768" alt="simpcom int" src="https://github.com/user-attachments/assets/2a91f67a-7015-48e4-8622-74064cc39ade" />

---
### ASCII String Analysis

The string "Hello, world!\n" is stored directly in memory and passed to
printf().

```
x/6xb 0x555555556004
x/6ub 0x555555556004
x/6cb 0x555555556004
```
These bytes map to printable characters according to the ASCII standard:

```
man ascii
```
### screenshots
<img width="1366" height="768" alt="simpcomp ascii" src="https://github.com/user-attachments/assets/d589b3cf-141d-436c-88ab-736a4c04f069" />
<img width="1366" height="768" alt="man ascii2" src="https://github.com/user-attachments/assets/323698b0-f6b0-4469-ae7f-6ef7c2ceaab0" />
<img width="1366" height="768" alt="man ascii" src="https://github.com/user-attachments/assets/95b10ff1-ca89-40be-a224-499bbf15312d" />

---
### Tools Used

- GCC
- GDB
- Objdump
- Linux (Kali Linux)
---
### What I Learned

- How C code maps to assembly instructions
- How loops and conditionals work at machine level
- How the CPU uses registers and memory
- How to debug programs instruction-by-instruction
- Differences between AT&T and Intel assembly syntax
- How little-endian memory layout works

