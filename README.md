# ASIC-Design

## LAB SESSION-1

### AIM
To verify and compile a C program to find sum from 1 to n using GCC and RISC-V GNU compiler.

### PROCEDURE
#### Task-1 : Compile the C program code using GCC compiler.
1. **Creating text editor :**
   ```
   leafpad sum.c
   ```
   ![WhatsApp Image 2024-08-07 at 3 21 35 PM](https://github.com/user-attachments/assets/76ba4dc1-66a0-4f63-b342-3fae9eecc83d)

2. **Code :**
   ```
   #include <stdio.h>
   int main() 
   {
       int n = 50;
       int sum = 0;
       for( int i = 1; i <= n; i++)
       {
         sum += i;
       }
       printf("The sum from 1 to %d is %d\n", n, sum);
       return 0;
   }
   ```
   ![WhatsApp Image 2024-08-07 at 7 09 57 PM (1)](https://github.com/user-attachments/assets/ee35387c-5cec-459b-9e84-34ccc4ff890c)


3. **Compile the code :**
   ```
   gcc sum.c
   ```
   ![WhatsApp Image 2024-08-07 at 3 21 34 PM (1)](https://github.com/user-attachments/assets/2592666e-649c-4cf8-b04d-f6b2d1a363b6)

4. **Run the code :**
   ```
   ./a.out
   ```
   
5. **Output :**
   ```
   The sum from 1 to 50 is 1275
   ```
   ![WhatsApp Image 2024-08-07 at 6 57 48 PM](https://github.com/user-attachments/assets/7dacd42c-2a4c-4227-8659-af7e769dd98c)

6. **Conclusion :**

   In the above snap, it can be seen that the C program is complied successfully and the output is also correctly calculated i.e., 1275 ( 1+2+3+...+49+50 = 1275).

#### Task-2 : Compile the C program code using RISC-V GNU compiler and optimising it using O1 and Ofast.
1. **Compile the code using O1 optimisation :**
   ```
   riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum.o sum.c
   ```
   ![WhatsApp Image 2024-08-07 at 7 09 57 PM](https://github.com/user-attachments/assets/c387346e-8e54-417f-8a00-89c5fcb48cae)


2. **Creating the output of the compiler i.e., the .o file( object file) :**
   ```
   riscv64-unknown-elf-objdump -d sum.o | less
   ```
   When you type the above command, a list of opcode appears in the terminal. To obtain the main section of the program we need to type /main as shown below.
   ```
   /main
   ```
   ![WhatsApp Image 2024-08-07 at 6 57 49 PM](https://github.com/user-attachments/assets/bfbeb764-c46e-43e9-a492-672731ba21de)
   ![WhatsApp Image 2024-08-07 at 6 30 19 PM](https://github.com/user-attachments/assets/c636e0dc-94b0-4724-9968-d26f9fe43dd7)
   ![WhatsApp Image 2024-08-07 at 7 09 57 PM (2)](https://github.com/user-attachments/assets/bf257908-e1b0-41bc-9443-d40e514f9b6c)

3. **Observation-1 :**
   
   We can see there are 14 lines of opcode in the main section.

4. **Compile the code using Ofast optimisation :**
   ```
   riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum.o sum.c
   ```
   ![WhatsApp Image 2024-08-07 at 6 57 47 PM](https://github.com/user-attachments/assets/7274225e-f5cc-4fd0-82d5-2c20140b94a7)

5. **Creating the output of the compiler i.e., .o file( object file) :**
   ```
   riscv64-unknown-elf-objdump -d sum.o | less
   ```
   The addition 'less' command is used to interactively browse the potentially lengthy disassembly output of sum.o.
   When you type the above command, a list of opcode appears in the terminal. to obtain the main section of the program we need to type /main as shown below.
   ```
   /main
   ```
   ![WhatsApp Image 2024-08-07 at 10 15 38 PM (1)](https://github.com/user-attachments/assets/56706ca5-2e96-45b3-a134-331c9fd63054)


6. **Observation-2 :**

   We can see there are 11 lines of opcode in the main section.

7. **Results :**
   
   * The number of instructions using O1 optimisation = 14.
   * The number of instructions usinf Ofast optimisation = 11.

8. **Conclusion :**
   
   The number of instructions decreases with Ofast due to aggressive optimization. This leads to an increase in compilation time and code size.

---
## LAB SESSION-2

### AIM
To debug the main section of the above program sum.c and to observe the values of register after each step of compilation.

### PROCEDURE

1. **Compile the code using the spike simulator and then compare the results obtained using both the compilation techniques by typing the following command.**
   ```
   spike pk sum.o
   ```
   ![WhatsApp Image 2024-08-07 at 10 15 38 PM](https://github.com/user-attachments/assets/5a4d02eb-1f1a-45a6-839b-14b88ee9f8e1)

   **Observation :** The results obtained from both the compilation techniques are the same.

2. **Execution of sum.o in the spike simulator to debug the code using the following command.**
   ```
   spike -d pk sum.o
   ```
   ![WhatsApp Image 2024-08-07 at 10 15 37 PM](https://github.com/user-attachments/assets/5bacb11d-9cd0-4723-b7fd-56eb3142e8d7)

   **Observation :** We'll be entering the debugging mode after this command has been executed.
   
3. **Bring the Program Counter( PC) to the start of main by using the following command.**
   ```
   until bc 0 100b0
   ```
   ![WhatsApp Image 2024-08-07 at 10 15 37 PM (1)](https://github.com/user-attachments/assets/199d8fae-d602-45ef-99cb-c45dcb163c33)

   * Here, 0x100b0 is the address of the start of main function.
   * Since register a0 is present at 0x100b0, we'll check register a0 before and after the execution of instructions in the next step.
   
4. **Execution of the following commands to check he contents of register a0 before and after running the instructions.**
   ```
   reg 0 a0
   ```
   ![WhatsApp Image 2024-08-07 at 10 15 37 PM (2)](https://github.com/user-attachments/assets/ae0f546f-0fbe-4022-ba6f-1cc1f2c57d6d)
   * lui a0, 0x21 command adds the register ao by 0x21.

5. **Move PC to location 100b4 using the following command.**
   ```
   until pc 0 100b4
   ``` 
   
6. **Checking the contents of sp.**
   ```
   reg 0 sp
   ```
   ![WhatsApp Image 2024-08-07 at 10 15 38 PM (2)](https://github.com/user-attachments/assets/cf986585-5933-442a-b28c-ace004dd545d)
   * addi sp, sp, -16 reduces the sp pointer by 16.
   * sp pointer gets updated from 0x0000003ffffffb50 to 0x0000003ffffffb40.

---
## LAB SESSION - 3

### AIM
* To identify different instruction types ( R-type, I-type, S-type, B-type, U-type, J-type).
* To write the 32-bit pattern code for each instruction.

### INSTRUCTIONS
```
 ADD r4, r5, r6
 SUB r6, r4, r5
 AND r5, r4, r6
 OR r8, r5, r5
 XOR r8, r4, r4
 SLT r10, r2, r4
 ADDI r12, r3, 5
 SW r3, r1, 4
 SRL r16, r11, r2
 BNE r0, r1, 20
 BEQ r0, r0, 15
 LW r13, r11, 2
 SLL r15, r11, r2
```

### RISC-V BASE INSTRUCTION FORMATS
![WhatsApp Image 2024-08-10 at 11 32 17 PM](https://github.com/user-attachments/assets/bced5b57-39ba-44c4-9767-70bf0017e8a9)

### BASE INSTRUCTION TABLE
![WhatsApp Image 2024-08-10 at 11 31 46 PM](https://github.com/user-attachments/assets/dfd4a9a9-9ef4-4ba1-9b46-151a9e219b47)

### 32-bit pattern for all the instructions and their RISC-V instruction type

## 1. `ADD r4, r5, r6`
- **Instruction Type**: R-type
- **Description**: `r4` will store the sum of `r5` and `r6`.
- **Opcode for ADD**: `0110011`
- **rd (destination register)**: `r4 = 00100`
- **rs1 (source register 1)**: `r5 = 00101`
- **rs2 (source register 2)**: `r6 = 00110`
- **func3**: `000`
- **func7**: `0000000`
- **32-bit instruction**: `0000000_00110_00101_000_00100_0110011`

## 2. `SUB r6, r4, r5`
- **Instruction Type**: R-type
- **Description**: `r6` will store the result of `r4` minus `r5`.
- **Opcode for SUB**: `0110011`
- **rd (destination register)**: `r6 = 00110`
- **rs1 (source register 1)**: `r4 = 00100`
- **rs2 (source register 2)**: `r5 = 00101`
- **func3**: `000`
- **func7**: `0100000`
- **32-bit instruction**: `0100000_00101_00100_000_00110_0110011`

## 3. `AND r5, r4, r6`
- **Instruction Type**: R-type
- **Description**: `r5` will store the bitwise AND of `r4` and `r6`.
- **Opcode for AND**: `0110011`
- **rd (destination register)**: `r5 = 00101`
- **rs1 (source register 1)**: `r4 = 00100`
- **rs2 (source register 2)**: `r6 = 00110`
- **func3**: `111`
- **func7**: `0000000`
- **32-bit instruction**: `0000000_00110_00100_111_00101_0110011`

## 4. `OR r8, r5, r5`
- **Instruction Type**: R-type
- **Description**: `r8` will store the bitwise OR of `r5` and `r5`.
- **Opcode for OR**: `0110011`
- **rd (destination register)**: `r8 = 01000`
- **rs1 (source register 1)**: `r5 = 00101`
- **rs2 (source register 2)**: `r5 = 00101`
- **func3**: `110`
- **func7**: `0000000`
- **32-bit instruction**: `0000000_00101_00101_110_01000_0110011`

## 5. `XOR r8, r4, r4`
- **Instruction Type**: R-type
- **Description**: `r8` will store the bitwise XOR of `r4` and `r4`.
- **Opcode for XOR**: `0110011`
- **rd (destination register)**: `r8 = 01000`
- **rs1 (source register 1)**: `r4 = 00100`
- **rs2 (source register 2)**: `r4 = 00100`
- **func3**: `100`
- **func7**: `0000000`
- **32-bit instruction**: `0000000_00100_00100_100_01000_0110011`

## 6. `SLT r10, r2, r4`
- **Instruction Type**: R-type
- **Description**: `r10` will be set to 1 if `r2` is less than `r4`, otherwise 0.
- **Opcode for SLT**: `0110011`
- **rd (destination register)**: `r10 = 01010`
- **rs1 (source register 1)**: `r2 = 00010`
- **rs2 (source register 2)**: `r4 = 00100`
- **func3**: `010`
- **func7**: `0000000`
- **32-bit instruction**: `0000000_00100_00010_010_01010_0110011`

## 7. `ADDI r12, r3, 5`
- **Instruction Type**: I-type
- **Description**: `r12` will store the sum of `r3` and the immediate value 5.
- **Opcode for ADDI**: `0010011`
- **rd (destination register)**: `r12 = 01100`
- **rs1 (source register 1)**: `r3 = 00011`
- **imm[11:0] (immediate value)**: `5 = 000000000101`
- **func3**: `000`
- **32-bit instruction**: `000000000101_00011_000_01100_0010011`

## 8. `SW r3, r1, 4`
- **Instruction Type**: S-type
- **Description**: Store the value in `r3` to the memory address calculated by `r1 + 4`.
- **Opcode for SW**: `0100011`
- **rs1 (source register 1)**: `r1 = 00001`
- **rs2 (source register 2)**: `r3 = 00011`
- **imm[11:5] and imm[4:0] (immediate value)**: `4 = 000000000100`
- **func3**: `010`
- **32-bit instruction**: `000000_00011_00001_010_000100_0100011`

## 9. `SRL r16, r11, r2`
- **Instruction Type**: R-type
- **Description**: Perform a logical shift right on `r11` by the number of positions specified in `r2`, store the result in `r16`.
- **Opcode for SRL**: `0110011`
- **rd (destination register)**: `r16 = 10000`
- **rs1 (source register 1)**: `r11 = 01011`
- **rs2 (source register 2)**: `r2 = 00010`
- **func3**: `101`
- **func7**: `0000000`
- **32-bit instruction**: `0000000_00010_01011_101_10000_0110011`

## 10. `BNE r0, r1, 20`
- **Instruction Type**: B-type
- **Description**: Branch to PC + 20 if `r0` is not equal to `r1`.
- **Opcode for BNE**: `1100011`
- **rs1 (source register 1)**: `r0 = 00000`
- **rs2 (source register 2)**: `r1 = 00001`
- **imm[12:1] (immediate value)**: `20 = 000000010100`
- **func3**: `001`
- **32-bit instruction**: `0_000001_00001_00000_001_0100_0_1100011`

## 11. `BEQ r0, r0, 15`
- **Instruction Type**: B-type
- **Description**: Branch to PC + 15 if `r0` is equal to `r0`.
- **Opcode for BEQ**: `1100011`
- **rs1 (source register 1)**: `r0 = 00000`
- **rs2 (source register 2)**: `r0 = 00000`
- **imm[12:1] (immediate value)**: `15 = 000000001111`
- **func3**: `000`
- **32-bit instruction**: `0_000000_00000_00000_000_1111_0_1100011`

## 12. `LW r13, r11, 2`
- **Instruction Type**: I-type
- **Description**: Load the word from the memory address calculated by `r11 + 2` into `r13`.
- **Opcode for LW**: `0000011`
- **rd (destination register)**: `r13 = 01101`
- **rs1 (source register 1)**: `r11 = 01011`
- **imm[11:0] (immediate value)**: `2 = 000000000010`
- **func3**: `010`
- **32-bit instruction**: `000000000010_01011_010_01101_0000011`

## 13. `SLL r15, r11, r2`
- **Instruction Type**: R-type
- **Description**: Perform a logical shift left on `r11` by the number of positions specified in `r2`, store the result in `r15`.
- **Opcode for SLL**: `0110011`
- **rd (destination register)**: `r15 = 01111`
- **rs1 (source register 1)**: `r11 = 01011`
- **rs2 (source register 2)**: `r2 = 00010`
- **func3**: `001`
- **func7**: `0000000`
- **32-bit instruction**: `0000000_00010_01011_001_01111_0110011`

### SUMMARY TABLE

| Instruction       | Instruction Type | 32-bit Pattern                  | Hexadecimal Pattern |
|-------------------|------------------|---------------------------------|---------------------|
| ADD r4, r5, r6     | R-type           | 0000000_00110_00101_000_00100_0110011 | 0x00628233          |
| SUB r6, r4, r5     | R-type           | 0100000_00101_00100_000_00110_0110011 | 0x40520333          |
| AND r5, r4, r6     | R-type           | 0000000_00110_00100_111_00101_0110011 | 0x006272B3          |
| OR r8, r5, r5      | R-type           | 0000000_00101_00101_110_01000_0110011 | 0x0052E433          |
| XOR r8, r4, r4     | R-type           | 0000000_00100_00100_100_01000_0110011 | 0x00424433          |
| SLT r10, r2, r4    | R-type           | 0000000_00100_00010_010_01010_0110011 | 0x00412533         |
| ADDI r12, r3, 5    | I-type           | 000000000101_00011_000_01100_0010011   | 0x00518613          |
| SW r3, r1, 4       | S-type           | 0000000_00011_00001_010_00100_0100011  | 0x0030A223         |
| SRL r16, r11, r2   | R-type           | 0000000_00010_01011_101_10000_0110011 | 0x0025D833         |
| BNE r0, r1, 20     | B-type           | 0_000001_00001_00000_001_0100_0_1100011| 0x02101463         |
| BEQ r0, r0, 15     | B-type           | 0_000000_00000_00000_000_1111_0_1100011| 0x00000F63          |
| LW r13, r11, 2     | I-type           | 000000000010_01011_010_01101_0000011   | 0x0025A683         |
| SLL r15, r11, r2   | R-type           | 0000000_00010_01011_001_01111_0110011 | 0x002597B3         |

---
## LAB SESSION - 4

### AIM
* To use the RISC-V core verilog netlist and testbench for a functional simulation experiment.
* To plot the waveform in gtkwave.

### HARDCODED CODE BASED ON THE VERILOG TABLE
* The opcode varies for each instruction type in the verilog code.
* Different instructions have different func3 values.
* To distinguish between immediate operations and other arithmetic operations, func7 is used. Otherwisr, func7 is set to be 0.

| Instruction       | Instruction Type | 32-bit Pattern                  | Hexadecimal Pattern | Hardcoded 32-bit Pattern | Hardcoded Hexadecimal Pattern |
|-------------------|------------------|---------------------------------|---------------------|------------------------------------|------------------------------------|
| ADD r4, r5, r6     | R-type           | 0000000_00110_00101_000_00100_0110011 | 0x00628233          | 0000000_00110_00101_000_00100_0110011 | 0x00B28233                         |
| SUB r6, r4, r5     | R-type           | 0100000_00101_00100_000_00110_0110011 | 0x40520333          | 0100000_00101_00100_000_00110_0110011 | 0x40A302B3                         |
| AND r5, r4, r6     | R-type           | 0000000_00110_00100_111_00101_0110011 | 0x006272B3          | 0000000_00110_00100_111_00101_0110011 | 0x00C2B2B3                         |
| OR r8, r5, r5      | R-type           | 0000000_00101_00101_110_01000_0110011 | 0x0052E433          | 0000000_00101_00101_110_01000_0110011 | 0x00A2C333                         |
| XOR r8, r4, r4     | R-type           | 0000000_00100_00100_100_01000_0110011 | 0x00424433          | 0000000_00100_00100_100_01000_0110011 | 0x00828233                         |
| SLT r10, r2, r4    | R-type           | 0000000_00100_00010_010_01010_0110011 | 0x00412533          | 0000000_00100_00010_010_01010_0110011 | 0x004112B3                         |
| ADDI r12, r3, 5    | I-type           | 000000000101_00011_000_01100_0010011   | 0x00518613          | 000000000101_00011_000_01100_0010011   | 0x00518693                         |
| SW r3, r1, 4       | S-type           | 0000000_00011_00001_010_00100_0100011  | 0x0030A223          | 0000000_00011_00001_010_00100_0100011  | 0x00212023                         |
| SRL r16, r11, r2   | R-type           | 0000000_00010_01011_101_10000_0110011 | 0x0025D833          | 0000000_00010_01011_101_10000_0110011 | 0x0025A533                         |
| BNE r0, r1, 20     | B-type           | 0_000001_00001_00000_001_0100_0_1100011| 0x02101463          | 0_000001_00001_00000_001_0100_0_1100011| 0x00211263                         |
| BEQ r0, r0, 15     | B-type           | 0_000000_00000_00000_000_1111_0_1100011| 0x00000F63          | 0_000000_00000_00000_000_1111_0_1100011| 0x0000F063                         |
| LW r13, r11, 2     | I-type           | 000000000010_01011_010_01101_0000011   | 0x0025A683          | 000000000010_01011_010_01101_0000011   | 0x0025A693                         |
| SLL r15, r11, r2   | R-type           | 0000000_00010_01011_001_01111_0110011 | 0x002597B3          | 0000000_00010_01011_001_01111_0110011 | 0x0025B533                         |
