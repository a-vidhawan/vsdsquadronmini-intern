# Task 3:

## RISC V ISA:

RISC-V is a new instruction-set architecture that was originaly desgned to support computer architecture research and eductaion, but which now hopes to become an architecture for industry implementations as well.

A RISC-V hardware platform can contain one or more RISC-V-compatible processing cores together with other non-RISC-V-compatible cores, fixed-function accelerators, various physical memory structures, I/O devices, and an interconnect structure to allow the components to communicate.

A RISC-V ISA is defined as a base integer ISA, which must be present in any implementation, plus optional extensions to the base ISA. The base integer ISAs are very similar to that of the early RISC processors except with no branch delay slots and with support for optional variable-length instruction encodings.

## Types of Instructions:

We'll be taking a look at the RV32I base integer instruction set, for the RISC-V architecture, as that is what we mostly use throughout this internship.

For this specific architecture, instructions are 32-bit wide, and we have access to 32 unprivileged registers. Though, of these unprivileged registers, it is a standard to use x1 as a link register, x2 as the stack pointer and x5 as an alternate link register. The program counter has its own register.

RISC-V instructions can be split up into 5 main catrgories:
- R Type - Register Instructions
- I Type - Immediate Instructions
- S Type - Store Instructions
- B Type - Branch Instructions
- U Type - Upper-Immediate Instructions
- J Type - Jump Instructions

![](image.png)

### Register Type Intructions

Used for arithmetic and logical operations that involve registers

### Immediate Type Instructions

Instruction with an immediate (constant) value, typically used for operations that involve a constant operand, like load instructions or environment call/returns.

### Store Type Instructions

Used for store operations, where a value from a register is stored into memory.

### Branch Type Instructions

Conditional branch instructions, used to alter the flow of execution based on the result of a comparison.

### Upper-Immediate Type Instructions

Used for instructions that operate on upper immediate values, such as loading a 20-bit immediate into the upper bits of a register.

### Jump Type Instructions

Used for jump instructions, which are unconditional control transfers.
