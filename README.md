# Vsd_SquadronMini-internship

This VSDSquadron Mini-Internship program introduces participants to the world of VLSI chip design and RISC-V architecture using open-source tools. It provides a practical, hands-on learning experience centered around the VSDSquadron Mini RISC-V Development Board.The development board is equipped with a 32-bit RISC-V core, 16KB of flash memory, and 2KB of SRAM, operating at a clock speed of 24MHz. It features 15 GPIO pins for hardware interfacing and supports widely-used communication protocols such as I2C, SPI, and USART, making it versatile for a range of projects. These capabilities allow participants to build and prototype hardware applications while deepening their understanding of the RISC-V ecosystem.
<details>
<summary><b>Task 1:</b> Installing RISC-V toolchain and Refer to C and RISC-V based lab videos </summary>   
<br>

 C-Based Lab
 ----
Install leafpad editor for C programming using command

 ```
         sudo apt  install leafpad
 ```
 ![install leafpad](https://github.com/user-attachments/assets/69a4702e-69e4-494d-8bb0-4a9f347eee5b)

Write a program that gives the sum of n numbers using C in leafpad editor."sum1ton.c" is the filename

 ![overleaf sum1ton](https://github.com/user-attachments/assets/9b683e34-7296-4785-889d-fc1b85038656)

 After Compiling C code save ``ctrl+s`` and close the window ``ctrl+w`` 

 Run the program and check the results using commands
 ````
gcc sum1ton.c
./a.out 
````
./a.out is used for result

result :

![c result ](https://github.com/user-attachments/assets/78b285ae-7b20-4409-acd1-4d8e9ebccf8c)

RISC -V Based Lab
----
Now we are compiling the same code in RISCV 
compiling using command ```cat sum1ton.c```

![risc1](https://github.com/user-attachments/assets/5e31f14d-e508-4d95-bd6d-167a52d6333a)

For compiling the above C code in RISCV use command 
```
 riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
```
Command breakdown : 

``riscv64-unknown-elf-gcc``: This is the cross-compiler for RISC-V architecture targeting 64-bit systems ``riscv64`` and producing ELF (Executable and Linkable Format) binaries.

`-O1`:Enables level 1 optimizations, focusing on basic optimizations to improve performance without excessive compile-time or memory usage.

`-mabi=lp64`:Specifies the application binary interface (ABI) for the target.
`lp64` means:
"long" and pointers are 64 bits.
Integer types are kept at 32 bits (e.g., int is 32 bits).
This is the standard ABI for 64-bit RISC-V systems

`-march=rv64i` : Specifies the target architecture.

`-o sum1ton.o`: Specifies the output file's name `sum1ton.o`, which will be the compiled object file.

After this open a new tab and give command as `` riscv64-unknown-elf-objdump -d sum1ton.o | less``

After compiling in RISC we will get the Assembly language code of it and we will seaarch for main part of the code using ``/main``

and the Assembly language for main function of the code is as :
![risc2](https://github.com/user-attachments/assets/a8e2d864-cbe7-44a6-9e9c-eb020d99e489)

In this Assembly language code there are 11 instructions using -O1 

Now we will check number of instructions using ``-Ofast``

![main o-fast](https://github.com/user-attachments/assets/ae45d301-d528-4fd0-894f-142e972ab70b)

Even in this case there are 11 instructions 

What is the Difference between -O1 and -Ofast ?
-
The `-O1` and `-Ofast` options in the context of compiling C code with RISC-V (or any GCC-compatible compiler) control the level and type of optimizations applied during the compilation process

In the case of `-O1` it  focus  A moderate level of optimization that aims to improve code performance while keeping compilation times and memory usage reasonable.

In the case of `-Ofast` it focus  Aggressively optimizes for maximum performance, often at the expense of strict adherence to the language standard and longer compilation times.

</details>

<details>
<summary><b>Task 2:</b> Performing SPIKE simulation and Debugging the C code Using spike  </summary>   
<br>
 
What is Spike Simulation?
--------- 
`Spike` is the official RISC-V ISA (Instruction Set Architecture) simulator. It allows developers to simulate RISC-V programs and applications, providing an environment to run, test, and debug code designed for RISC-V-based processors. Spike is commonly used in the context of development and research related to RISC-V architecture.

`GCC (GNU Compiler Collection)` is a popular set of compilers that supports a variety of programming languages, including C and C++. In the context of RISC-V, GCC can be used to compile code for RISC-V targets, and Spike can then simulate the execution of that code on a virtual RISC-V machine.

Now we wil use the command `spike pk sum1ton.o` give the output of the C code and verifies the instructions are correct

![Screenshot from 2024-11-25 11-42-08](https://github.com/user-attachments/assets/e369d5bb-0bb3-4ec5-92a7-fdf1847afa2e)

Debugging the Assembly Language Program using ``spike -d pk sum1ton.o`` in a new terminal window.

Assembly Language Program :

![assemblylang](https://github.com/user-attachments/assets/75bdb822-76cd-4b17-8913-4f573dcbfdd4)

Debugger:

![match sp](https://github.com/user-attachments/assets/1d6583a8-e92a-4ae1-a792-40461e99a1e4)

In Debugger we Debug the Assembly Language by following the each instruction .At the address of `100b4` the register value of stack point `sp` is `0x0000003ffffffb50` and after completion of instruction`sp, sp, -16` ,the new value of register stack pointer is `0x0000003ffffffb40`

`lui` 
Load Upper Immediate:

![Screenshot 2024-11-25 154052](https://github.com/user-attachments/assets/8e55efa5-e3c8-4f87-bc37-91a4d644515a)

 Instruction: `lui a0, %hi(LC1)`
LUI is an instruction in the RISC-V architecture that loads a 20-bit immediate value into the upper 20 bits of a 32-bit or 64-bit register. The lower 12 bits of the register are set to zero.
In the example, the instruction loads the upper 20 bits of a label (LC1) into the register a0

` addi`
Add Immediate
![image](https://github.com/user-attachments/assets/e8b72f51-cee7-4706-9fec-226a7d1eb7e9)

 Instruction:` addi a0, a0, %lo(LC1)`
Purpose: The ADDI instruction adds an immediate value (12-bit constant) to the value in a source register (rs1) and stores the result in a destination register (rd).

![final](https://github.com/user-attachments/assets/f055f819-bad6-426d-a04a-05fd96a704ee)

After  finishing all the instructions in Assembly language ,At the address of `100d8` it returns the value of sum.

task : Wite a simple C program for any application and compile using RISC -V GCC/SPIKE

Application:
--

Countdown Counter :
--
The countdown counter is a program that begins from a specified value and decrements it by one at regular intervals until it reaches zero.

We want to create a program that:

1.Initializes a timer with a starting value (e.g., 10 seconds).

2.Prints the current countdown value.

3.Decrements the timer every second.

4.Stops when the timer reaches zero.

C-program using Leafpad :
-
![counterdown leafpad](https://github.com/user-attachments/assets/5eb13062-6041-481f-9971-946d5169e903)

output of C code is :

![c out counterdown](https://github.com/user-attachments/assets/9753cb5a-5902-45a3-959a-8a3c3c8ac925)

compilation using gcc :

![counterdown using gcc](https://github.com/user-attachments/assets/6c1c059b-38ec-45b6-98ee-10562faaaff7)

Assembly Language program for the above C code: 


![Assembly language counterdown](https://github.com/user-attachments/assets/e42643af-fc6c-4bcb-9d62-acd4bdd7d234)

Debugging all the instructions in the Assembly language program using spike 

![debug using spike](https://github.com/user-attachments/assets/421d1e4f-ab7c-42fe-8ca9-2897a80975da)

Debugging the Assembly Language instructions :

| **Address** | **Instruction**          | **Explanation**                                    |
|-------------|--------------------------|----------------------------------------------------|
| `101c4`     | `addi sp, sp, -32`       | Allocate 32 bytes on the stack.                   |
| `101c8`     | `sd ra, 24(sp)`          | Save return address (`ra`) on the stack.          |
| `101ca`     | `sd s0, 16(sp)`          | Save register `s0` on the stack.                  |
| `101cc`     | `sd s1, 8(sp)`           | Save register `s1` on the stack.                  |
| `101ce`     | `sd s2, 0(sp)`           | Save register `s2` on the stack.                  |
| `101d0`     | `lui s2, 0x21`           | Load upper immediate `0x21` into `s2`.            |
| `101d4`     | `addi s2, s2, 720`       | Add `720` to `s2`.                                |
| `101d8`     | `mv a0, s2`              | Move the value of `s2` to `a0` (argument for `printf`). |
| `101dc`     | `jal ra, 1047c <printf>` | Call the `printf` function.                       |
| `101e0`     | `addi ra, 10184 <delay>` | Load the address of `delay` into `ra`.            |
| `101e4`     | `addiw s0, s0, -1`       | Decrement `s0` by 1.                              |
| `101e6`     | `bne s0, s1, 101e4`      | Branch to `101e4` if `s0` is not equal to `s1` (loop condition). |
| `101e8`     | `lui a0, 0x21`           | Load upper immediate `0x21` into `a0`.            |
| `101ea`     | `addi a0, a0, 744`       | Add `744` to `a0`.                                |
| `101ec`     | `jal a0, 105a0 <puts>`   | Call the `puts` function.                         |
| `10200`     | `ld ra, 24(sp)`          | Restore return address (`ra`) from the stack.     |
| `10202`     | `ld s0, 16(sp)`          | Restore register `s0` from the stack.             |
| `10204`     | `ld s1, 8(sp)`           | Restore register `s1` from the stack.             |
| `10206`     | `ld s2, 0(sp)`           | Restore register `s2` from the stack.             |
| `1021e`     | `addi sp, sp, 32`        | Deallocate 32 bytes from the stack.               |
| `10220`     | `ret`                    | Return to the caller.                             |



</details>

<details>
<summary><b>Task 3:</b> Various RISCV instruction type and 32 bit instruction code for instructions from application code  </summary>   
<br>

RISCV Instruction types
--

There are 6 types of instruction types in RISCV ISA
 1.  R-Type (Register Type)
 2.  I-Type (Immediate Type)
 3.  S-Type (Store Type)
 4.  U-Type (Branch Type)
 5.  B-Type (Upper Immediate Type)
 6.  J-Type (Jump Type)

In the base RV32I ISA, there are four core instruction formats (R/I/S/U), as shown in Base instruction formats. All are a fixed 32 bits in length.

![image](https://github.com/user-attachments/assets/47b33518-df07-42ce-9922-4530c16492e9)

1.R-Type:
--
![image](https://github.com/user-attachments/assets/6bcad23c-667d-4fa1-ba98-e5c6d82f3b12)

  This diagram represents the R-Type instruction format in the RISC-V Instruction Set       
    Architecture (ISA). R-Type instructions are typically used for register-to-register operations

1. Opcode (bits 6-0):

   The 7-bit opcode identifies the type of operation and the instruction format. For R-Type instructions, the opcode specifies that the instruction is register-based.

2. rd( bits 11:7):
   This bit is used for designation register where the output of the operation is written.
3. funct3( bits 14:12) :
   This 3 bit is used for differentiate between categories of operations within the same opcode.
   R type operations:
   
| **funct3** | **Operation**                      |
|------------|------------------------------------|
| `000`      | Add / Sub (depends on `funct7`)   |
| `001`      | Shift Left Logical (SLL)          |
| `010`      | Set Less Than (SLT)               |
| `011`      | Set Less Than Unsigned (SLTU)     |
| `100`      | XOR                               |
| `101`      | Shift Right (Logical/Arithmetic; depends on `funct7`) |
| `110`      | OR                                |
| `111`      | AND                               |

4. rs1(bits 19:15) :
 It specifies the first source register for the operation.
5. rs2(bits 24:20) :
 It specifies the second source register for the operation.
6. funct7(bits 31:25) :
 It provides additional differentiation between instructions that use the same opcode and fuct3.

Examples for R Type operation.  

| **funct7**  | **funct3** | **Operation**                        |
|-------------|------------|--------------------------------------|
| `0000000`   | `000`      | Add                                 |
| `0100000`   | `000`      | Sub                                 |
| `0000000`   | `001`      | Shift Left Logical (SLL)            |
| `0000000`   | `010`      | Set Less Than (SLT)                 |
| `0000000`   | `011`      | Set Less Than Unsigned (SLTU)       |
| `0000000`   | `100`      | XOR                                 |
| `0000000`   | `101`      | Shift Right Logical (SRL)           |
| `0100000`   | `101`      | Shift Right Arithmetic (SRA)        |
| `0000000`   | `110`      | OR                                  |
| `0000000`   | `111`      | AND                                 |

2.I-Type :
--

![image](https://github.com/user-attachments/assets/291e132c-afb6-46c2-a34c-9e46c2a845de)

I-Type instructions are used for operations involving immediate values, such as arithmetic with constants, memory access (e.g., loads), and control flow (e.g., jumps).

Breakdown of the Fields:
-
1. opcode( bits 6:0) :
 This 7 bits are used to identify the general operation type 
 
2. rd (bits 11:7) :
 It specifies the Destination register which is used to store the result of operation

3. funct3(bits 14:12) :
 It specifies the operation to perform such as load , immediate arthematic etc.,

4. rs1 (bits 19 :15 ) :
specifies the source register for the operation. For example, it provides the base address for memory instructions or a source operand for arithmetic operations.

5. imm[11:0] ( bits 31:20) :
 This 12-bit immediate value is sign-extended and used directly as part of the operation.
It serves as a constant operand for immediate operations or an offset for memory access.

Common I -Type instructions :
-
| **Instruction** | **opcode** | **funct3** | **Description**                       |
|-----------------|------------|------------|---------------------------------------|
| `addi`          | `0010011`  | `000`      | Add immediate to register (`rd = rs1 + imm`). |
| `slti`          | `0010011`  | `010`      | Set if less than immediate (signed). |
| `andi`          | `0010011`  | `111`      | Bitwise AND with immediate.          |
| `lw`            | `0000011`  | `010`      | Load word from memory.               |
| `lh`            | `0000011`  | `001`      | Load halfword from memory.           |
| `jalr`          | `1100111`  | `000`      | Jump and link register (indirect jump). |

3.S-Type:
-
![image](https://github.com/user-attachments/assets/e69f40bc-c431-4334-b77a-a5f1286a4431)

 S-Type instructions are primarily used for store operations, where data from a register is stored into memory at a specified address.

1. opcode (bits 6:0) :
 It identifies the general operation

2. imm[4:0] (bits 11:7) :
 Lower 5 bits of the 12-bit immediate (offset)

3. funct3 (bits 14:12) :
 specifies the type of store like word, byte,halfword etc.,

4. rs1 (bits 19 :15 ) :
 specifies the first source register for the operation.

5. rs2 (bits 24:20) :
 specifies the source register containing the value to be stored in memory.

6. imm[11:5] (bits 31:25) :
  Upper 7 bits of the 12-bit immediate (offset).

Common S-Type Instructions
-
| **Instruction** | **opcode**  | **funct3** | **Description**                      |
|-----------------|-------------|------------|--------------------------------------|
| `sw`           | `0100011`   | `010`      | Store Word (32-bit).                |
| `sh`           | `0100011`   | `001`      | Store Halfword (16-bit).            |
| `sb`           | `0100011`   | `000`      | Store Byte (8-bit).                 |

4.U-Type :
-
![image](https://github.com/user-attachments/assets/fca113b5-0577-4955-a24f-d5454a9aa0a6)

U-Type format is used for instructions like LUI (Load Upper Immediate) and AUIPC (Add Upper Immediate to PC)

1.opcode(bits 6:0) :
 It identifies the general operation

2.rd(bits 11:7) :
 It specifies the Destination register which is used to store the reult of the operation

3.imm[31:12] (bits 31:12) :
 20-bit immediate value (constant) used in the instruction. It is stored in the upper 20 bits of the target register.

Common U-Type Instrutions:
--
| **Instruction** | **Opcode (Bits 6–0)** | **Description**                                         |
|------------------|-----------------------|---------------------------------------------------------|
| `LUI`            | `0110111`            | Load Upper Immediate                                    |
| `AUIPC`          | `0010111`            | Add Upper Immediate to Program Counter (PC)            |


There are further two variants of the instruction formats (B/J) based on the handling of immediates .

5.B-Type:
-
![image](https://github.com/user-attachments/assets/f332980d-bbbd-47aa-9635-fdc77de1d97f)

B-Type instructions enable branching (jumping) to another location in the code, determined by the offset in the instruction.These instructions check specific conditions and branch (jump) to a target address if the condition is satisfied. If the condition is not met, the program continues with the next sequential instruction.

1.opcode(bits 6:0) :
 It identifies the general operation
 
2.imm[11] (bit 7) :
 Represents one of the middle bits of the immediate value.
 
3.imm[4:1] (bits 11:8) :
 Contributes the lower bits of the branch offset.
 
4.funct3 (bits 14:12) :
 specifies the branch condition that determines how the values in the source registers (rs1 and rs2) are compared.
 
5.rs1 (bits 19:15) :
 specifies the first source register for comparision
 
6.rs2 (bits 24:20) :
 specifies the second source register for comparision
 
7.imm[10:5] (bits 30:25) :
 Provides part of the branch offset.
These bits are directly concatenated to the rest of the immediate fields to form the full 12-bit offset.
 
8.imm[12] (bit 31) :
 Determines the sign of the branch offset.
If imm[12] is 1, the offset is negative (indicating a backward branch in memory).
If imm[12] is 0, the offset is positive (indicating a forward branch in memory).


funct3 examples in B-Type:

| **Instruction** | **`funct3` Value** | **Condition**                   |
|------------------|---------------------|----------------------------------|
| `BEQ`           | `000`              | Branch if `rs1 == rs2`.         |
| `BNE`           | `001`              | Branch if `rs1 != rs2`.         |
| `BLT`           | `100`              | Branch if `rs1 < rs2` (signed). |
| `BGE`           | `101`              | Branch if `rs1 >= rs2` (signed).|
| `BLTU`          | `110`              | Branch if `rs1 < rs2` (unsigned).|
| `BGEU`          | `111`              | Branch if `rs1 >= rs2` (unsigned).|

6.J-Type:
-
![image](https://github.com/user-attachments/assets/5fa4d127-0de3-45e7-8305-d116a979ebae)

J-Type instructions are used for unconditional jumps ,these are also  used for control flow, such as implementing function calls or jumping to a specific instruction

1.opcode(bits 6:0) :
 identifies the general operation

2.rd (bits 11:7) :
 Holds the return address (PC + 4), allowing the program to return to this location after completing the jump.

3.imm[19:12] (bits 19:12) :
 Bits 19 through 12 of the immediate value.

4.imm[11] (bit 20) :
 Bit 11 of the intermediate Value

5.imm[10:1] (bits 30:21) :
 Bits 10 through 1 of the immediate value.

6.imm[20] (bit 31) :
 The 21st (MSB) bit of the 21-bit immediate (used for sign extension).

Common J-Type instructions:
-

| **Instruction** | **Opcode (Bits 6–0)** | **Registers** | **Description**                           |
|------------------|-----------------------|---------------|-------------------------------------------|
| `JAL`           | `1101111`            | `rd`          | Jump and Link: Save return address and jump to target address |


## 32-bit instructions from application (counterdown clock):

![15 instr](https://github.com/user-attachments/assets/025ed997-7162-426f-aa1a-eaac5449bad6)

1 . addi sp,sp -32
-
I-Type Instruction

imm[11:0] | rs1 | funct3 | rd | opcode

- *opcode* = 0010011

- *funct3* = 000 (addi)

- *rd* = x2 (sp)

- *rs1* = x2 (sp)

- *imm* = -32 (signed 12-bit: 111111111110)

32 - bit representation:

``11111111111000010 |  000  |   00010 |  0010011``

2.lw a5,12(sp)
-
I-Type Instruction

imm[11:0]  | rs1   | funct3 | rd    | opcode

- *imm[11:0]*	000000001100 (12)

- *rs1*	00010 (sp)

- *funct3*	010 (lw)

- *rd*	01111 (a5)

- *opcode*	0000011

32-bit representation:

``00000000110000010 | 010 | 01111 | 0000011``

3.jal ra,10184
-
J-Type (Jump)

imm[20|10:1|11|19:12] | rd | opcode

- *opcode* = 1101111

- *rd* = x1 (ra)

- *imm* = 10184

- *imm[20]* = 0

- *imm[10:1]* = 1111101000

- *imm[11]* = 1

- *imm[19:12]* = 1100

32-bit representation:

``000000110011001001 | 00001 | 1101111``

4.ld a5,88(a0)
-
I-Type (Load, RV64I)

imm[11:0] | rs1 | funct3 | rd | opcode

- *opcode* = 0000011

- *funct3* = 011 (ld)

- *rd* = x15 (a5)

- *rs1* = x10 (a0)

- *imm* = 88 (000000010110)

32 - bit representation:

``00000001011001010 |011 |   01111 |   0000011``

5.jalr a5
-
I-Type (Jump Register)

imm[11:0] | rs1 | funct3 | rd | opcode

- *opcode* = 1100111

- *funct3* = 000 (jalr)

- *rd* = x15 (a5)

- *rs1* = x15 (a5)

- *imm* = 0

32 - bit-representation:

``00000000000001111 | 000  |   01111  | 1100111``

6.srai s2,s2,0x3
-
I-Type Instruction

imm[11:0] | rs1 | funct3 | rd | opcode

- *imm[11:5]*	0100000

- *imm[4:0]*	00011 (shift amount = 3)

- *rs1*	10010 (s2)

- *funct3*	101 (srai)

- *rd*	10010 (s2)

- *opcode*	0010011

32 - bit representation:

``01000000001110010 | 101 | 10010 | 0010011``

7.bne s2, s1, 10330
-
B-Type (Conditional Branch)

imm[12|10:5] | rs2 | rs1 | funct3 | imm[4:1|11] | opcode

- *opcode* = 1100011


- *funct3* = 001 (bne)

- *rs1* = x18 (s2)

- *rs2* = x9 (s1)

- *imm* = 10330, split as:

- *imm[12]* = 1

- *imm[10:5]* = 100011

- *imm[4:1]* = 0101

- *imm[11]* = 0

32 - bit representation:

``1| 100011   |   01001  |10010 |  001  |  0101| 0  | 1100011``

8.auipc a5, 0xfffff0
---------
U-Type (Upper Immediate)

imm[31:12] | rd | opcode

- *opcode* = 0010111

- *rd* = x15 (a5)

- *imm* = 0xfffff0 (11111111111100000)

32 - bit representation:

``11111111111100000 | 01111 | 0010111``

9.beqz a5, 100c8 <register_fini+0x18>
-
beqz is a pseudo-instruction for beq a5, x0, offset.
It checks if a5 is equal to 0 (x0) and branches to the offset 100c8.

- *imm[12]*	1

- *imm[10:5]*	000110

- *rs2*	00000

- *rs1*	01111

- *funct3*	000

- *imm[4:1]*	1010

- *imm[11]*	1

- *opcode*	1100011

32-bit representation:

``10001000000011110 | 000 | 01010 | 1100011``

10.li a0, 0
-
I-Type Instruction

- *imm[11:0]*	000000000000 (0)

- *rs1*	00000 (zero)

- *funct3*	000 (addi)

- *rd*	01010 (a0)

- *opcode*	0010011

32-bit representation:

``00000000000000000 | 000 | 01010 | 0010011``

11.bnez s1, 102a0
-
B-Type Instruction

- *imm[12]*	0

- *imm[10:5]*	100010

- *rs2*	00000 (zero)

- *rs1*	01001 (s1)

- *funct3*	001 (bne)

- *imm[4:1]*	0001

- *imm[11]*	1

- *opcode*	1100011

32 - bit representation:

``00001000101010001 | 001 | 00101 | 1100011``

12.sd a6, 80(sp)
-
S-Type Instruction

- *imm[11:5]*	0000001

- *rs2*	00110 (a6)

- *rs1*	00001 (sp)

- *funct3*	011 (sd)

- *imm[4:0]*	01000

- *opcode*	0100011

32- bit representation:

``00000010011000001 | 011 | 00001 | 0100011``

13.lui a3, 0xffff
-
U-Type Instruction

*imm[31:12]*	111111111111

- *rd*	00011 (a3)

- *opcode*	0110111

32- bit representation:

``111111111111 | 00011 | 0110111``

14.or a2, a4, -4(a4)
-
R-Type Instruction

- *rs2*	00100 (a4)

- *rs1*	00100 (a4)

- *funct3*	000 (or)

- *rd*	00010 (a2)

- *funct7*	0000000

- *opcode*	0110011

32- bit representation:

``000000000100 | 00100 | 000 | 00010 | 0110011 ``

15.lbu a2,-4(a4)
-
I-Type  Instruction:

- *imm[11:0]*	111111111100

- *rs1*	01000 (a4)

- *funct3*	100 (lbu)

- *rd*	00010 (a2)

- *opcode*	0000011

32- bit reprsentation:

``111111111100 | 01000 | 100 | 00010 | 0000011``

</details>
<details>
<summary><b>Task 4:</b> Functional simulation of RISC-V Core Verilog netlist and Testbench </summary>   
<br>

In this task, we will perform functional simulation of RISC-V instructions modeled as a Verilog netlist and observe the output waveforms using GTKWave.

We will use iverilog and Gtkwave tools

How it works?
-
You write Verilog code and run simulations using Icarus Verilog. The simulator generates a Value Change Dump (VCD) or other waveform file formats.
These waveform files are then loaded into GTKWave for graphical analysis of the signal behavior, helping you verify your design's functionality and timing.

Installing iverilog using command ``sudo apt install iverilog gtkwave``

![iverilog install](https://github.com/user-attachments/assets/9450ad57-a250-4055-bdb8-511628e50a0a)

Steps to perform functional simulation of RISC-V:
-
1.create a new directory using your name as ``mkdir harshith1``.

2.create two files using command ``touch`` and give them names as ``harshith1_rv32i.v``and ``harshith1_rv32i_tb.v``.These are used for verilog and testbench files respectively.

As part of this internship, we are not writing the Verilog code ourselves instead, we are cloning the Verilog and testbench code from a reference GitHub repository.

Refernce Github Repository: [iiitb_rv32i](https://github.com/vinayrayapati/rv32i/tree/main)

3.using leafpad copy the verilog and testbench code in respective files in the directory

4.Simulate the Verilog code using

``iverilog -o harshith1_rv32i harshith1_rv32i.v harshith_rv32i_tb.v`` 

it will create iiitb_rv32i.vcd file which is used for gtkwave.

![vcd file command](https://github.com/user-attachments/assets/18b922c3-3d36-4079-8e77-f6f7387bd409)

5.Open the gtkwave using command ``gtkwave iiitb_rv32i.vcd``

![gtkwave](https://github.com/user-attachments/assets/5887f944-523f-491a-aff1-5ae5904ad603)

The given Verilog file contains instructions that are hard-coded, meaning the designer has used custom bit patterns for each instruction instead of following the standard RISC-V specification for bit encoding.

| **Operation**      | **Description**                                       | **Standard RISC-V ISA** | **Hard-Coded ISA** |
|---------------------|-------------------------------------------------------|-------------------------|---------------------|
| **ADD R6, R2, R1**  | Adds the values in R2 and R1, stores result in R6     | `32'h00110333`         | `32'h02208300`      |
| **SUB R7, R1, R2**  | Subtracts the value in R2 from R1, stores result in R7| `32'h402083b3`         | `32'h02209380`      |
| **AND R8, R1, R3**  | Performs bitwise AND between R1 and R3, stores in R8  | `32'h0030f433`         | `32'h0230a400`      |
| **OR R9, R2, R5**   | Performs bitwise OR between R2 and R5, stores in R9   | `32'h005164b3`         | `32'h02513480`      |
| **XOR R10, R1, R4** | Performs bitwise XOR between R1 and R4, stores in R10 | `32'h0040c533`         | `32'h0240c500`      |
| **SLT R1, R2, R4**  | Sets R1 to 1 if R2 < R4, else sets to 0               | `32'h0045a0b3`         | `32'h02415580`      |
| **ADDI R12, R4, 5** | Adds immediate value 5 to R4, stores result in R12    | `32'h004120b3`         | `32'h00520600`      |
| **BEQ R0, R0, 15**  | Branches to offset 15 if R0 equals R0                 | `32'h00000f63`         | `32'h00f00002`      |
| **SW R3, R1, 2**    | Stores word from R3 to memory address (R1 + 2)        | `32'h0030a123`         | `32'h00209181`      |
| **LW R13, R1, 2**   | Loads word from memory address (R1 + 2) into R13      | `32'h0020a683`         | `32'h00208681`      |
| **SRL R16, R14, R2**| Shifts R14 right by the value in R2, stores in R16    | `32'h0030a123`         | `32'h00271803`      |
| **SLL R15, R1, R2** | Shifts R1 left by the value in R2, stores in R15      | `32'h002097b3`         | `32'h00208783`      |

1. **Standard RISC-V ISA**: Instructions follow the official RISC-V specification for 32-bit instruction encoding.
2. **Hard-Coded ISA**: Instructions deviate from the RISC-V standard and follow a custom encoding defined by the designer.
3. To ensure correct simulation, you must use the hard-coded instruction encodings when working with the provided Verilog netlist and testbench.

Veifying instructions using Gtkwave :
-
1.``ADD R6, R2, R1``

![add gtk](https://github.com/user-attachments/assets/19a0d4f4-a836-4880-8fbc-0b359449b384)

2.``SUB R7, R1, R2``

![sub gtkwave](https://github.com/user-attachments/assets/0aa6192b-4f81-4604-b09d-e1f1d7617324)

3.``AND R8, R1, R3``

![and gtkwave](https://github.com/user-attachments/assets/fac6962c-000a-4f88-b7ba-61d542f1087c)

4.``OR R9, R2, R5``

![or gtkwave](https://github.com/user-attachments/assets/f1bad13c-7453-48cf-b5c8-89fecd460315)

5.``XOR R10, R1, R4``

![xor gtkwave](https://github.com/user-attachments/assets/159a1b56-4811-4261-9102-7cf9d3819b69)

6.``SLT R1, R2, R4``

![slt gtkwave](https://github.com/user-attachments/assets/8191d7b8-c73f-4a66-9b23-ad62c6e00f80)

7.``ADDI R12, R4, 5``

![addi gtkwave](https://github.com/user-attachments/assets/faae3915-9462-42b6-9231-0a36180c63f6)

8.``SW R3, R1, 2``

![sw gtkwave](https://github.com/user-attachments/assets/70c6306d-4a62-483f-8cc3-9260607e417c)
value in register R3 is stored into memory at an address calculated as the sum of the base address in register R1 and the offset 2.
offset 2 is added to the base address from R1 to compute the target memory address (visible in the EX_MEM_ALU_OUT signal).

9.``SRL r16, r11, r2``

![Srl gtkwave](https://github.com/user-attachments/assets/0f7c2e0e-f3dc-4a11-846d-2da9b78f5260)

Perform a logical right shift on the value in r11 by the number of bits specified in r2, and store the result in r16.

10.``BEQ R0, R0, 15``

![beq gtkwave](https://github.com/user-attachments/assets/850966c3-da9f-457c-bdb8-1351daf8cb8c)

This instruction compares the values in registers R0 and R0.
If the values are equal, the program counter (PC) is updated to branch to a target address determined by the offset

Target Address=Current PC+Offset


11.``BNE R0, R1, 15``

![bne gtkwave](https://github.com/user-attachments/assets/30fbe5e7-732a-4dd3-8e2a-201cf548fbaa)

BNE (Branch Not Equal):

Compares the values in R0 and R1.

If R0 ≠ R1, the program counter (PC) branches to the target address.

If R0 = R1, no branch occurs, and the PC continues sequentially.

Offset = 20 (decimal).

Branch Target Address=PC+Offset

12.`` SLL R15, R1, R2``

![sll gtkwave](https://github.com/user-attachments/assets/ecff238a-ecc6-4a39-8714-bcfdbef912e0)

Shift Left Logical (SLL) instruction, which shifts the bits in R1 to the left by the amount specified in R2, with the result stored in R15.

R1: original value to be shifted.

R2: Specifies the shift amount.

Output:

The result (R15) is observed as 0x00000019 in EX_MEM_ALUOUT.

</details>
<details>
<summary><b>Task 5:</b> Design and Implementation of Countdown Counter using Vsdsquadron-Mini </summary>   
<br>

Overview
--
Countdown counter application is designed for an embedded system using a 4x4 matrix keypad and an I2C-based LCD display. It allows users to set a countdown timer in seconds, display the time remaining on the LCD.

Users can input a countdown time (up to 4 digits, in seconds) using the numeric keys (0-9) on the keypad. After entering the desired countdown time, the user presses the` #` key to start the countdown.The LCD shows the time remaining in seconds, which updates every second during the countdown.Users can press the `*` key to reset the timer back to zero. The LCD will display a "Time reset" message.
When the countdown reaches zero, the LCD displays "Time's Up!" to indicate that the timer has finished.After the countdown finishes, users can enter a new time for another countdown cycle. The system continuously waits for input.

Components requried for Application:
-
1.VSDSquadron Mini

2.4x4 Matrix keypad

3.I2c Lcd display

4.Jumper Wires

5.VS Code 

6.PlatformIO IDE

Vsd_Squadronmini:
--

![image-removebg-preview (5)](https://github.com/user-attachments/assets/a63ce851-c985-4a77-88ce-3f7c9678e50d)

Hardware connections:
--
## **Matrix Keypad Connections**

| **Keypad Wire** | **Function** | **VSD Squadron Mini Pin** | **Description**       |
|------------------|--------------|---------------------------|-----------------------|
| Wire 8          | Row 1        | PD7                       | Connects to GPIO PD7  |
| Wire 7          | Row 2        | PD2                       | Connects to GPIO PD2  |
| Wire 6          | Row 3        | PD3                       | Connects to GPIO PD3  |
| Wire 5          | Row 4        | PD4                       | Connects to GPIO PD4  |
| Wire 4          | Column 1     | PC4                       | Connects to GPIO PC4  |
| Wire 3          | Column 2     | PC5                       | Connects to GPIO PC5  |
| Wire 2          | Column 3     | PC6                       | Connects to GPIO PC6  |
| Wire 1          | Column 4     | PC7                       | Connects to GPIO PC7  |

---

## **I2C LCD Display Connections**

| **Pin** | **Function**  | **VSD Squadron Mini Pin** | **Description**        |
|---------|---------------|----------------------------|------------------------|
| GND (1) | Ground        | GND                       | Ground connection      |
| VCC (2) | Power         | 5V                        | Power supply (5V)      |
| SDA (3) | I2C Data Line | PC1                       | Connects to GPIO PC1   |
| SCL (4) | I2C Clock Line| PC2                       | Connects to GPIO PC2   |

Circuit Diagram:
--

![countdown counter](https://github.com/user-attachments/assets/33100501-6150-45bd-a0d1-32319e365b23)



Implemtation of Countdown Counter using Vsdsquadron-Mini , I2c Lcd display and Keypad    
-

Application overview :
-
-  keypad allows the user to input a 4-digit number for the countdown timer.
-  When the user presses the # key, the 4-digit input is converted into a countdown value (in seconds), and the timer starts counting down , the remaining time is displayed on the LCD.
-  User can press the * key at any time to restart the application, clearing the display and prompting the user to enter a new 4-digit countdown value.
-  I2C protocol is used to control the LCD, allowing for clear and dynamic updates during the countdown process.

Code:
-
<details>
<summary><b> code </summary>   
<br>
 
```
#include <ch32v00x.h>
#include <ch32v00x_gpio.h>
#include <stdio.h>  // For sprintf

// Define Keypad pins
#define R1 GPIO_Pin_5 // PD7
#define R2 GPIO_Pin_2 // PD2
#define R3 GPIO_Pin_3 // PD3
#define R4 GPIO_Pin_4 // PD4
#define C1 GPIO_Pin_4 // PC4
#define C2 GPIO_Pin_5 // PC5
#define C3 GPIO_Pin_6 // PC6
#define C4 GPIO_Pin_7 // PC7

// Onboard LED pin (PD6)
#define LED_PIN GPIO_Pin_6

// Define the SDA and SCL Pins for I2C Communication
#define SDA_PIN GPIO_Pin_1
#define SCL_PIN GPIO_Pin_2

// LCD I2C Address
#define LCD_Address 0x27

// Function Prototypes
void GPIO_INIT(void);
char keypad_get_key(void);
void delay_ms(uint32_t ms);
void i2c_start(void);
void i2c_stop(void);
void i2c_write(unsigned char data);
void i2c_ACK(void);
void lcd_send_cmd(unsigned char cmd);
void lcd_send_data(unsigned char data);
void lcd_send_string(const char *str);
void lcd_init(void);
void restart_application(void);

// GPIO Initialization for Keypad and LCD
void GPIO_INIT(void) {
    GPIO_InitTypeDef GPIO_InitStructure;

    // Enable clocks for GPIO ports C and D
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC | RCC_APB2Periph_GPIOD, ENABLE);

    // Initialize rows (R1-R4 on PD) as output
    GPIO_InitStructure.GPIO_Pin = R1 | R2 | R3 | R4;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOD, &GPIO_InitStructure);

    // Initialize columns (C1-C4 on PC) as input pull-up
    GPIO_InitStructure.GPIO_Pin = C1 | C2 | C3 | C4;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU;
    GPIO_Init(GPIOC, &GPIO_InitStructure);

    // Initialize LED pin (PD6) as output push-pull
    GPIO_InitStructure.GPIO_Pin = LED_PIN;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOD, &GPIO_InitStructure);

    // Initialize I2C Pins
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC, ENABLE);
    GPIO_InitStructure.GPIO_Pin = SDA_PIN | SCL_PIN;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_OD;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOC, &GPIO_InitStructure);

    // Initialize LCD
    lcd_init();
}

// Delay Function
void delay_ms(uint32_t ms) {
    while (ms--) {
        for (uint32_t i = 0; i < 4000; i++) {
            __NOP(); // No operation, just a delay
        }
    }
}

// Keypad scan function
char keypad_get_key(void) {
    const char keys[4][4] = {
        {'1', '2', '3', 'A'},
        {'4', '5', '6', 'B'},
        {'7', '8', '9', 'C'},
        {'*', '0', '#', 'D'}
    };

    // Row pins array
    GPIO_TypeDef* row_ports[] = {GPIOD, GPIOD, GPIOD, GPIOD};
    uint16_t rows[] = {R1, R2, R3, R4};

    // Column pins array
    GPIO_TypeDef* col_ports[] = {GPIOC, GPIOC, GPIOC, GPIOC};
    uint16_t cols[] = {C1, C2, C3, C4};

    // Scan rows
    for (int row = 0; row < 4; row++) {
        // Set all rows high
        for (int r = 0; r < 4; r++) {
            GPIO_WriteBit(row_ports[r], rows[r], Bit_SET);
        }

        // Pull current row low
        GPIO_WriteBit(row_ports[row], rows[row], Bit_RESET);

        // Check each column
        for (int col = 0; col < 4; col++) {
            if (GPIO_ReadInputDataBit(col_ports[col], cols[col]) == RESET) {
                return keys[row][col]; // Return pressed key
            }
        }
    }
    return '\0'; // No key pressed
}

// I2C Start Condition
void i2c_start(void) {
    GPIO_SetBits(GPIOC, SDA_PIN);
    GPIO_SetBits(GPIOC, SCL_PIN);
    delay_ms(1);
    GPIO_ResetBits(GPIOC, SDA_PIN);
    delay_ms(1);
    GPIO_ResetBits(GPIOC, SCL_PIN);
    delay_ms(1);
}

// I2C Stop Condition
void i2c_stop(void) {
    GPIO_ResetBits(GPIOC, SDA_PIN);
    delay_ms(1);
    GPIO_SetBits(GPIOC, SCL_PIN);
    delay_ms(1);
    GPIO_SetBits(GPIOC, SDA_PIN);
    delay_ms(1);
}

// I2C Write Byte
void i2c_write(unsigned char data) {
    for (int i = 0; i < 8; i++) {
        if (data & 0x80) {
            GPIO_SetBits(GPIOC, SDA_PIN);
        } else {
            GPIO_ResetBits(GPIOC, SDA_PIN);
        }
        delay_ms(1);
        GPIO_SetBits(GPIOC, SCL_PIN);
        delay_ms(1);
        GPIO_ResetBits(GPIOC, SCL_PIN);
        data <<= 1;
    }
    // Release SDA for ACK
    GPIO_SetBits(GPIOC, SDA_PIN);
    delay_ms(1);
    GPIO_SetBits(GPIOC, SCL_PIN);
    delay_ms(1);
    GPIO_ResetBits(GPIOC, SCL_PIN);
    delay_ms(1);
}

// LCD Initialization
void lcd_init(void) {
    lcd_send_cmd(0x02); // Initialize in 4-bit mode
    lcd_send_cmd(0x28); // Function set: 4-bit, 2 lines, 5x7 font
    lcd_send_cmd(0x0C); // Display ON, Cursor OFF
    lcd_send_cmd(0x06); // Entry mode set: Increment cursor
    lcd_send_cmd(0x01); // Clear display
    delay_ms(20);       // Wait for the clear command
}

// Send Command to LCD
void lcd_send_cmd(unsigned char cmd) {
    unsigned char cmd_u = (cmd & 0xF0);
    unsigned char cmd_l = ((cmd << 4) & 0xF0);

    i2c_start();
    i2c_write(LCD_Address << 1);
    i2c_write(cmd_u | 0x0C); // Enable, RS = 0
    i2c_write(cmd_u | 0x08); // Disable, RS = 0
    i2c_write(cmd_l | 0x0C); // Enable, RS = 0
    i2c_write(cmd_l | 0x08); // Disable, RS = 0
    i2c_stop();
    delay_ms(2);
}

// Send Data to LCD
void lcd_send_data(unsigned char data) {
    unsigned char data_u = (data & 0xF0);
    unsigned char data_l = ((data << 4) & 0xF0);

    i2c_start();
    i2c_write(LCD_Address << 1);
    i2c_write(data_u | 0x0D); // Enable, RS = 1
    i2c_write(data_u | 0x09); // Disable, RS = 1
    i2c_write(data_l | 0x0D); // Enable, RS = 1
    i2c_write(data_l | 0x09); // Disable, RS = 1
    i2c_stop();
    delay_ms(2);
}

// Send String to LCD
void lcd_send_string(const char *str) {
    while (*str) {
        lcd_send_data(*str++);
    }
}

// Restart Application
void restart_application(void) {
    // Reinitialize all GPIOs and LCD
    GPIO_INIT();

    // Clear the digits array
    char digits[4] = {' ', ' ', ' ', ' '};

    lcd_send_cmd(0x80); // Move cursor to first row, first column
    lcd_send_string("Start Timer!");
    delay_ms(2000);  // Display for 2 seconds

    lcd_send_cmd(0x80); // Move cursor to first row, first column
    lcd_send_string("Enter 4 Digits:");
}

// Main Function
int main(void) {
    GPIO_INIT();

    lcd_send_cmd(0x80); // Move cursor to first row, first column
    lcd_send_string("Start Timer!");

    delay_ms(2000);  // Display for 2 seconds

    lcd_send_cmd(0x80); // Move cursor to first row, first column
    lcd_send_string("Enter 4 Digits:");

    char digits[4] = {' ', ' ', ' ', ' '};  // Array to store the last 4 digits
    int digit_count = 0;

    while (1) {
        char key = keypad_get_key();

        // If any key is pressed
        if (key != '\0') {
            // If # is pressed, convert the digits to integer and count down
            if (key == '#') {
                // Convert the digits array to an integer
                int num = (digits[0] - '0') * 1000 + (digits[1] - '0') * 100 + 
                          (digits[2] - '0') * 10 + (digits[3] - '0');

                // Countdown from the number to 0
                for (int i = num; i >= 0; i--) {
                    if (i == 0) {
                        lcd_send_cmd(0xC0); // Move cursor to second row
                        lcd_send_string("Time left: 0 sec");
                        delay_ms(500);  // Wait before updating the LCD
                        lcd_send_cmd(0x01); // Clear display
                        lcd_send_cmd(0x80); // Move cursor to top row
                        lcd_send_string("Time's Up!");
                        delay_ms(2000);  // Display "Time's Up!" for 2 seconds
                        // Clear display
                        lcd_send_cmd(0x01); // Clear display
                        // Show restart message
                        lcd_send_cmd(0x80); // Move cursor to top row
                        lcd_send_string("* to restart");
                        break;
                    } else {
                        char buffer[16];
                        sprintf(buffer, "Time left:%d sec", i); // Format countdown as "Time left: <value> sec"
                        lcd_send_cmd(0xC0); // Move cursor to second row
                        lcd_send_string(buffer);
                        delay_ms(500);  // Wait before updating the LCD
                        lcd_send_cmd(0x01); // Clear display
                    }
                }
            } else {
                // Shift the digits to the left and add the new key
                for (int i = 0; i < 3; i++) {
                    digits[i] = digits[i + 1];
                }
                digits[3] = key;

                // Display the digits on the LCD
                lcd_send_cmd(0xC0); // Move cursor to second row
                lcd_send_data(digits[0]);
                lcd_send_data(digits[1]);
                lcd_send_data(digits[2]);
                lcd_send_data(digits[3]);

                delay_ms(200);  // Small delay to avoid bouncing
            }
        }

        // Check for "*" to restart
        if (key == '*') {
            restart_application();
        }
    }
}
```
</details>

Application Video
--

[Implementation Video Link](https://drive.google.com/file/d/1UFg3R2AqGH0I_9nuWgqDIXKle_nVIilu/view?usp=sharing)


</details>

<details>
<summary><b> Acknowledgement </summary>   
<br>

I want to express my sincere gratitude to Kunal Ghosh Sir for providing me with this wonderful opportunity to learn through the RISCV Architecture internship with VSDSquadron Mini. This internship not only deepened my understanding of the RISCV architecture but also provided valuable practical experience with the Squadron Mini.

As the VSD Squadron Mini Board was essential to the successful completion of our 4x4 Keypad-Controlled Countdown Timer project, I would like to sincerely thank VSD for providing it.





</details>
