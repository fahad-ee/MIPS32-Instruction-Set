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

# MIPS Arithmetic Instructions

## Overview
Arithmetic instructions perform basic mathematical operations like addition, subtraction, multiplication, and division. These instructions can operate on both signed and unsigned integers, with some variants handling overflow conditions.

---

## Instruction Table

| Instruction          | Example               | Type | Binary Representation (Highlighted)                     | Description                                                                 |
|----------------------|-----------------------|------|---------------------------------------------------------|-----------------------------------------------------------------------------|
| **add**             | `add $t0, $t1, $t2`   | R    | `000000 01001 01010 01000 00000 100000`                 | Adds two registers. On overflow, triggers an exception.                     |
| **sub**             | `sub $t0, $t1, $t2`   | R    | `000000 01001 01010 01000 00000 100010`                 | Subtracts two registers. On overflow, triggers an exception.                |
| **addi**            | `addi $t0, $t1, 100`  | I    | `001000 01001 01000 0000000001100100`                   | Adds a register and a 16-bit immediate value. On overflow, triggers an exception. |
| **addu**            | `addu $t0, $t1, $t2`  | R    | `000000 01001 01010 01000 00000 100001`                 | Adds two registers as unsigned integers. No overflow exception.             |
| **subu**            | `subu $t0, $t1, $t2`  | R    | `000000 01001 01010 01000 00000 100011`                 | Subtracts two registers as unsigned integers. No overflow exception.        |
| **addiu**           | `addiu $t0, $t1, 100` | I    | `001001 01001 01000 0000000001100100`                   | Adds a register and a 16-bit immediate value as unsigned. No overflow exception. |
| **mul**             | `mul $t0, $t1, $t2`   | R    | `011100 01001 01010 01000 00000 000010`                 | Multiplies two registers (32-bit result). No overflow handling.             |
| **mult**            | `mult $t1, $t2`       | R    | `000000 01001 01010 00000 00000 011000`                 | Multiplies two registers (64-bit result stored in `hi` and `lo`).           |
| **div**             | `div $t1, $t2`        | R    | `000000 01001 01010 00000 00000 011010`                 | Divides two registers (quotient in `lo`, remainder in `hi`).                |


## Examples

## 1. **add** 
**Example:** `add $t0, $t1, $t2`  
- **Action:** Adds the values in `$t1` and `$t2`, stores the result in `$t0`.  
- **Binary:** `000000 01001 01010 01000 00000 100000`  
  - `op=000000` (R-type), `rs=01001 ($t1)`, `rt=01010 ($t2)`, `rd=01000 ($t0)`, `funct=100000 (add)`.  
- **Execution:**  
  - If `$t1 = 5` and `$t2 = 3`, then `$t0 = 8`.  


### 2. **sub**  
**Example:** `sub $t0, $t1, $t2`  
- **Action:** Subtracts `$t2` from `$t1`, stores the result in `$t0`.  
- **Binary:** `000000 01001 01010 01000 00000 100010`  
  - `funct=100010 (sub)`.  
- **Execution:**  
  - If `$t1 = 5` and `$t2 = 3`, then `$t0 = 2`.  


### 3. **addi**  
**Example:** `addi $t0, $t1, 100`  
- **Action:** Adds `100` to `$t1`, stores the result in `$t0`.  
- **Binary:** `001000 01001 01000 0000000001100100`  
  - `op=001000 (addi)`, `imm=0000000001100100 (100)`.  
- **Execution:**  
  - If `$t1 = 200`, then `$t0 = 300`.  


### 4. **addu**  
**Example:** `addu $t0, $t1, $t2`  
- **Action:** Adds `$t1` and `$t2` as unsigned integers (no overflow exception).  
- **Binary:** `000000 01001 01010 01000 00000 100001`  
  - `funct=100001 (addu)`.  
- **Execution:**  
  - If `$t1 = 0xFFFFFFFF` and `$t2 = 1`, then `$t0 = 0x00000000` (wraps around).  


### 5. **subu**  
**Example:** `subu $t0, $t1, $t2`  
- **Action:** Subtracts `$t2` from `$t1` as unsigned integers.  
- **Binary:** `000000 01001 01010 01000 00000 100011`  
  - `funct=100011 (subu)`.  
- **Execution:**  
  - If `$t1 = 0` and `$t2 = 1`, then `$t0 = 0xFFFFFFFF`.  


### 6. **addiu**  
**Example:** `addiu $t0, $t1, 100`  
- **Action:** Adds `100` to `$t1` as an unsigned operation (no overflow).  
- **Binary:** `001001 01001 01000 0000000001100100`  
  - `op=001001 (addiu)`.  
- **Execution:**  
  - If `$t1 = 0xFFFFFF00`, then `$t0 = 0x00000064` (only lower 32 bits matter).  


### 7. **mul**  
**Example:** `mul $t0, $t1, $t2`  
- **Action:** Multiplies `$t1` and `$t2`, stores the lower 32 bits in `$t0`.  
- **Binary:** `011100 01001 01010 01000 00000 000010`  
  - Special `op=011100`, `funct=000010 (mul)`.  
