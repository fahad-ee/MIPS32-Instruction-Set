# MIPS Instruction Set Tutorial

## Description
This repository provides a comprehensive tutorial on the MIPS (Microprocessor without Interlocked Pipeline Stages) instruction set architecture. MIPS is a reduced instruction set computer (RISC) architecture, widely used in academic settings and embedded systems. This tutorial covers the basics of MIPS instructions, including arithmetic, logical, data transfer, conditional branching, and system calls. It also explains the 32 general-purpose registers, instruction formats (R, I, and J types), and provides examples with binary representations for better understanding.

---

## MIPS Registers (32-bit)
MIPS has 32 general-purpose registers, each 32 bits wide. Below is a table summarizing their names and purposes:

| Register Number | Name    | Description                                                                 |
|-----------------|---------|-----------------------------------------------------------------------------|
| 0              | $zero   | Always holds the value 0. Writes to it are ignored.                         |
| 1              | $at     | Reserved for the assembler (used in pseudo-instructions).                   |
| 2-3            | $v0-$v1 | Used for function return values and expression evaluation.                  |
| 4-7            | $a0-$a3 | Used to pass the first four arguments to subroutines.                       |
| 8-15           | $t0-$t7 | Temporary registers. Not preserved across function calls.                   |
| 16-23          | $s0-$s7 | Saved registers. Preserved across function calls.                           |
| 24-25          | $t8-$t9 | Additional temporary registers.                                             |
| 26-27          | $k0-$k1 | Reserved for the operating system (kernel).                                 |
| 28             | $gp     | Global pointer (points to the middle of the global data area).              |
| 29             | $sp     | Stack pointer (points to the last location on the stack).                   |
| 30             | $fp     | Frame pointer (used for stack frame management).                            |
| 31             | $ra     | Return address (holds the address to return to after a function call).     |

---

## Instruction Formats
MIPS instructions are categorized into three types: R-type, I-type, and J-type. Each has a specific format:

### 1. R-Type (Register)
Used for arithmetic and logical operations where all operands are registers.

| Field   | Bits  | Description                          |
|---------|-------|--------------------------------------|
| op      | 6     | Opcode (0 for R-type).               |
| rs      | 5     | First source register.               |
| rt      | 5     | Second source register.              |
| rd      | 5     | Destination register.                |
| shamt   | 5     | Shift amount (for shift operations). |
| funct   | 6     | Function code (specific operation).  |

**Example:**  
`add $t0, $t1, $t2`  
- Binary: `000000 01001 01010 01000 00000 100000`  
  - `op=000000`, `rs=01001 ($t1)`, `rt=01010 ($t2)`, `rd=01000 ($t0)`, `shamt=00000`, `funct=100000 (add)`.

---

### 2. I-Type (Immediate)
Used for operations with immediate values, load/store, and branches.

| Field   | Bits  | Description                          |
|---------|-------|--------------------------------------|
| op      | 6     | Opcode (identifies the operation).   |
| rs      | 5     | Source or base register.             |
| rt      | 5     | Destination or target register.      |
| imm     | 16    | Immediate value or offset.           |

**Example:**  
`addi $t0, $t1, 100`  
- Binary: `001000 01001 01000 0000000001100100`  
  - `op=001000 (addi)`, `rs=01001 ($t1)`, `rt=01000 ($t0)`, `imm=0000000001100100 (100)`.

---

### 3. J-Type (Jump)
Used for unconditional jumps.

| Field   | Bits  | Description                          |
|---------|-------|--------------------------------------|
| op      | 6     | Opcode (identifies the jump).        |
| addr    | 26    | Target address (shifted left by 2).  |

**Example:**  
`j 1000`  
- Binary: `000010 0000000000000000001111101000`  
  - `op=000010 (j)`, `addr=0000000000000000001111101000 (1000)`.

---

## Instruction Categories

### 1. Arithmetic Instructions
Perform arithmetic operations like addition, subtraction, multiplication, and division.

| Instruction | Example          | Type | Binary Representation (Highlighted) |
|-------------|------------------|------|--------------------------------------|
| add         | `add $t0, $t1, $t2` | R    | `000000 01001 01010 01000 00000 100000` |
| sub         | `sub $t0, $t1, $t2` | R    | `000000 01001 01010 01000 00000 100010` |
| addi        | `addi $t0, $t1, 100` | I    | `001000 01001 01000 0000000001100100` |

**Example Execution:**  
`add $t0, $t1, $t2`  
- If `$t1=5` and `$t2=3`, `$t0` becomes `8`.

---

### 2. Logical Instructions
Perform bitwise operations like AND, OR, and shifts.

| Instruction | Example          | Type | Binary Representation (Highlighted) |
|-------------|------------------|------|--------------------------------------|
| and         | `and $t0, $t1, $t2` | R    | `000000 01001 01010 01000 00000 100100` |
| or          | `or $t0, $t1, $t2` | R    | `000000 01001 01010 01000 00000 100101` |
| sll         | `sll $t0, $t1, 2` | R    | `000000 00000 01001 01000 00010 000000` |

**Example Execution:**  
`and $t0, $t1, $t2`  
- If `$t1=0b1010` and `$t2=0b1100`, `$t0` becomes `0b1000`.

---

### 3. Data Transfer Instructions
Move data between registers and memory.

| Instruction | Example          | Type | Binary Representation (Highlighted) |
|-------------|------------------|------|--------------------------------------|
| lw          | `lw $t0, 100($t1)` | I    | `100011 01001 01000 0000000001100100` |
| sw          | `sw $t0, 100($t1)` | I    | `101011 01001 01000 0000000001100100` |
| lui         | `lui $t0, 100`   | I    | `001111 00000 01000 0000000001100100` |

**Example Execution:**  
`lw $t0, 100($t1)`  
- Loads the word at memory address `$t1 + 100` into `$t0`.

---

### 4. Conditional Branch Instructions
Control flow based on conditions.

| Instruction | Example          | Type | Binary Representation (Highlighted) |
|-------------|------------------|------|--------------------------------------|
| beq         | `beq $t0, $t1, label` | I    | `000100 01000 01001 0000000000000100` |
| bne         | `bne $t0, $t1, label` | I    | `000101 01000 01001 0000000000000100` |

**Example Execution:**  
`beq $t0, $t1, label`  
- If `$t0 == $t1`, jump to `label`.

---

### 5. Jump Instructions
Unconditional jumps.

| Instruction | Example          | Type | Binary Representation (Highlighted) |
|-------------|------------------|------|--------------------------------------|
| j           | `j label`        | J    | `000010 00000000000000000000000010` |
| jal         | `jal label`      | J    | `000011 00000000000000000000000010` |

**Example Execution:**  
`j label`  
- Jumps to `label`.

---

### 6. System Calls
Interact with the operating system.

| Service       | Example          | Code | Result           |
|---------------|------------------|------|------------------|
| print_int     | `li $v0, 1`      | 1    | Prints an integer. |
| exit          | `li $v0, 10`     | 10   | Terminates the program. |

**Example Execution:**  
`li $v0, 1` followed by `syscall` prints the integer in `$a0`.

---

## Conclusion
This tutorial covers the essential aspects of the MIPS instruction set, including registers, instruction formats, and examples. By following this guide, beginners can gain a solid understanding of MIPS assembly programming and implement these instructions in their projects. Happy coding!
