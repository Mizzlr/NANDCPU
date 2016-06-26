# NAND CPU

A simple 16-bit architecture with only one instruction,
that is `NAND`

`NAND` operation take two bit and generates one bit as shown

|A | B | A NAND B |
|---|---|--- |
|0 | 0 | 1 |
|0 | 1 | 1 |
|1 | 0 | 1 |
|1 | 1 | 0 |


assembler.py generates machine code for the assembly code
emulator.py emulates our architecture in python runtime

``` sh
$ assembler.py input.asm machinecode.nand # to assemble
$ emulator.py machinecode.nand # to execute the code
```

examples directory has some example assembly code

This project is to show how a toy assembly language and 
machine architecture can be created with an added fun of 
doing it with just one instruction, NAND. NAND is a universal
boolean operation. Any boolean operation can be computed with
NAND

## The Architecture

Since, the machine is going to be 16-bit, there are 65536 cells
of memory array `MEM` addressed from 0x0000 to 0xFFFF. We are
going to need some registers to perform some actions. The 
registers are as shown in table below.

|REG | ADDR | Description |
|---|---|:--- |
|IP | 0x0000 | Instruction Pointer  |
|BP | 0x0001 | Base Pointer of the Stack |
|SP | 0x0002 | Stack Pointer at the top |
|SR | 0x0003 | Left cyclic Shift Register  |
|R0 | 0x0004 | Register 0 |
|R1 | 0x0005 | Register 1 |
|R2 | 0x0006 | Register 2 |
|R3 | 0x0007 | Register 3 |
|CS | 0x0008 | Code Segment Pointer |
|DS | 0x0009 | Data Segment Pointer |

So, the registers are memory mapped with main memory. This 
help simplify writing from and to registers. Also we will 
memory map some ports and write drivers for some I/O devices.
These drivers will run separately in tandem with emulator
at the runtime.

The memory layout is a shown below.

```
+------------------------------------------------+
| REGS | IVT | CODE | DATA ... ->    <- ... STACK|
+------------------------------------------------+
```

`REGS` are the 10 registers as shown above. `IVT` is the 
interrupt vector table. `CODE` is the segment of memory where
the machine code will be loaded. `DATA` is the heap memory
and `STACK` is the stack memory for function call. `IVT` will
contain pointers to some use ful Interrupt subroutines such as
to read input and write output, read and write file etc.

## Execution of an Instruction

An instruction in assembly would be `NAND op1, op2, res`
which would result in the action

`MEM[MEM[res]] <= NAND(MEM[MEM[op1]], MEM[MEM[op2]])`

That is `op1`, `op2` and `res` point to the cell that contains 
address of the operand 1, operand 2 and the resultant. The 
result is generated as 16-bit `NAND` operation of `op1` and `op2`