- **Execution:**  
  - If `$t1 = 65536` and `$t2 = 65536`, then `$t0 = 0` (overflow ignored).  


### 8. **mult**  
**Example:** `mult $t1, $t2`  
- **Action:** Multiplies `$t1` and `$t2`, stores the 64-bit result in `hi` (high 32 bits) and `lo` (low 32 bits).  
- **Binary:** `000000 01001 01010 00000 00000 011000`  
  - `funct=011000 (mult)`.  
- **Execution:**  
  - If `$t1 = 65536` and `$t2 = 65536`, then `hi=0x00000001`, `lo=0x00000000`.  


### 9. **div**  
**Example:** `div $t1, $t2`  
- **Action:** Divides `$t1` by `$t2`, stores the quotient in `lo` and remainder in `hi`.  
- **Binary:** `000000 01001 01010 00000 00000 011010`  
  - `funct=011010 (div)`.  
- **Execution:**  
  - If `$t1 = 10` and `$t2 = 3`, then `lo=3`, `hi=1`.  

---

# MIPS Logical Instructions

## Overview
Logical instructions perform bitwise operations like AND, OR, shifts, and immediate variants. These are essential for manipulating data at the binary level.


## Instruction Table

| Instruction          | Example               | Type | Binary Representation (Highlighted)                     | Description                                                                 |
|----------------------|-----------------------|------|---------------------------------------------------------|-----------------------------------------------------------------------------|
| **and**             | `and $t0, $t1, $t2`   | R    | `000000 01001 01010 01000 00000 100100`                 | Bitwise AND of two registers.                                              |
| **or**              | `or $t0, $t1, $t2`    | R    | `000000 01001 01010 01000 00000 100101`                 | Bitwise OR of two registers.                                               |
| **andi**            | `andi $t0, $t1, 100`  | I    | `001100 01001 01000 0000000001100100`                   | Bitwise AND with a 16-bit immediate value.                                 |
| **ori**             | `ori $t0, $t1, 100`   | I    | `001101 01001 01000 0000000001100100`                   | Bitwise OR with a 16-bit immediate value.                                  |
| **sll**             | `sll $t0, $t1, 2`     | R    | `000000 00000 01001 01000 00010 000000`                 | Logical left shift by a constant (shamt).                                  |
| **srl**             | `srl $t0, $t1, 2`     | R    | `000000 00000 01001 01000 00010 000010`                 | Logical right shift by a constant (shamt).                                 |


## Examples

### 1. **and**  
**Example:** `and $t0, $t1, $t2`  
- **Action:** Performs bitwise AND between `$t1` and `$t2`, stores result in `$t0`.  
- **Binary:** `000000 01001 01010 01000 00000 100100`  
  - `op=000000` (R-type), `rs=01001 ($t1)`, `rt=01010 ($t2)`, `rd=01000 ($t0)`, `funct=100100 (and)`.  
- **Execution:**  
  - If `$t1 = 0b1100` and `$t2 = 0b1010`, then `$t0 = 0b1000`.  


### 2. **or**  
**Example:** `or $t0, $t1, $t2`  
- **Action:** Performs bitwise OR between `$t1` and `$t2`, stores result in `$t0`.  
- **Binary:** `000000 01001 01010 01000 00000 100101`  
  - `funct=100101 (or)`.  
- **Execution:**  
  - If `$t1 = 0b1100` and `$t2 = 0b1010`, then `$t0 = 0b1110`.  


### 3. **andi**  
**Example:** `andi $t0, $t1, 0xFF`  
- **Action:** Bitwise AND between `$t1` and immediate `0xFF` (masks lower 8 bits).  
- **Binary:** `001100 01001 01000 0000000011111111`  
  - `op=001100 (andi)`, `imm=0000000011111111 (255)`.  
- **Execution:**  
  - If `$t1 = 0x1234ABCD`, then `$t0 = 0x000000CD`.  


### 4. **ori**  
**Example:** `ori $t0, $t1, 0xFF00`  
- **Action:** Bitwise OR between `$t1` and immediate `0xFF00`.  
- **Binary:** `001101 01001 01000 1111111100000000`  
  - `op=001101 (ori)`, `imm=1111111100000000 (0xFF00)`.  
- **Execution:**  
  - If `$t1 = 0x00001234`, then `$t0 = 0x0000FF34`.  


### 5. **sll**  
**Example:** `sll $t0, $t1, 2`  
- **Action:** Shifts `$t1` left by 2 bits, stores in `$t0`.  
- **Binary:** `000000 00000 01001 01000 00010 000000`  
  - `rs=00000` (unused), `shamt=00010 (2)`, `funct=000000 (sll)`.  
- **Execution:**  
  - If `$t1 = 0b0001`, then `$t0 = 0b0100`.  


