# EzASM - A Simpler Assembly Language

## Authors 
Trevor Brunette\
trevorbrunette@gmail.com

Kate Vandermolen\
CerulanLumina@users.noreply.github.com

## Scripts

### Program scripts for Linux:
`scripts/linuxBuild.sh` - uses the installed JDK (in `$PATH`) to compile the program and javadocs; also makes jar file.\
`scripts/linuxRun.sh` - runs the program with the installed JRE (in `$PATH`) using the given parameters.


### Program scripts for Windows:
`scripts/windowsBuild.cmd` - uses the installed JDK (in `%PATH%`) to compile the program and javadocs; also makes jar file.\
`scripts/windowsRun.cmd` - runs the program with the installed JRE (in `%PATH%`) using the given parameters.

## Introduction

The goal of this project is to create a small-instruction-set programming language interpreter written in Java with a GUI interface for inspecting the current state of the environment. This simple interpreted language would be able to demonstrate the concepts of a lower level assembly language while still being easier to write. The instructions would be intuitive and simple compared to MIPS (e.g., no system calls or immediate limits) and act upon virtual registers akin to other assembly languages.

The user is able to either run a file containing instructions (a program) or enter instructions line by line. The results of these instructions would be resultant on the GUI would list the state of all of the registers, the past/current/upcoming instructions, and the program memory.

### Development
This software was developed and tested using a 64-bit Arch Linux system with Java 17 (openJDK 17.0.4+8 x64)


## Implementation details:

8-byte words (64-bit integer, 64-bit float) \
Stack and Heap size: 8192 bytes (can be changed in program arguments)

Registers \
0 : ZERO \
1 : instruction line number \
2 : stack ptr \
3 : return ptr \
4-6 : arguments \
7-9 : return \
10-19 : save0 - save9 \
20-29 : temp0 - temp9 \
30-31 : special instructions 

Floating point registers \
0-9 : savef0 - savef9 \
10-19 : tempf0 - tempf9 \
20-21 : special instructions 
 

### Calling convention:
Register based similar to MIPS 

### Handling text:
First class support with embedding into data section \
C style strings (end with null byte) \
Register can have a char in it, or a pointer to a string 

### Instructions: 
Arithmetic \
`add` `sub` `mul` `div` `and` `or` `xor` `sll` `srl` - command (store register) (reg1/imm1) (reg2/imm2) \
`not` (store register) (reg1/imm1) \
`inc` `dec` (reg1, increments or decrements the value in that register by 1) \

`load` `store` heap-ish \
`mv` (reg1) (reg2) \
`load` (register) (ptr) \
`store` (register/immediate) (ptr) 

Function \
`call` (line number / label, equivalent to ‘jal’) \
`return` (0-3 registers / immediate arguments copied into return registers) 

Jump (and conditional jump) \
`j`(ump) (line number / label) \
`jal` (line number / label, puts line number in return register) \
`bne` `beq` `blt` `bgt` `ble` `bge` - command (reg1/imm1) (reg2/imm2) (line number / label) 

String manip (first argument is source, second is destination) \
`strlen` (ptr, stores value in return register) \
`strcpy` (ptr) (ptr) (second ptr buffer size) \
`strcat` (ptr) (ptr) \
`strcat` (ptr) (ptr) (number of characters) 

### System call instructions: 
`alloc` (register destination) (register/immediate number of words) \

`printi` (register/immediate, integer interpretation) \
`printf` (register/immediate, float interpretation) \
`printc` (register/immediate ASCII, char interpretation) \
`prints` (register/inline ptr to Cstring, string interpretation) \
`prints` (register/inline ptr to Cstring, string interpretation) (number of bytes to print) 

`readi` (register) \
`readf` (register) \
`readc` (register) \
`reads` (register with ptr) (size of buffer) 

`exit` (register/immediate return value) 

### Syntax:
Whitespace - `INSTRUCTION ARG1 ARG2 … <newline>` \
Delimiter - `INSTRUCTION ARG1, ARG2, … ;` \
replace syntax with whitespace at runtime \
Label - line only contains `LABEL_NAME:` \
Comment - line starts with `#` 
