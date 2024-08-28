---
layout: default
title: Chiadika's Personal Projects
---

## Building an 8-bit computer from scratch

<br><br>

***Update: This project is unfinished and likely will remain so for a while.***
 
This project (unfinished and pictured below) is based off of and 
primarily inspired by Ben Eater's 
8-bit computer, 
which can be found 
[here](https://www.youtube.com/playlist?list=PLowKtXNTBypGqImE405J2565dvjafglHU).

The computer is made primarily of relatively simple parts (4-bit adders, 
basic logic gates, 4-bit registers, etc.) with the exception of 
times where building the component out of elementary parts would 
take an unreasonable amount of time or space to construct (such as the 
control logic).
<br><br>

![](https://chiadika.github.io/assets/8bit_unfinished.jpg)

Shown in the picture is the clock (upper left corner), RAM 
(directly below the clock), program counter (upper right 
corner), and the A and B registers and ALU (directly below the 
program counter). Additional content and schematics will be posted in 
the coming weeks as this project progresses.
<br><br>
<br><br>

### Some thoughts on instruction implementation with only 8 bits 
When I took EE306 (Intro to Computing) in Fall 2018 at UT, we worked 
with a simulated computer called the LC3 to introduce us to all of the 
elements present within computing. The LC3 was similar in structure to 
this computer; it utilizes a von Neumann architecture with a central 
bus and various components - like the ALU, registers, and RAM - 
connected to it. There's one important distinction, however - the LC3's 
bus is 16 bits wide, and its list of instructions looks something like 
this: 

![](https://chiadika.github.io/assets/LC3ISA.PNG)

This project doesn't have the flexibility for things like 9-bit 
offsets. While it certainly doesn't need the capability the LC3 
provides, having only 8 bits is still a potentially huge impediment. 
When scaling down from 16 to 8 bits, it's important to know what we want 
the computer to do, and what we want the computer to be capable of. For 
this project, I'd like to primarily focus on three things: opcode 
length and count, number of registers, and ranges of immediate values. 

### Opcodes
With instructions we want to primarily do 3 things: move data from 
memory to registers (and back), manipulate data, and control program 
flow. The LC3 had a 4-bit opcode and 15 different instructions; this 
computer will most likely have a 3-bit opcode and 8 different 
instructions. Knowing the capabilities of this machine, we can work off 
of the LC3 instruction set and remove what we don't need. 

Given the space of the memory (256 bytes), we probably won't need to 
have an indirect addressing mode, and we also probably won't be 
utilizing 
subroutines or TRAPs either; this reduces our list of instructions from 
15 to 8. 
Removing the NOT instruction (because our ALU is currently just an 
adder) puts us right at 7. This gives us the essential instructions with 
some room for an additional instruction that we might need to implement 
later, like MOV.  

### Registers and Immediate Values
The number of registers is incredibly important to any instruction set 
architecture. Especially at this scale, there's a tradeoff: since we 
have only a certain number of bits, having more registers (which 
requires more bits to represent in an instruction) limits the range of 
immediate values that we can utilize. This isn't as much of a problem in 
LC3 with its 16 instruction bits; if you want to add some value *n* to 
register R1 and store it in register R0, *n* isn't unreasonably limited. 
Given 4 bits for the opcode, 3 bits for each register, and a bit to 
indicate referencing an immediate value, you have 5 bits for *n*, 
meaning that *n*, if it's a signed number, can be anywhere from -16 to 15. If 
we had 8 registers (3 bits each) and tried to do the same, we couldn't 
even fit it all into an instruction; 3 bits for the opcode plus 3 bits 
for the source register and 3 bits for the destination register is 9 
bits. We can scale the number of registers down to 4, but it's still a 
bit limited; 3+2+2 is 7, meaning we only have 1 bit for an immediate 
value. Not a whole ton of space. The only obvious choice is to have two 
registers, allowing for 3 bits and a potential range of -4 to 3. Not 
blisteringly powerful, but it's the best we can do. We can also make the 
immediate value unsigned and negate it if needed later, making it an 
effective range of -7 to 7. 

There's a bit more flexibility with instructions that don't need two 
registers. An instruction that uses a PC-relative addressing mode with 
one register (like LD) has 4 bits to work with for the offset, which is 
an effective range of -8 to 7 from the PC without the potential trouble 
of negating the number. 

However, making the decision for just 2 registers isn't that cut and 
dry: if we have 4, for instance, we could potentially have two 
registers to store some special values (like a memory address with some 
important data we needed to reference over and over again, or a 
particular value that might be too large to put in a single instruction) 
and still have two registers that we could use to add, subtract, and store 
numbers. This could greatly reduce unnecessary memory accesses, code 
size, and execution time, but again we're at the earlier problem where if we want to add some value *n* to R0 
and put it in R1, we're limited to...0 or 1. But then again, we could 
just store *n* in another register, and it could be bigger than 4 or 7 
or whatever we were limited with before.

It's a rather difficult place to be. It's not quite like the scenario 
of the opcodes, where we know the computer is only capable of some range 
of things and can fit the specifications accordingly. It's more like two 
different and equally valid approaches to how we keep values, each with 
their own advantages and disadvantages. As the control logic comes 
together, I'll make a post about which route I'll choose.

___ 
## Schematics

_Coming soon!_
