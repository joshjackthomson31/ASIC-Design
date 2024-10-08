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
  

<!--| Instruction       | 32-bit Pattern                  | Hexadecimal Pattern | Hardcoded 32-bit Pattern | Hardcoded Hexadecimal Pattern |
|-------------------|---------------------------------|---------------------|------------------------------------|------------------------------------|
| ADD r6, r1, r2     | 000000000001 00010 000 00110 0110011 | 0x00110333         | 000000100010 00000 100 00000 01100000 | 0x02208300  |
| SUB r7, r1, r2     | 010000000010 00000 000 00111 0110011 | 0x402083b3          | 000000100010 01000 100 10000 01100000 | 0x02209380   |  
| AND r8, r1, r3     | 000000000011 00000 111 01000 0110011 | 0x0030f433          | 000000100011 01000 101 00100 01100000 | 0x0230a400      |
| OR r9, r2, r5      | 000000000101 00010 110 01001 0110011 | 0x005164b3          | 000000100101 00010 110 10100 01100000 | 0x02513480     |   
| xor r10, r1, r4     | 000000000100 00000 100 01010 0110011 | 0x0040c533          | 000000100100 00000 100 11000 01100000 | 0x0240c500     |                   
| SLT r11, r2, r4    | 000000000100 00010 010 00001 0110011 | 0x0045a0b3          | 000000100100 00010 101 01010 01100000 | 0x02415580   |
| ADDI r12, r4, 5    | 000000000101 00010 010 01100 0010011   | 0x004120b3         | 000000100100 00010 010 00010 01100000   | 0x00520600     |                 
| SW r3, r1, 2       | 000000000011 00000 010 00001 0100011  | 0x0030a123         | 000000100010 00000 010 00001 01100000  | 0x00209181       |                
| LW r13, r1, 2   | 000000000010 00000 010 01101 0000011 | 0x0020a683          | 000000100010 00000 010 01100 01100000 | 0x00208681 |
| BEQ r0, r0, 15     | 000000000000 00000 000 00000 1100011| 0x00000f63          | 000000000000 00000 000 00000 11000000| 0x00f00002       |                
| ADD r14, r2, r2     | 00000000001000010000011100110011| 0x00210733          | 00000000001000010000011100000000| 0x00210700            | -->

### Custom Instructions Used in the previous task
| Instruction       | 32-bit Pattern                  | Hexadecimal Pattern |
|-------------------|---------------------------------|---------------------|
| ADD r4, r5, r6     | 0000000_00110_00101_000_00100_0110011 | 0x00628233          |
| SUB r6, r4, r5     | 0100000_00101_00100_000_00110_0110011 | 0x40520333          |
| AND r5, r4, r6     | 0000000_00110_00100_111_00101_0110011 | 0x006272B3          |
| OR r8, r5, r5      | 0000000_00101_00101_110_01000_0110011 | 0x0052E433          |
| XOR r8, r4, r4     | 0000000_00100_00100_100_01000_0110011 | 0x00424433          |
| SLT r10, r2, r4    | 0000000_00100_00010_010_01010_0110011 | 0x00412533          |
| ADDI r12, r3, 5    | 000000000101_00011_000_01100_0010011   | 0x00518613          |
| SW r3, r1, 4       | 0000000_00011_00001_010_00100_0100011  | 0x0030A223          |
| SRL r16, r11, r2   | 0000000_00010_01011_101_10000_0110011 | 0x0025D833          |
| BNE r0, r1, 20     | 0_000001_00001_00000_001_0100_0_1100011| 0x02101463          |
| BEQ r0, r0, 15     | 0_000000_00000_00000_000_1111_0_1100011| 0x00000F63          |
| LW r13, r11, 2     | 000000000010_01011_010_01101_0000011   | 0x0025A683          |
| SLL r15, r11, r2   | 0000000_00010_01011_001_01111_0110011 | 0x002597B3          |

### Differences between Standard RISC-V ISA and Hardcoded ISA
| Instruction       | Hexadecimal Pattern | Hardcoded Hexadecimal Pattern |
|-------------------|---------------------|------------------------------------|
| ADD r6, r1, r2     | 0x00110333         | 0x02208300  |
| SUB r7, r1, r2     | 0x402083b3          | 0x02209380   |  
| AND r8, r1, r3     | 0x0030f433          | 0x0230a400      |
| OR r9, r2, r5      | 0x005164b3          | 0x02513480     |   
| XOR r10, r1, r4    | 0x0040c533          | 0x0240c500     |                   
| SLT r11, r2, r4    | 0x0045a0b3          | 0x02415580   |
| ADDI r12, r4, 5    | 0x004120b3         | 0x00520600     |                 
| SW r3, r1, 2       | 0x0030a123         | 0x00209181       |                
| LW r13, r1, 2      | 0x0020a683          | 0x00208681 |
| BEQ r0, r0, 15     | 0x00000f63          | 0x00f00002       |                
| ADD r14, r2, r2    | 0x00210733          | 0x00210700            |


### OUTPUT WAVEFORM

