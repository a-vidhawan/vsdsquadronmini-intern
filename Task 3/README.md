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

![alt text](image-1.png)

- Used for arithmetic and logical operations that involve registers.
- Typically have 2 source and 1 destination register.
- Fields:
    - `opcode` : Operation code, used to determine category of operation
    - `rd` : Destination Register
    - `funct3` : 3 bits of function modifier to specify the kind of operation required
    - `rs1` : Source Register 1
    - `rs2` : Source Register 2
    - `funct7` : 7 bits of function modifier to specify the kind of operation required
- Example: `add a0, a5, a4`

### Immediate Type Instructions

![alt text](image-2.png)

- Instruction with an immediate (constant) value, typically used for operations that involve a constant operand, like load instructions or environment call/returns.
- Fields:
    - `opcode` : Operation code, used to determine category of operation
    - `rd` : Destination Register
    - `funct3` : 3 bits of function modifier to specify the kind of operation required
    - `rs1` : Source Register 1
    - `imm[11:0]` : 12 bits of immediate value
- Example: `addi a0, s5, 696`

### Store Type Instructions

![alt text](image-3.png)

- Used for store operations, where a value from a register is stored into memory.
- Fields:
    - `opcode` : Operation code, used to determine category of operation
    - `imm[4:0]` : Lower 5 bits of immediate value
    - `funct3` : 3 bits of function modifier to specify the kind of operation required
    - `rs1` : Source Register 1
    - `rs2` : Source Register 2
    - `imm[11:5]` : Upper 7 bits of immediate value
- Example: `sw a3, 0(a2)`

### Branch Type Instructions

![alt text](image-4.png)

- Conditional branch instructions, used to alter the flow of execution based on the result of a comparison.
- Fields:
    - `opcode` : Operation code, used to determine category of operation
    - `imm[11]` : Immediate value bit 11
    - `imm[4:1]` : Bits 4 to 1 of Immediate value
    - `funct3` : 3 bits of function modifier to specify the kind of operation required
    - `rs1` : Source Register 1
    - `rs2` : Source Register 2
    - `imm[10:5]` : Bits 10 to 5 of Immediate Value
    - `imm[12]` : Immediate Value Bit 12
- Example: `bne s1, s8, 1012c`

### Upper-Immediate Type Instructions

![alt text](image-5.png)

- Used for instructions that operate on upper immediate values, such as loading a 20-bit immediate into the upper bits of a register.
- Fields:
    - `opcode` : Operation code, used to determine category of operation
    - `rd` : Destination Register
    - `imm[31:12]` : 20 bit Upper Immediate Value
- Example: `lui s7, 0x21`

### Jump Type Instructions

![alt text](image-6.png)

- Used for jump instructions, which are unconditional control transfers.
- Fields:
    - `opcode` : Operation code, used to determine category of operation
    - `rd` : Destinatioon Register
    - `imm[19:12]` : Bits 19 to 12 of Immediate Value
    - `imm[11]` : Immediate Value Bit 11
    - `imm[10:1]` : Bits 10 to 1 of Immediate Value
    - `imm[20]` : Immediate Value Bit 20
- Example: `jal ra, 10584`

### Special Implementation

The No-Op instruction is crucial to facilitate pipelining of a procesor, and is hence a part of all ISAs today. For this architecture, the NOP Instruction is encoded as `addi x0, x0, 0`.

### Summary of 15 RISC-V Instructions with 32-Bit Encoding

| Assembly Instruction   | Instruction Type | Encoding (Hexadecimal)  |
|------------------------|------------------|-------------------------|
| `addi sp, sp, -80`     | I-Type           | `0xfb010113`            |
| `lui a0, 0x21`         | U-Type           | `0x00021537`            |
| `jal ra, 119c4`        | J-Type           | `0x79c010ef`            |
| `addiw s3, s0, 1`      | I-Type           | `0x0014099b`            |
| `beq s1, s8, 10178`    | B-Type           | `0X05848863`            |
| `mv a3, s0`            | R-Type           | `0x00040693`            |
| `li a3, 0`             | I-Type           | `0x00000693`            |
| `j 10120`              | J-Type           | `0xf79ff06f`            |
| `sd ra, 72`            | S-Type           | `0x04113423`            |
| `ld a0, 16`            | I-Type           | `0x0107b503`            |
| `bne s0, s3, 10244`    | B-Type           | `0xfd3416e3`            |
| `addiw s2, s2, 1`      | I-Type           | `0x0019091b`            |
| `jal ra, 11bf8`        | J-Type           | `0x311010ef`            |
| `mv a3, s0`            | R-Type           | `0x00040693`            |
| `addi a0, s5, 696`     | I-Type           | `0x2b8a8513`            |