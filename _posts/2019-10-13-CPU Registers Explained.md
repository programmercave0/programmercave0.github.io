---
layout: post
title: "CPU Registers Explained"
subtitle: "In computing, a processor or processing unit is an electronic circuit which performs operations on some external data source, usually memory or some other data stream. The term is frequently used to refer to the central processor (central processing unit) in a system. The Principal components of a CPU include the arithmetic logic unit (ALU) that performs arithmetic and logic operations, processor registers that supply operands to the ALU and store the results of ALU operations, and a control unit that orchestrates the fetching (from memory) and execution of instructions by directing the coordinated operations of the ALU, registers and other components. "
author: "Programmercave"
header-img: "assets/basicCPU.png"
tags:  [CPU Architecture, Computer Science]
date: 2019-10-13
---


In computing, a processor or processing unit is an electronic circuit which performs operations on some external data source, usually memory or some other data stream. The term is frequently used to refer to the central processor (central processing unit) in a system. The Principal components of a CPU include the arithmetic logic unit (ALU) that performs arithmetic and logic operations, processor registers that supply operands to the ALU and store the results of ALU operations, and a control unit that orchestrates the fetching (from memory) and execution of instructions by directing the coordinated operations of the ALU, registers and other components. 

![Basic CPU Diagram]({{ site.url }}/assets/basicCPU.png){:class="img-responsive"} 

Registers are like internal variable for the processor. Registers have small amount of storage but they are very fast. It is very time consuming to store immediate calculation results in main memory. The distance between the processor and main memory is far enough for the signal to take a significant time to get there. To get past this issue there are small amounts of memory stored inside the processor itself, these are called registers.

Almost all computers load data from a larger memory into registers where it is used for arithmetic operations. Processor registers are normally at the top of the memory hierarchy, and provide the fastest way to access data. 

A common property of computer programs is *locality of reference*, which refers to accessing the same values repeatedly and holding frequently used values in registers to improve performance; this makes fast registers and caches meaningful.

Registers are normally measured by the number of bits they can hold, for example, an "8-bit register", "32-bit register" or a "64-bit register" or even more. 

Here we are going to see about x*86* Processor. 8086 CPU was the first x*86* processor. It was developed and manufactured by Intel. The x86 architecture has 8 General-Purpose Registers (GPR), 6 Segment Registers, 1 Flags Register and an Instruction Pointer. 64-bit x86 has additional registers.
 <br/><input type="hidden" name="IL_IN_ARTICLE"> <br/>
**General Purpose Registers (GPR)**<br/>
The 8 General Purpose Registers are:<br/>

1. Accumulator register (*AX*): In this register intermediate arithmetic and logic results are stored. <br/>
2. Counter register (*CX*): It is used as counter in shift/rotate instructions and loops.<br/>
3. Data register (*DX*): It is used in I/O operations and arithematic operations.<br/>
4. Base register (*BX*): It is a pointer to data (located in segment register DS, when in segmented mode).<br/>
5. Stack Pointer register (*SP*). It stores the address of the most recent entry that was pushed onto the stack.<br/>
6. Stack Base Pointer register (*BP*). It points to the base of the stack and it is fixed for that stack while *SP* changes. <br/>
7. Source Index register (*SI*). It is a pointer to a source in stream operations.<br/>
8. Destination Index register (*DI*). It is a pointer to a destination in stream operations.<br/>

*AX, CX, DX,* and *DX* act as temporary variables for the CPU when it is executing machine
instructions. <br/>
*SP* and *BP* are important for program execution and memory management.<br/>
*SI* and *DI* point to the source and destination when data needs to be read from or written to.<br/>

The order in which they are listed is the same order that is used in a push-to-stack operation.<br/>

*AX*, *CX* are 16-bit notations. Here are 32 bit and 64 bit notations.

|---
| Register | Accumulator | Counter | Data | Base | Stack Pointer | Stack Base Pointer | Source |  Destination |
|-|-|-|-|-|-|-|-
| 16-bit | AX | CX | DX | BX | SP | BP | SI | DI |
|---
| 32-bit | EAX | ECX | EDX | EBX | ESP | EBP | ESI | EDI |
|===
| 64-bit | RAX | RCX | RDX | RBX | RSP | RBP | RSI | RDI |


**Segment Register**<br/>

Segment registers are basically *pointer* which points to a location where data is stored or code execution begins.<br/>
The 6 Segment Registers are: <br/>
1. Stack Segment (*SS*): It is a pointer to the stack.<br/>
2. Code Segment (*CS*): Machine instructions exist at some offset into a code segment. It is a pointer to the code.<br/>
3. Data Segment (*DS*): Variables and other data exist at some offset into a data segment. It is a pointer to the data.<br/>
4. Extra Segment (*ES*): It points to extra data ('E' stands for 'Extra').<br/>
5. F Segment (*FS*). It points to more extra data ('F' comes after 'E').<br/>
6. G Segment (*GS*). It points to still more extra data ('G' comes after 'F').<br/>


{% include ads.html %}<br/>
**EFLAGS Register**
The *EFLAGS* is a 32-bit register used as a collection of bits representing Boolean values to store the results of operations and the state of the processor. <br/>
The flags are grouped into:<br/>
	*Status Flags*: To monitor the kind of result produced from the execution of arithmetic instructions by evaluating the status flags of the EFLAGS register.<br/>
	*Control Flags*: There is only one control flag, named DF (Direction Flag). It is used only for string instructions.<br/>
	*System Flags*: It is used to monitor and control the overall operations. They controls I/O, maskable interrupts, debugging, task switching, and virtual-8086 mode.<br/>
 <br/><input type="hidden" name="IL_IN_ARTICLE"> <br/>
**Instruction Pointer**
The *EIP* register always contains the address of the next instruction to be executed. It can only be read through the stack after a `call` instruction. It cannot be changed or accessed directly. However, instructions that control program flow, such as calls, jumps, loops, and interrupts, automatically change the instruction pointer. <br/>

References:<br/>
[Processor (computing)](https://en.wikipedia.org/wiki/Processor_(computing))<br/>
[The processor and its components](https://en.wikibooks.org/wiki/A-level_Computing/AQA/Paper_2/Fundamentals_of_computer_organisation_and_architecture/The_processor_and_its_components)<br/>
[Processor register](https://en.wikipedia.org/wiki/Processor_register)<br/>
[Accumulator (computing)](https://en.wikipedia.org/wiki/Accumulator_(computing))<br/>
[What is a stack pointer used for in microprocessors?](https://stackoverflow.com/questions/1464035/what-is-a-stack-pointer-used-for-in-microprocessors)<br/>
[Segment Registers ](http://www.c-jump.com/CIS77/ASM/Memory/M77_0020_seg_reg.htm)<br/>
[http://www.c-jump.com/CIS77/ASM/Memory/M77_0120_reg_names.htm](http://www.c-jump.com/CIS77/ASM/Memory/M77_0120_reg_names.htm)<br/>
[FLAGS register](https://en.wikipedia.org/wiki/FLAGS_register)
[EFLAGS Register ](https://faculty.kfupm.edu.sa/COE/aimane/assembly/pagegen-31.aspx.htm)<br/>
[EIP Instruction Pointer Register](http://www.c-jump.com/CIS77/ASM/Instructions/I77_0040_instruction_pointer.htm)<br/>
[X86 Assembly/X86 Architecture](https://en.wikibooks.org/wiki/X86_Assembly/X86_Architecture)<br/>
[Hacking: The Art of Exploitation](https://amzn.to/35F91Wq)<br/>