## 1. `ADD r6, r1, r2`
![357153400-cbf23082-1b95-4b73-93d2-1d89a50ed8e6-2](https://github.com/user-attachments/assets/2b2e0349-9692-4cb5-888d-4c01992d727a)

## 2. `SUB r7, r1, r2`
![357153993-97e14855-3f17-4f94-8ee1-970fe52b5e7d](https://github.com/user-attachments/assets/08d74c15-7490-4630-a161-1e2409bddcc4)

## 3. `AND r8, r1, r3`
![357155025-0a861382-5687-4349-9922-d2cc806ba511](https://github.com/user-attachments/assets/a81d4e1c-c8c8-4b35-ac09-b317eccba831)

## 4. `OR r9, r2, r5`
![357156142-e09a563c-0268-4c2c-89fc-720e4c84519f-2](https://github.com/user-attachments/assets/1dc078f2-77e4-4813-863e-72040caab03d)

## 5. `XOR r10, r1, r4`
![357156892-a8a0ee27-6c15-4eb3-be80-a9b645637a92](https://github.com/user-attachments/assets/775d4034-0d1d-4227-807d-7d30ae116542)

## 6. `SLT r11, r2, r4`
![357158841-c37f11b7-103f-421f-9fc0-4b4389f42daf](https://github.com/user-attachments/assets/a723c357-3af5-4d19-a9cf-a5ae072f76ae)

## 7. `ADDI r12, r4, 5`
![357159228-defe4c4c-17fb-4dbd-b09f-7073727eaeea](https://github.com/user-attachments/assets/975d40e6-26d6-47be-aa5a-ea3efda8fef0)

## 8. `SW r3, r1, 2`
![357160078-88e47ae3-a8d3-4e70-b031-4f312a1c1d86](https://github.com/user-attachments/assets/389f9b68-381b-4492-a3f9-df488eb9444b)

## 9. `LW r13, r1, 2`
![357160364-4fecc4b2-6f2f-4ac1-85e8-d9aea0acd91b](https://github.com/user-attachments/assets/230227a5-bd68-4612-b48e-2fedabc9d193)

## 10. `BEQ r0, r0, 15`
![357160810-837fffa7-fba9-4039-96fe-9641fb0f7f34](https://github.com/user-attachments/assets/68781a8d-81cd-4b10-a252-39c2ad6938bf)

## 11. `ADD r14, r2, r2`
![357161095-d396a1c2-8c54-4f39-b107-29479d6cc627](https://github.com/user-attachments/assets/1fb48fe6-7fdb-4058-b6a5-1f1fae21de0f)

## FINAL OUTPUT
![357161461-a50a8aa6-151a-445c-a51f-561af167ae0a](https://github.com/user-attachments/assets/0469a7bb-ce2a-4ac1-9050-6387f292f495)

---
## LAB SESSION - 5

### AIM
* To select an application and write it's code in C language.
* To compile the code using GCC and RISC-V GCC compilers.

### APPLICATION
* To create a power calculator that calculates the the power of a number to another number.

### C CODE FOR THE APPLICATION
```
#include <stdio.h>

double power( int a, int b)
{
        double result = 1.0;
        for( int i = 0; i < b; i++)
        {
                result *= a;
        }
        return result;
}

int main()
{
        int a, b;
        printf( "Enter a base : ");
        scanf( " %d", &a);
        printf( "Enter a power : ");
        scanf( " %d", &b);
        int exp = b;
        if( b < 0) exp = -b;
        double result = power( a, exp);
        if( b < 0) result = 1.0/result;
        printf( "%d to the power of %d is %g\n", a, b, result);
        return 0;
}
```
![WhatsApp Image 2024-08-14 at 9 21 38 PM](https://github.com/user-attachments/assets/7b4aa55f-9ca8-4e5a-93e3-065bf121ff20)

### CASE 1 : Running using GCC compiler
```
gcc powercalc.c
./a.out
```
![WhatsApp Image 2024-08-14 at 9 23 54 PM](https://github.com/user-attachments/assets/ba20d9b9-b532-414c-ab81-6c8a7b7cb9bf)

### CASE 2 : Running using RISC-V GCC compiler
* Compile using the O1 optimization for the RISC-V 64-bit architecture.
* Spike Simulator is used to run the RISC-V 64-bit architecture.
```
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o powercalc.o powercalc.c
spike pk powercalc.o
```
![WhatsApp Image 2024-08-14 at 9 25 08 PM](https://github.com/user-attachments/assets/cf74d222-c84c-426f-939d-4f470fe8ffe3)

### CONCLUSION
* We can observe that the output is same when compiled using both GCC and RISC-V GCC compilers. This indicates that the program's logic is platform-independent.

---
## LAB SESSION - 6
## Day 3
---
### Digital Logic Design and TL-Verilog with Makerchip : 
### Logic Gates : 
* Logic gates are essential elements in digital circuits, carrying out key logical operations on binary input signals. They form the foundation of complex systems, such as processors, memory units, and controllers. Operating on binary signals, where "0" represents a low voltage level and "1" represents a high voltage level, logic gates receive one or more input signals and generate an output signal according to defined logical functions.

The below table contains some common logic gates.
<img width="857" alt="Screenshot 2024-08-21 at 9 54 54 PM" src="https://github.com/user-attachments/assets/ec55f756-47e6-44ec-8ccc-9ad2c8b1a740">

### TL - Verilog : 
* TL-Verilog is an advanced extension of traditional Verilog, developed by Redwood EDA, aimed at simplifying hardware design and modeling. It streamlines the design process with a more abstract and efficient syntax, while maintaining compatibility with standard Verilog. TL-Verilog enables transaction-level modeling, which simplifies the management of complex microarchitectures by allowing transactions to move through the architecture and interact with components such as pipelines, arbiters, and queues. This approach is particularly effective in minimizing bugs and enhancing the design process when using tools like Makerchip.

### Makerchip IDE : 
* Makerchip IDE is a robust platform for digital design, offering an all-in-one environment for coding, simulation, and testing of HDL designs. It supports languages such as TL-Verilog, SystemVerilog, Verilog, and VHDL, providing a visual interface to build and simulate digital systems in real-time. Its intuitive interface and comprehensive features make it suitable for both novice and experienced designers. Makerchip allows you to efficiently prototype, debug, and optimize your digital designs, ensuring they work correctly before proceeding to hardware implementation.

## BASIC COMBINATIONAL CIRCUITS : 
### 1. Inverter
#### Code
```
$out = ! $in;
```
#### The generated block diagram and waveforms are shown below.
<img width="1440" alt="Screenshot 2024-08-21 at 9 59 52 PM" src="https://github.com/user-attachments/assets/71863040-7e07-44dc-88ed-af3bcaee3c35">

### 2. Arithmetic Operation on Vectors
#### Code
```
$out[4:0] = $in1[3:0] + $in2[3:0];
```
#### The generated block diagram and waveforms are shown below.
<img width="1440" alt="Screenshot 2024-08-21 at 10 00 25 PM" src="https://github.com/user-attachments/assets/96b770e3-e591-4f18-a781-05e77454bfd3">

### 3. 2-input AND gate
#### Code
```
$out = $in1 && $in2;
```
#### The generated block diagram and waveforms are shown below.
<img width="1440" alt="Screenshot 2024-08-21 at 10 01 01 PM" src="https://github.com/user-attachments/assets/9ccdfafc-421c-458f-92db-a902deec6ba7">

### 4. 2-inoput OR gate
#### Code
```
$out = $in1 || $in2;
```
#### The generated block diagram and waveforms are shown below.
<img width="1440" alt="Screenshot 2024-08-21 at 10 01 38 PM" src="https://github.com/user-attachments/assets/01a0e546-6a4c-4fef-8065-1071c71ae7b6">

### 5. 2-input XOR gate
#### Code
```
$out = $in1 ^ $in2;
```
#### The generated block diagram and waveforms are shown below.
<img width="1440" alt="Screenshot 2024-08-21 at 10 02 10 PM" src="https://github.com/user-attachments/assets/e0d65059-c583-43b9-b615-7cfe4b4c3936">

### 6. 2:1 MUX
#### Code
```
$out[11:0] = $sel ? $in1[11:0] : $in0[11:0];
```
#### The generated block diagram and waveforms are shown below.
<img width="1440" alt="Screenshot 2024-08-21 at 10 02 36 PM" src="https://github.com/user-attachments/assets/f3527722-2325-4148-a183-ae72a1700b09">

### 7. Combinational Calculator
* This calculator performs basic fundamental operations such as addition, subtraction, multipication, division.
#### Code
```$val1[31:0] = $rand1[3:0];
$val2[31:0] = $rand2[3:0];

$sum[31:0]  = $val1[31:0] + $val2[31:0];
$diff[31:0] = $val1[31:0] - $val2[31:0];
$prod[31:0] = $val1[31:0] * $val2[31:0];
$quot[31:0] = $val1[31:0] / $val2[31:0];

$out[31:0] = ($sel[1:0] == 2'b00) ? $sum[31:0]:
             ($sel[1:0] == 2'b01) ? $diff[31:0]:
             ($sel[1:0] == 2'b10) ? $prod[31:0]:
                                    $quot[31:0];
```
#### The generated block diagram and waveforms are shown below.
<img width="1440" alt="Screenshot 2024-08-21 at 10 03 32 PM" src="https://github.com/user-attachments/assets/695ca1b0-220f-44b9-96c6-8bfcb313661f">

## SEQUENTIAL CIRCUITS :
* A sequential circuit is a type of digital circuit that incorporates memory components to store data, enabling it to produce outputs based on both the current inputs and its previous state. Unlike combinational circuits, which rely solely on present inputs, sequential circuits use feedback loops and storage elements like flip-flops or registers to maintain their internal state. This internal state, combined with current inputs, determines the circuit's behavior, allowing it to perform tasks that depend on input history, such as counting, data storage, or event sequencing.

### 1. Fibonacci Series
#### Code
```
$num[31:0] = $reset ? 1 : (>>1$num + >>2$num);
```
#### The generated block diagram and waveforms are shown below.
<img width="1440" alt="Screenshot 2024-08-21 at 10 04 39 PM" src="https://github.com/user-attachments/assets/47256c0c-ed64-45e1-8a64-91b3747a62c8">

### 2. Counter Series
#### Code
```
$cnt[31:0] = $reset ? 0 : (>>1$cnt + 1);
```
#### The generated block diagram and waveforms are shown below.
<img width="1440" alt="Screenshot 2024-08-21 at 10 05 30 PM" src="https://github.com/user-attachments/assets/62ce46dc-c45f-4da4-b7e4-2c8849e2897b">

### 3. Sequential Calculator
#### Code
```
   $val1[31:0] = >>2$out;
   $val2[31:0] = $rand2[3:0];

   $sum[31:0]  = $val1[31:0] + $val2[31:0];
   $diff[31:0] = $val1[31:0] - $val2[31:0];
   $prod[31:0] = $val1[31:0] * $val2[31:0];
   $quot[31:0] = $val1[31:0] / $val2[31:0];

   $nxt[31:0] = ($sel[1:0] == 2'b00) ? $sum[31:0]:
                ($sel[1:0] == 2'b01) ? $diff[31:0]:
                ($sel[1:0] == 2'b10) ? $prod[31:0]:
                                       $quot[31:0];
   
   $out[31:0] = $reset ? 32'h0 : $nxt;
```
#### The generated block diagram and waveforms are shown below.
<img width="1440" alt="Screenshot 2024-08-21 at 10 06 38 PM" src="https://github.com/user-attachments/assets/e17a08e2-54d2-4399-8651-ff036efddf19">

## PIPELINED LOGIC
* In Transaction-Level Verilog (TL-Verilog), pipelined logic is effectively represented using pipeline constructs that capture the flow of data through various design stages, each aligned with a clock cycle. This method streamlines the modeling of sequential logic by automatically managing state propagation, allowing for clear and concise descriptions of complex, multi-stage operations, which enhances both the clarity and maintainability of the design.

### 1. Recreating the design
#### Code
```|pipe
  @1
    $err1 = $bad_input || $illegal_op;
  @3
    $err2 = $over_flow || $err1;
  @6
    $err3 = $div_by_zero || $err2;
```
#### The generated block diagram and waveforms are shown below.
<img width="1440" alt="Screenshot 2024-08-21 at 10 08 46 PM" src="https://github.com/user-attachments/assets/e8e129be-5cc9-4447-9fd5-d604b15a3af0">

### 2. Pipelined calculator
#### Code
```   |cal
      @1
         $reset = *reset;
         $clk_kar = *clk;

         $valid[31:0] = $reset ? 0 : (>>1$valid + 1);
         $nreset = $reset | ~$valid;
         
         $val1[31:0] = >>2$out;
         $val2[31:0] = $rand2[3:0];

         $sum[31:0]  = $val1[31:0] + $val2[31:0];
         $diff[31:0] = $val1[31:0] - $val2[31:0];
         $prod[31:0] = $val1[31:0] * $val2[31:0];
         $quot[31:0] = $val1[31:0] / $val2[31:0];
         
      @2
         $nxt[31:0] = ($sel[1:0] == 2'b00) ? $sum[31:0]:
                      ($sel[1:0] == 2'b01) ? $diff[31:0]:
                      ($sel[1:0] == 2'b10) ? $prod[31:0]:
                                             $quot[31:0];
        
         
         $out[31:0] = $nreset ? 32'h0 : $nxt;
```
#### The generated block diagram and waveforms are shown below.
<img width="1440" alt="Screenshot 2024-08-21 at 10 09 27 PM" src="https://github.com/user-attachments/assets/5c22dc15-388c-4d65-a144-d541ffe750c7">

### 3. Cycle calculator with validity
#### Code
```
   $reset = *reset;
   $clk_kar = *clk;
   
   |cal
      @1
         $reset = *reset;
         $clk_kar = *clk;
         
         $cnt[31:0] = $reset ? 0 : (>>1$cnt + 1);
         $valid = $cnt;
         $valid_or_reset = ($reset | $valid);
         
      
      ?$valid
         @1
            $val1[31:0] = >>2$out;
            $val2[31:0] = $rand2[3:0];
            
            $sum[31:0]  = $val1[31:0] + $val2[31:0];
            $diff[31:0] = $val1[31:0] - $val2[31:0];
            $prod[31:0] = $val1[31:0] * $val2[31:0];
            $quot[31:0] = $val1[31:0] / $val2[31:0];
            
         @2
            $nxt[31:0] = ($sel[1:0] == 2'b00) ? $sum[31:0]:
                         ($sel[1:0] == 2'b01) ? $diff[31:0]:
                         ($sel[1:0] == 2'b10) ? $prod[31:0]:
                                                $quot[31:0];
            
            $out[31:0] = $valid_or_reset ? 32'h0 : $nxt;
```
#### The generated block diagram and waveforms are shown below.
<img width="1440" alt="Screenshot 2024-08-21 at 10 10 17 PM" src="https://github.com/user-attachments/assets/8fc5105b-cfb7-4d60-aa31-ec22c7036761">

---
## Day 4
---
### Basic RISC-V CPU Micro-architecture
* This section will discuss the implementation of a basic 3-stage RISC-V Core/CPU.
* The three main stages are: Fetch, Decode, and Execute.

#### A basic block diagram of the CPU core is shown below.
<img width="517" alt="Screenshot 2024-08-21 at 10 52 47 PM" src="https://github.com/user-attachments/assets/4f15ad5a-4055-4829-82df-c5dd2258444b">

### 1. Program Counter
* The Program Counter, also known as the Instruction Pointer, is a component that holds the address of the next instruction to be executed. It typically increments by 4 to fetch the subsequent instruction from memory. If a reset occurs, the Program Counter is reinitialized to zero for the next instruction, and then it resumes operation.
#### Code
```
$pc[31:0] = >>1$reset ? 0 : ( >>1$pc + 31'h4 );
```
#### The generated block diagram and waveforms are shown below.
<img width="1440" alt="Screenshot 2024-08-21 at 10 58 38 PM" src="https://github.com/user-attachments/assets/52c97945-be5c-4102-b0ba-ae1173704cbc">

### 2. Adding instruction memory
* The output of the Program Counter (PC) is sent to the instruction memory, which then provides the instruction to be executed.
* The PC increments by 4 with each valid cycle.
* This output is used to retrieve an instruction from the instruction memory, which outputs a 32-bit instruction based on the provided address.
* During the Fetch stage, the processor retrieves the instruction from the instruction memory using the address specified by the PC.

#### Code
```
$imem_rd_en = >>1$reset ? 0 : 1;
$imem_rd_addr[31:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
$instr[31:0] = $imem_rd_data[31:0];
```
#### The generated block diagram and waveforms are shown below.
<img width="1440" alt="Screenshot 2024-08-21 at 11 05 26 PM" src="https://github.com/user-attachments/assets/738ab0a6-a773-4768-a513-9d8ec90e66e4">

### 3. Decoding instruction
* The 32-bit fetched instruction must first be decoded to determine the operation to be performed and the source/destination addresses. There are six types of instructions:

- R-type: Register
- I-type: Immediate
- S-type: Store
- B-type: Branch (Conditional Jump)
- U-type: Upper Immediate
- J-type: Jump (Unconditional Jump)

* The instruction format includes the Opcode, immediate value, source address, and destination address.
* During the Decode stage, the processor interprets the instruction based on its format and type.
* Typically, the RISC-V ISA offers 32 registers, each with a width of XLEN (e.g., XLEN = 32 for RV32).
* The register file used here supports 2 reads and 1 write simultaneously.

<img width="844" alt="Screenshot 2024-08-21 at 10 10 45 PM" src="https://github.com/user-attachments/assets/68a50379-7be3-4b7f-855f-c50141e9cd33">

#### Code
```
$is_i_instr = $instr[6:2] ==? 5'b0000x ||
              $instr[6:2] ==? 5'b001x0 ||
              $instr[6:2] ==? 5'b11001;
$is_r_instr = $instr[6:2] ==? 5'b01011 ||
              $instr[6:2] ==? 5'b011x0 ||
              $instr[6:2] ==? 5'b10100;
$is_s_instr = $instr[6:2] ==? 5'b0100x;
$is_b_instr = $instr[6:2] ==? 5'b11000;
$is_j_instr = $instr[6:2] ==? 5'b11011;
$is_u_instr = $instr[6:2] ==? 5'b0x101;
```
#### The generated block diagram and waveforms are shown below.
<img width="1440" alt="Screenshot 2024-08-21 at 11 06 58 PM" src="https://github.com/user-attachments/assets/69484e57-a88f-4d5b-a8f2-7d76ad610f62">

### 4. Immediate decode logic
<img width="823" alt="Screenshot 2024-08-21 at 11 42 19 PM" src="https://github.com/user-attachments/assets/109f8017-f7dc-4ca3-9242-7d76c1ba1e47">

#### Code
```
$imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
             $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
             $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
             $is_u_instr ? {$instr[31:12], 12'b0} :
             $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} : 32'b0;
```
#### The generated block diagram and waveforms are shown below.
<img width="1440" alt="Screenshot 2024-08-21 at 11 09 31 PM" src="https://github.com/user-attachments/assets/0e9b437e-c364-469f-8cfc-f2c3778c65b7">

### 5. Decode logic for other fields
#### Code
```
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
            
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
            
         $funct7_valid = $is_r_instr ;
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
            
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         ?$rd_valid
            $rd[4:0] = $instr[11:7];

         $opcode[6:0] = $instr[6:0];
```
#### The generated block diagram and waveforms are shown below.
<img width="1440" alt="Screenshot 2024-08-21 at 11 11 51 PM" src="https://github.com/user-attachments/assets/b2d1a70d-ec17-4f13-a68b-41cef3f813c2">

### 6. Decoding individual instructions
#### Code
```
$dec_bits [10:0] = {$funct7[5], $funct3, $opcode};
$is_beq = $dec_bits ==? 11'bx_000_1100011;
$is_bne = $dec_bits ==? 11'bx_001_1100011;
$is_blt = $dec_bits ==? 11'bx_100_1100011;
$is_bge = $dec_bits ==? 11'bx_101_1100011;
$is_bltu = $dec_bits ==? 11'bx_110_1100011;
$is_bgeu = $dec_bits ==? 11'bx_111_1100011;
$is_addi = $dec_bits ==? 11'bx_000_0010011;
$is_add = $dec_bits ==? 11'b0_000_0110011;
```
#### Program counter should be updated as below.
```
$pc[31:0] = >>1$reset ? 32'b0 :
            >>1$taken_branch ? >>1$br_target_pc :
            >>1$pc + 32'd4;
```
#### The generated block diagram and waveforms are shown below.
<img width="1440" alt="Screenshot 2024-08-21 at 11 18 36 PM" src="https://github.com/user-attachments/assets/f8fc7f56-2a30-46c1-b363-f2d4d0f7d589">

### 7. Register file Read and Enable
#### Code
```
$rf_rd_en1 = $rs1_valid;
$rf_rd_en2 = $rs2_valid;
$rf_rd_index1[4:0] = $rs1;
$rf_rd_index2[4:0] = $rs2;
$src1_value[31:0] = $rf_rd_data1;
$src2_value[31:0] = $rf_rd_data2;
```
#### The generated block diagram and waveforms are shown below.
<img width="1440" alt="Screenshot 2024-08-21 at 11 26 24 PM" src="https://github.com/user-attachments/assets/105b2686-81f4-4316-9b99-704215c94c6d">

### 8. ALU
#### Code
```
$result[31:0] = $is_addi ? $src1_value + $imm :
                $is_add ? $src1_value + $src2_value :
                32'bx ;
```
#### The generated block diagram and waveforms are shown below.
<img width="1440" alt="Screenshot 2024-08-21 at 11 27 14 PM" src="https://github.com/user-attachments/assets/f2c70e3f-6d93-4fae-a9ab-3818776c9eec">

### 9. Register file write
* Once the ALU completes operations on the values in the registers, these results may need to be written back into the registers.
* This is done using the register file write.
* It is important to ensure that no values are written to the destination register if it is x0, as this register is always intended to remain 0.
#### Code
```
$rf_wr_en = $rd_valid && $rd != 5'b0;
$rf_wr_index[4:0] = $rd;
$rf_wr_data[31:0] = $result;
```
#### The generated block diagram and waveforms are shown below.
<img width="1440" alt="Screenshot 2024-08-21 at 11 30 37 PM" src="https://github.com/user-attachments/assets/f7d15043-067b-4632-9975-0752cf3f0e31">

### 10. Branch instructions
#### Code
```
$taken_branch = $is_beq ? ($src1_value == $src2_value):
                $is_bne ? ($src1_value != $src2_value):
                $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                $is_bltu ? ($src1_value < $src2_value):
                $is_bgeu ? ($src1_value >= $src2_value):1'b0;

$br_target_pc[31:0] = $pc +$imm;
```
#### The generated block diagram and waveforms are shown below.
<img width="1440" alt="Screenshot 2024-08-21 at 11 30 37 PM" src="https://github.com/user-attachments/assets/fc614e9c-4eb5-4f52-b77b-7e83a81e9066">

### 11. Testbench
* In order to check whether the code is correct or not, we can verify using testbench for the 1st five cycles.
#### Code
```
*passed = |cpu/xreg[10]>>5$value == (1+2+3+4+5+6+7+8+9) ;
```
#### If we check the log file, we can get the following result.
<img width="1440" alt="Screenshot 2024-08-21 at 11 57 53 PM" src="https://github.com/user-attachments/assets/a6e31c88-632e-444e-a504-52ee71793fee">

### 12. Results
#### Block Diagram
<img width="1440" alt="Screenshot 2024-08-22 at 1 25 11 AM" src="https://github.com/user-attachments/assets/e8aa2015-b89d-4f72-8e46-544523adc5f2">

#### Clock waveform
<img width="1440" alt="Screenshot 2024-08-22 at 1 25 59 AM" src="https://github.com/user-attachments/assets/c0aca3ac-5527-4652-ad22-a0cf25bc80b4">

#### Reset waveform
<img width="1440" alt="Screenshot 2024-08-22 at 1 26 05 AM" src="https://github.com/user-attachments/assets/a2383957-3fc7-491d-8039-80ac5294abe5">

#### Final result waveform
<img width="1440" alt="Screenshot 2024-08-22 at 1 26 15 AM" src="https://github.com/user-attachments/assets/504bf917-6b82-4927-a18a-c6fcdc73325b">

---
## Day 5
---
### Complete Pipelined RISC-V CPU Micro-architecture
* Once pipelining is validated through simulations, support for Jump instructions is added.
* Additionally, the implementation of Instruction Decode and the ALU for the RV32I Base Integer Instruction Set are integrated.

### Valid signal for pipelined logic : 
* The TL-Verilog code to introduce valid signal for pipelined logic is given below.
```
$start = >>1$reset && !$reset;
$valid = $reset ? 1'b0 : ($start || >>3$valid);
$valid_or_reset = $valid || $reset;
$rs1_or_funct3_valid    = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
$rs2_valid              = $is_r_instr || $is_s_instr || $is_b_instr;
$rd_valid               = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
$funct7_valid           = $is_r_instr;
```
### Handling data hazards in register file with bypassing : 
```
$src1_value[31:0] = $rs1_bypass ? >>1$result[31:0] : $rf_rd_data1[31:0];
$src2_value[31:0] = $rs2_bypass ? >>1$result[31:0] : $rf_rd_data2[31:0];
```
### Correecting branch target path : 
```
   //Current instruction is valid if one of the previous 2 instructions were not (taken_branch or load or jump)
   $valid = ~(>>1$valid_taken_br || >>2$valid_taken_br || >>1$is_load || >>2$is_load || >>2$jump_valid 	|| >>1$jump_valid);
         
   //Current instruction is valid & is a taken branch
   $valid_taken_br = $valid && $taken_br;
         
   //Current instruction is valid & is a load
   $valid_load = $valid && $is_load;
         
   //Current instruction is valid & is jump
   $jump_valid = $valid && $is_jump;
   $jal_valid  = $valid && $is_jal;
   $jalr_valid = $valid && $is_jalr;
    
    *passed = |cpu/xreg[14]>>5$value == (1+2+3+4+5+6+7+8+9+10);
```
### Final 5-stage pipeline
```
\m4_TLV_version 1d: tl-x.org
\SV
   // Template code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 0 to 9 Program |
   // \====================/
   //
   // Add 0,1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   // 
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   m4_asm(SW, r0, r10, 10000)           // Store r10 result in dmem
   m4_asm(LW, r17, r0, 10000)           // Load contents of dmem to r17
   m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
         $clk_jack = *clk;
         
         //PC fetch - branch, jumps and loads introduce 2 cycle bubbles in this pipeline
         $pc[31:0] = >>1$reset ? '0 : (>>3$valid_taken_br ? >>3$br_tgt_pc :
                                       >>3$valid_load     ? >>3$inc_pc[31:0] :
                                       >>3$jal_valid      ? >>3$br_tgt_pc :
                                       >>3$jalr_valid     ? >>3$jalr_tgt_pc :
                                                     (>>1$inc_pc[31:0]));
         // Access instruction memory using PC
         $imem_rd_en = ~ $reset;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         
         
      @1
         //Getting instruction from IMem
         $instr[31:0] = $imem_rd_data[31:0];
         
         //Increment PC
         $inc_pc[31:0] = $pc[31:0] + 32'h4;
         
         //Decoding I,R,S,U,B,J type of instructions based on opcode [6:0]
         //Only [6:2] is used here because this implementation is for RV64I which does not use [1:0]
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] == 5'b11001;
         
         $is_r_instr = $instr[6:2] == 5'b01011 ||
                       $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] == 5'b10100;
         
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $is_b_instr = $instr[6:2] == 5'b11000;
         
         $is_j_instr = $instr[6:2] == 5'b11011;
         
         //Immediate value decode
         $imm[31:0] = $is_i_instr ? { {21{$instr[31]}} , $instr[30:20]} :
                      $is_s_instr ? { {21{$instr[31]}} , $instr[30:25] , $instr[11:8] , $instr[7]} :
                      $is_b_instr ? { {20{$instr[31]}} , $instr[7] , $instr[30:25] , $instr[11:8] , 1'b0} :
                      $is_u_instr ? { $instr[31] , $instr[30:12] , { 12{1'b0}} } :
                      $is_j_instr ? { {12{$instr[31]}} , $instr[19:12] , $instr[20] , $instr[30:21] , 1'b0} :
                      >>1$imm[31:0];
         
         //Generate valid signals for each instruction fields
         $rs1_or_funct3_valid    = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid              = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid               = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct7_valid           = $is_r_instr;
         
         //Decode other fields of instruction - source and destination registers, funct, opcode
         ?$rs1_or_funct3_valid
            $rs1[4:0]    = $instr[19:15];
            $funct3[2:0] = $instr[14:12];
         
         ?$rs2_valid
            $rs2[4:0]    = $instr[24:20];
         
         ?$rd_valid
            $rd[4:0]     = $instr[11:7];
         
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         
         $opcode[6:0] = $instr[6:0];
         
         //Decode instruction in subset of base instruction set based on RISC-V 32I
         $dec_bits[10:0] = {$funct7[5],$funct3,$opcode};
         
         //Branch instructions
         $is_beq   = $dec_bits ==? 11'bx_000_1100011;
         $is_bne   = $dec_bits ==? 11'bx_001_1100011;
         $is_blt   = $dec_bits ==? 11'bx_100_1100011;
         $is_bge   = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu  = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu  = $dec_bits ==? 11'bx_111_1100011;
         
         //Jump instructions
         $is_auipc = $dec_bits ==? 11'bx_xxx_0010111;
         $is_jal   = $dec_bits ==? 11'bx_xxx_1101111;
         $is_jalr  = $dec_bits ==? 11'bx_000_1100111;
         
         //Arithmetic instructions
         $is_addi  = $dec_bits ==? 11'bx_000_0010011;
         $is_add   = $dec_bits ==  11'b0_000_0110011;
         $is_lui   = $dec_bits ==? 11'bx_xxx_0110111;
         $is_slti  = $dec_bits ==? 11'bx_010_0010011;
         $is_sltiu = $dec_bits ==? 11'bx_011_0010011;
         $is_xori  = $dec_bits ==? 11'bx_100_0010011;
         $is_ori   = $dec_bits ==? 11'bx_110_0010011;
         $is_andi  = $dec_bits ==? 11'bx_111_0010011;
         $is_slli  = $dec_bits ==? 11'b0_001_0010011;
         $is_srli  = $dec_bits ==? 11'b0_101_0010011;
         $is_srai  = $dec_bits ==? 11'b1_101_0010011;
         $is_sub   = $dec_bits ==? 11'b1_000_0110011;
         $is_sll   = $dec_bits ==? 11'b0_001_0110011;
         $is_slt   = $dec_bits ==? 11'b0_010_0110011;
         $is_sltu  = $dec_bits ==? 11'b0_011_0110011;
         $is_xor   = $dec_bits ==? 11'b0_100_0110011;
         $is_srl   = $dec_bits ==? 11'b0_101_0110011;
         $is_sra   = $dec_bits ==? 11'b1_101_0110011;
         $is_or    = $dec_bits ==? 11'b0_110_0110011;
         $is_and   = $dec_bits ==? 11'b0_111_0110011;
         
         //Store instructions
         $is_sb    = $dec_bits ==? 11'bx_000_0100011;
         $is_sh    = $dec_bits ==? 11'bx_001_0100011;
         $is_sw    = $dec_bits ==? 11'bx_010_0100011;
         
         //Load instructions - support only 4 byte load
         $is_load  = $dec_bits ==? 11'bx_xxx_0000011;
         
         $is_jump = $is_jal || $is_jalr;
         
      @2
         //Get Source register values from reg file
         $rf_rd_en1 = $rs1_or_funct3_valid;
         $rf_rd_en2 = $rs2_valid;
         
         $rf_rd_index1[4:0] = $rs1[4:0];
         $rf_rd_index2[4:0] = $rs2[4:0];
         
         //Register file bypass logic - data forwarding from ALU to resolve RAW dependence
         $src1_value[31:0] = $rs1_bypass ? >>1$result[31:0] : $rf_rd_data1[31:0];
         $src2_value[31:0] = $rs2_bypass ? >>1$result[31:0] : $rf_rd_data2[31:0];
         
         //Branch target PC computation for branches and JAL
         $br_tgt_pc[31:0] = $imm[31:0] + $pc[31:0];
         
         //RAW dependence check for ALU data forwarding
         //If previous instruction was writing to reg file, and current instruction is reading from same register
         $rs1_bypass = >>1$rf_wr_en && (>>1$rd == $rs1);
         $rs2_bypass = >>1$rf_wr_en && (>>1$rd == $rs2);
         
      @3
         //ALU
         $result[31:0] = $is_addi  ? $src1_value +  $imm :
                         $is_add   ? $src1_value +  $src2_value :
                         $is_andi  ? $src1_value &  $imm :
                         $is_ori   ? $src1_value |  $imm :
                         $is_xori  ? $src1_value ^  $imm :
                         $is_slli  ? $src1_value << $imm[5:0]:
                         $is_srli  ? $src1_value >> $imm[5:0]:
                         $is_and   ? $src1_value &  $src2_value:
                         $is_or    ? $src1_value |  $src2_value:
                         $is_xor   ? $src1_value ^  $src2_value:
                         $is_sub   ? $src1_value -  $src2_value:
                         $is_sll   ? $src1_value << $src2_value:
                         $is_srl   ? $src1_value >> $src2_value:
                         $is_sltu  ? $sltu_rslt[31:0]:
                         $is_sltiu ? $sltiu_rslt[31:0]:
                         $is_lui   ? {$imm[31:12], 12'b0}:
                         $is_auipc ? $pc + $imm:
                         $is_jal   ? $pc + 4:
                         $is_jalr  ? $pc + 4:
                         $is_srai  ? ({ {32{$src1_value[31]}} , $src1_value} >> $imm[4:0]) :
                         $is_slt   ? (($src1_value[31] == $src2_value[31]) ? $sltu_rslt : {31'b0, $src1_value[31]}):
                         $is_slti  ? (($src1_value[31] == $imm[31]) ? $sltiu_rslt : {31'b0, $src1_value[31]}) :
                         $is_sra   ? ({ {32{$src1_value[31]}}, $src1_value} >> $src2_value[4:0]) :
                         $is_load  ? $src1_value +  $imm :
                         $is_s_instr ? $src1_value + $imm :
                                    32'bx;
         
         $sltu_rslt[31:0]  = $src1_value <  $src2_value;
         $sltiu_rslt[31:0] = $src1_value <  $imm;
         
         //Jump instruction target PC computation
         $jalr_tgt_pc[31:0] = $imm[31:0] + $src1_value[31:0]; 
         
         //Branch resolution
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                     $is_bne ? ($src1_value != $src2_value) :
                     $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bltu ? ($src1_value < $src2_value) :
                     $is_bgeu ? ($src1_value >= $src2_value) :
                     1'b0;
         
         //Current instruction is valid if one of the previous 2 instructions were not (taken_branch or load or jump)
         $valid = ~(>>1$valid_taken_br || >>2$valid_taken_br || >>1$is_load || >>2$is_load || >>2$jump_valid || >>1$jump_valid);
         
         //Current instruction is valid & is a taken branch
         $valid_taken_br = $valid && $taken_br;
         
         //Current instruction is valid & is a load
         $valid_load = $valid && $is_load;
         
         //Current instruction is valid & is jump
         $jump_valid = $valid && $is_jump;
         $jal_valid  = $valid && $is_jal;
         $jalr_valid = $valid && $is_jalr;
         
         //Destination register update - ALU result or load result depending on instruction
         $rf_wr_en = (($rd != '0) && $rd_valid && $valid) || >>2$valid_load;
         $rf_wr_index[4:0] = $valid ? $rd[4:0] : >>2$rd[4:0];
         $rf_wr_data[31:0] = $valid ? $result[31:0] : >>2$ld_data[31:0];
         
      @4
         //Data memory access for load, store
         $dmem_addr[3:0]     =  $result[5:2];
         $dmem_wr_en         =  $valid && $is_s_instr;
         $dmem_wr_data[31:0] =  $src2_value[31:0];
         $dmem_rd_en         =  $valid_load;
         
      
         //Write back data read from load instruction to register
         $ld_data[31:0]      =  $dmem_rd_data[31:0];
         
      
      

      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   //Checks if sum of numbers from 1 to 9 is obtained in reg[17] and runs 10 cycles extra after this is met
   *passed = |cpu/xreg[17]>>10$value == (1+2+3+4+5+6+7+8+9);
   //Run for 200 cycles without any checks
   //*passed = *cyc_cnt > 200;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@2, @3)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+dmem(@4)    // Args: (read/write stage)
   
   m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic
                       // @4 would work for all labs
\SV
   endmodule
```
### Final check for passed condition : 
```
*passed = |cpu/xreg[14]>>5$value == (1+2+3+4+5+6+7+8+9);
```
### Results
#### Block Diagram
<img width="1440" alt="Screenshot 2024-08-22 at 1 57 50 AM" src="https://github.com/user-attachments/assets/dc099981-f9e2-4ad7-b383-21fcae476ed4">

#### VIZ table
<img width="1440" alt="Screenshot 2024-08-22 at 1 58 03 AM" src="https://github.com/user-attachments/assets/13f4f038-5835-4ff2-a596-e167485a21bf">

#### Clock waveform
<img width="1440" alt="Screenshot 2024-08-22 at 1 58 17 AM" src="https://github.com/user-attachments/assets/0102f6eb-462c-4634-89a3-86e91bb30b2f">

#### Reset waveform
<img width="1440" alt="Screenshot 2024-08-22 at 1 58 28 AM" src="https://github.com/user-attachments/assets/01812007-ae6e-4158-b4ff-f4bc52d2ebee">

#### Final result waveform
<img width="1440" alt="Screenshot 2024-08-22 at 2 06 31 AM" src="https://github.com/user-attachments/assets/fc84f7d7-f695-459b-95f2-4c351620b606">

---
## LAB SESSION - 7
---
## Comparision of RISC-V Pre-Synthesis Simulation outputs using Iverilog GTKwave and Makerchip
* The RISC-V processor was developed using TL-Verilog in the Makerchip IDE. To deploy it on an FPGA, it was converted to Verilog using the Sandpiper-SaaS compiler. Pre-synthesis simulations were subsequently carried out using the GTKWave simulator.

### Process of simulation
1. Execute these commands to configure a development environment for working with simulation and synthesis tools, specifically for tasks related to Verilog and RISC-V.
```
$ sudo apt install make python python3 python3-pip git iverilog gtkwave
$ cd ~
$ sudo apt-get install python3-venv
$ python3 -m venv .venv
$ source ~/.venv/bin/activate
$ pip3 install pyyaml click sandpiper-saas
```
![361258922-a22edab5-6c59-4e98-bac6-bb59c89a5234](https://github.com/user-attachments/assets/2a3e9e62-2330-4d85-8fd0-f63e18a5cb40)

2. To install the necessary packages, execute these commands within a virtual environment:
```
$ sudo apt install make python python3 python3-pip git iverilog gtkwave docker.io
$ sudo chmod 666 /var/run/docker.sock
$ cd ~
$ pip3 install pyyaml click sandpiper-saas
```
![361259149-c2851530-1ddb-494e-8dda-accb4d7d0df7](https://github.com/user-attachments/assets/20f707c9-174a-410b-9122-30fde9593103)

3. Next, clone the following repository into your home directory, and create a `pre_synth_sim` directory to store the output :
```
$ cd ~
$ git clone https://github.com/manili/VSDBabySoC.git
$ cd /home/vsduser/VSDBabySoC
$ make pre_synth_sim
```
![361259238-36e4b6d9-fdad-43cb-a7c0-696abeeecad7](https://github.com/user-attachments/assets/73c9f525-ec93-4663-9488-3cafac420033)

4. Replace the `rvmyth.tlv` file in the `VSDBabySoC/src/module` folder with your RISC-V design `.tlv` file from Makerchip that you want to convert to Verilog. Additionally, update the testbench to match your Makerchip code.

5. To obtain the Verilog code from your TLV code, i.e., to convert the `.tlv` definition of RISC-V into a `.v` definition, use the following code.
```
$ sandpiper-saas -i ./src/module/rvmyth.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/
```
![361259770-abfa2d5b-9db8-4ddb-954c-6e248e450576](https://github.com/user-attachments/assets/b07721f2-01ab-4206-bf2b-054a792d5eb3)

6. To compile and simulate the RISC-V design, execute the following code.
```
$ iverilog -o output/pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module
```

7. The simulation results (`pre_synth_sim.vcd`) will be saved in the `output/pre_synth_sim` directory.
```
$ cd output
$ ./pre_synth_sim.out
```
![361259960-929c392e-aa09-4420-ac4f-6a3c2b04ba8a](https://github.com/user-attachments/assets/8f13877d-04c2-480c-9b91-e19bebc376ca)

8. To open the .vcd simulation file through GTKWave simulation tool :
```
$ gtkwave pre_synth_sim.vcd
```

### Pre-synthesis simulation results
* The following signals are to be plotted.
1. clk_jack : This is the clock input to the RISC-V core.
2. reset : This is the input reset signal to the RISC-V core.
3. OUT[9:0]: This is the 10-bit output port [9:0] from the RISC-V core. It is sourced from RISC-V register #14.

### GTKWave simulation waveforms : 
1. clk_jack plot
![image](https://github.com/user-attachments/assets/231b8d9e-85c0-483a-8faa-b8fda5afffd8)

2. reset plot
![image](https://github.com/user-attachments/assets/91e78f8a-4131-48d5-a8c8-19b72d4e74fb)

3. OUT[9:0] plot
![image](https://github.com/user-attachments/assets/5c87303e-5f57-4998-ae29-a7e89815bd0b)

### Makerchip IDE simulation results for comparison
1. clk_jack plot
<img width="1440" alt="360088847-0102f6eb-462c-4634-89a3-86e91bb30b2f" src="https://github.com/user-attachments/assets/755845aa-452b-411a-8d5f-0dbf91ba12ad">

2. reset plot
<img width="1440" alt="360088915-01812007-ae6e-4158-b4ff-f4bc52d2ebee" src="https://github.com/user-attachments/assets/72c82dd6-831c-4ea2-bbb1-15eb064c424d">

3. OUT[9:0] plot
<img width="1440" alt="360090757-fc84f7d7-f695-459b-95f2-4c351620b606" src="https://github.com/user-attachments/assets/8ac013d2-4167-4ec4-bdab-11a3f2a9cfce">

---
## LAB SESSION - 8
---
### AIM
* This task involves integrating and validating the functionality of a custom RISC-V processor (rvmyth) within the BabySoC platform, utilizing a specific set of tools for digital design and simulation.
* The goal is to generate DAC and PPL waveforms for the RISC-V processor.

### PHASE-LOCKED LOOP (PLL)
* A Phase-Locked Loop (PLL) is an electronic control system that produces an output signal with a phase that is synchronized to the phase of an input signal.
* It is widely utilized in telecommunications, radio, and computing for signal synchronization, frequency stabilization, and clock generation for digital circuits.

### Download and Prepare Project Files
* All the files BabySoc can be downloaded using the following command.
```
git clone https://github.com/manili/VSDBabySoC.git
```
![WhatsApp Image 2024-09-03 at 1 35 56 AM](https://github.com/user-attachments/assets/9b644de2-d9cf-4850-93f3-926e29b90974)

### Editing the Top-level verilog code
![WhatsApp Image 2024-09-03 at 1 34 33 AM](https://github.com/user-attachments/assets/011c1096-d3f1-4855-98b8-54f3e07511b9)

### Simulation Procedure
* Functional simulation can be performed using the following command.
```
cd BabySoC_Simulation
iverilog -o ./pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module/
./pre_synth_sim.out
gtkwave pre_synth_sim.vcd
```
![WhatsApp Image 2024-09-03 at 1 33 00 AM](https://github.com/user-attachments/assets/57b3edb0-f847-4fb9-9aec-0e982cab98ba)

### RESULTS
* Clock waveform named as clk_jack along with PLL clock
![WhatsApp Image 2024-09-03 at 1 28 45 AM](https://github.com/user-attachments/assets/dd6a1044-61a5-4fde-987a-bb658e6dc928)

* Reset waveform
  ![WhatsApp Image 2024-09-03 at 1 29 20 AM](https://github.com/user-attachments/assets/7c64f90e-1521-4588-a0b0-f0d2508c10f1)

* Final result waveform
  ![WhatsApp Image 2024-09-03 at 1 29 58 AM](https://github.com/user-attachments/assets/2f58767a-62a9-425f-b760-8e009fc81f7b)
