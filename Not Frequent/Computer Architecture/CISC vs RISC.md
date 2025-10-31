
Many programming languages, some are high-level languages, other are low-level languages.
closed to an instruction set architecture, which defines:
- Supported Instructions
- Data Types
- Registers
- Addressing Modes

## Register - Memory Architecture
Operands of instructions can be immediates or contained in memory or in registers. (single memory access)
If all operands can be in registers or in memory (or in combinations) the architecture is called **register-plus-memory** (multiple memory access)
Access slows down the CPU -> tradeoff: power - speed

## Register - Register Architecture
Also called load-store architecture
Instructions divided into:
- **Memory access** --> load and store between memory and registers
- **ALU operations** --> only between registers
Operands of ALU instructions can be immediates or contained in registers

## CISC
Complex Instruction Set Computer
Main purpose is to complete a task in as few assembly lines as possible
This makes it complex on the compiler to decode difficult instructions
Common CISC CPUs: x86 (1978), x64 (2003) family from Intel and AMD
### Key Properties
- Length of instructions is variable (x86 family: 1-6 bytes)
- Processor architecture is more complex
- Number of clock cycles required for execution is variable
- small code size

## RISC
Reduced Instruction Set Computer
The main purpose is to make the processor as simple as possible
Common RISC CPUs: ARM family, MIPS family, RISC-V...
### Key Properties
- Instructions are simpler
- length of instructions is fixed
- processor architecture is simpler
- number of clock cycles required to execute instructions is normally lower than CISC
- Longer code length

## Examples
### x86 Code (CISC)

```
MOV AL, [SI]  
ADD AL, [BX]
```

1. load into AL register the value contained in memory location pointed by SI register
2. sum the value contained in the AL register with the value contained in the memory location pointed to by the SI register and save result in AL

### MIPS Code (RISC)

```
LB r1, 0(r0)
LB r2, 0(r0)
DADD r3, r2, r1
```

1. load into r1 the value contained in the memory location equal to the sum of the value contained in r0 and 0
2. same thing with r2
3. sum the value contained in r1 with the value in r2 and save in r3

## Coding Optimization for Register-Memory Architectures
### x86 Code

```
LEA BX, VAR
DEC [BX]
MOV AX, [BX]
```

1. load into register BX the memory address of VAR
2. decrement by 1 the value contained in memory location BX
3. load into register AX the value contained in BX

Memory Access: 2

```
LEA BX, VAR
MOV AX, [BX]
DEC [AX]
```

1. load into register BX the memory address of VAR
2. load into register AX the value contained in BX
3. decrement by 1 the value contained in memory location AX

Memory Access: 1

The order of instructions can affect the number of memory accesses
The more they increase the more it slows down the execution performance Load-Store architectures, by their nature, do not suffer from this problem
## Addressing Modes

#### 8086 Addressing Mode
1. Register Mode: `MOV AX, BX`
2. Immediate mode: `MOV AX, 2000`
3. Displacement or Direct Mode: `MOV AX, [DISP]`
4. Register Indirect Mode: `MOV AX, [DI]`
5. Based indexed mode: `MOV AX, [BX + DI]`
6. Indexed mode: `MOV AX, [SI + 2000]` 
7. Based mode: `MOV AX, [BP + 0100]`
8. Based indexed displacement mode: `MOV AX, [SI + BP + 2000]`
#### ARM Addressing Mode
- ALU Operations
	- Register Addressing Mode: `ADD r0, r1, r2`
	- Immediate Addressing Mode: `ADd r0, r1, #10`
- Load / Store Operations
	- Pre-indexed addressing `LDR r2, [r0, r1, LSL #1]`  `LDR r2, [r0, #4]!`
	- Post-indexed addressing `LDR r2, [r0], #4`
#### MIPS Addressing Mode
- ALU Operations
	- Register Addressing Mode: `ADD $t0, $t1, $t2`
	- Immediate Addressing Mode: `ADDI $t0, $t1, 8`
- Load / Store Operations
	- Base Displacement Addressing Mode `LW $t1, 4($t2)` `LW $t1, variable($t2)` 

