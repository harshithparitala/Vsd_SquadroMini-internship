# Vsd_SquadroMini-internship

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

Application:
--

</details>