### 6. **srl**  
**Example:** `srl $t0, $t1, 2`  
- **Action:** Shifts `$t1` right by 2 bits (zero-fill), stores in `$t0`.  
- **Binary:** `000000 00000 01001 01000 00010 000010`  
  - `funct=000010 (srl)`.  
- **Execution:**  
  - If `$t1 = 0b1000`, then `$t0 = 0b0010`.  

---

# MIPS Data Transfer Instructions

## Overview
Data transfer instructions move data between registers and memory, or manipulate addresses. These include load/store operations and pseudo-instructions for address management.


## Instruction Table

| Instruction          | Example               | Type | Binary Representation (Highlighted)                     | Description                                                                 |
|----------------------|-----------------------|------|---------------------------------------------------------|-----------------------------------------------------------------------------|
| **lw**              | `lw $t0, 100($t1)`    | I    | `100011 01001 01000 0000000001100100`                   | Loads a word from memory at address `$t1 + 100` into `$t0`.                |
| **sw**              | `sw $t0, 100($t1)`    | I    | `101011 01001 01000 0000000001100100`                   | Stores a word from `$t0` to memory at address `$t1 + 100`.                 |
| **lui**             | `lui $t0, 0xABCD`     | I    | `001111 00000 01000 1010101111001101`                   | Loads the 16-bit immediate into the upper 16 bits of `$t0` (lower bits set to 0). |
| **la**              | `la $t0, label`       | P    | *Assembler expands to `lui` + `ori`*                    | Pseudo-instruction: Loads the address of `label` into `$t0`.                |
| **li**              | `li $t0, 100`         | P    | *Assembler expands to `addi` or `lui` + `ori`*          | Pseudo-instruction: Loads a 32-bit immediate into `$t0`.                   |
| **mfhi**            | `mfhi $t0`            | R    | `000000 00000 00000 01000 00000 010000`                 | Moves the value from `hi` register to `$t0`.                               |
| **mflo**            | `mflo $t0`            | R    | `000000 00000 00000 01000 00000 010010`                 | Moves the value from `lo` register to `$t0`.                               |

# MIPS Data Transfer Instructions

## 1. lw (Load Word)
**Example:** `lw $t0, 100($t1)`  
- **Type:** I-type  
- **Binary:** `100011 01001 01000 0000000001100100`  
- **Fields:**  
  - `op=100011` (lw)  
  - `rs=01001` ($t1)  
  - `rt=01000` ($t0)  
  - `offset=0000000001100100` (100)  
- **Action:** Loads word from memory[$t1 + 100] → $t0  
- **Execution:**  
  If $t1=0x1000 and memory[0x1064]=0x12345678 → $t0=0x12345678  

## 2. sw (Store Word)
**Example:** `sw $t0, 100($t1)`  
- **Type:** I-type  
- **Binary:** `101011 01001 01000 0000000001100100`  
- **Fields:** Same as lw  
- **Action:** Stores word from $t0 → memory[$t1 + 100]  
- **Execution:**  
  If $t0=0xABCD1234 and $t1=0x2000 → memory[0x2064]=0xABCD1234  

## 3. lui (Load Upper Immediate)
**Example:** `lui $t0, 0xABCD`  
- **Type:** I-type  
- **Binary:** `001111 00000 01000 1010101111001101`  
- **Fields:**  
  - `op=001111` (lui)  
  - `rs=00000` (unused)  
  - `rt=01000` ($t0)  
  - `imm=1010101111001101` (0xABCD)  
- **Action:** $t0 = imm << 16  
- **Execution:**  
  $t0 = 0xABCD0000  

## 4. la (Load Address)
**Example:** `la $t0, label`  
- **Type:** Pseudo (expands to lui+ori)  
- **Binary:** N/A (assembler generates actual instructions)  
- **Action:** Loads address of label → $t0  
- **Execution:**  
  If label is at 0x10004000 → $t0=0x10004000  

## 5. li (Load Immediate)
**Example:** `li $t0, 0x12345678`  
- **Type:** Pseudo (expands to lui+ori)  
- **Binary:** N/A (assembler generates actual instructions)  
- **Action:** Loads 32-bit immediate → $t0  
- **Execution:**  
  $t0 = 0x12345678  

## 6. mfhi (Move From HI)
**Example:** `mfhi $t0`  
- **Type:** R-type  
- **Binary:** `000000 00000 00000 01000 00000 010000`  
- **Fields:**  
  - `op=000000`  
  - `funct=010000` (mfhi)  
  - `rd=01000` ($t0)  
- **Action:** $t0 = hi  
- **Execution:**  
  If hi=0x00000001 → $t0=0x00000001  

## 7. mflo (Move From LO)
**Example:** `mflo $t0`  
- **Type:** R-type  
- **Binary:** `000000 00000 00000 01000 00000 010010`  
- **Fields:**  
  - `funct=010010` (mflo)  
- **Action:** $t0 = lo  
- **Execution:**  
  If lo=0xFFFFFFFF → $t0=0xFFFFFFFF  

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
