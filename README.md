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

---
LAB SESSION - 9
---

<details>
<summary><strong>Day 1:</strong>Introduction to Verilog RTL design and Synthesis.</summary>

## Introduction to iverilog

* In digital circuit design, **Register-Transfer Level (RTL)** is an abstraction used to model synchronous digital circuits by describing the flow of data between hardware registers and the logic operations applied to these signals. This RTL abstraction is expressed using **HDL (Hardware Description Language)** to create high-level models, which are later transformed into lower-level representations and, ultimately, the physical hardware design.

* **Simulator**: A tool used to verify the circuit design. In this workshop, we use the **Icarus Verilog (iverilog)** tool. Simulation involves creating models that mimic the behavior of the target device (simulation models) and using test models to validate the device (test benches). RTL design is typically composed of one or more Verilog files that specify the design’s functionality and requirements.

* **Test Bench**: A setup that delivers stimulus (test vectors) to the design, verifying its behavior and ensuring it meets the required specifications.


### HOW SIMULATOR WORKS 
* **Simulator** looks for changes on input signals and based on that output is evaluated if there is an input change, the output is evaluated; else the simulator will never evaluate the output.

![Screenshot from 2024-10-21 10-51-23](https://github.com/user-attachments/assets/8612b99d-cf8d-4e88-b1bc-89d8a0bb9bbc)


* **Design** may have 1 or more primary inputs and primary outputs but **testbench** doesn't have it.

### SIMULATION FLOW

![Screenshot from 2024-10-21 10-59-06](https://github.com/user-attachments/assets/1862f4ea-b19a-4fce-9c8d-de6a2db43151)


## Introduction to LABS

### ENVIRONMENT SETUP

Set up the tool flow using the below commands:

```
mkdir VLSI

cd VLSI

git clone https://github.com/kunalg123/vsdflow.git

git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git

cd sky130RTLDesignAndSynthesisWorkshop
```

![Screenshot from 2024-10-21 23-08-41](https://github.com/user-attachments/assets/d57c401f-dcba-4933-9a9d-aac04f2630f4)

* The **sky130RTLDesignAndSynthesisWorkshop** directory includes **My_Lib**, which holds all the required library files. The **lib** folder contains standard cell libraries for synthesis, while the **verilog_model** folder has the Verilog models for all the standard cells in the library. The **verilog_files** folder has all the lab session experiments, including both the Verilog code and testbench files.

![Screenshot from 2024-10-21 23-10-49](https://github.com/user-attachments/assets/d8f9b9cf-65e5-4f89-91d7-f30344fbfe4e)

## Labs using iverilog & gtkwave

### Simulation using iverilog simulator - 2:1 multiplexer rtl design

#### VERILOG FILE OF A SIMPLE 2:1 MUX

To compile the verilog and testbench file use the following commands which will generate an executable file and will dump the waveform to view it using the gtkwave

```
iverilog good_mux.v tb_good_mux.v

./a.out

gtkwave tb_good_mux.vcd
```

We can view the waveform of a simple 2:1 mux which selects the input based on the select line

![Screenshot from 2024-10-21 23-15-54](https://github.com/user-attachments/assets/c7132abe-dbec-402b-b038-6ad1b8d818f5)

![Screenshot from 2024-10-21 23-16-48](https://github.com/user-attachments/assets/e45054d8-424c-4829-ba6c-877de342410d)

#### Access Module Files
To view the contents of the **good_mux** file run the following command
```
 vim tb_good_mux.v -o good_mux.v 
```

Design file and Testbench File

![Screenshot from 2024-10-21 23-18-25](https://github.com/user-attachments/assets/eb7af07d-003d-4d00-84bd-84667466f555)

![Screenshot from 2024-10-21 23-18-03](https://github.com/user-attachments/assets/0f8c9822-5c52-426b-ba05-9f009baddf07)

## Introduction to Yosys & Logic Synthesis

**Synthesizer** is a tool for converting the **RTL** to Netlist and here we are using the **Yosys** Synthesizer.

### Yosys SETUP

![Screenshot from 2024-10-21 11-10-21](https://github.com/user-attachments/assets/537bebbf-c706-46f3-84f2-f3d05a36656f)


### Verifying the Synthesis

![Screenshot from 2024-10-21 11-10-50](https://github.com/user-attachments/assets/a267721b-7a56-4396-ae22-671c7b0b62f3)

**Note**:- The set of Primary inputs / primary outputs will remain the same between the RTL design and Synthesis so we can use the same testbench.

## Logic Synthesis

RTL Design - behavioral representation in HDL form for the required specification.

 **Synthesis** - RTL to Gate level translation.
 The design is converted int gates and connections are made. This given outas a file called **netlist**.

**Liberty (.lib)** is a collection of logical modules, including basic logic gates like AND, OR, and NOT, with different variants such as 2-input, 3-input, and 4-input gates, as well as slow, medium, and fast versions. Fast cells are used for high-performance needs, while slower cells help address hold time issues. Using faster cells can increase area and power consumption and may cause hold time violations. On the other hand, overusing slower cells can degrade performance. Optimal cell selection during synthesis is based on constraints that balance area, power, and timing requirements.

![Screenshot from 2024-10-21 11-11-26](https://github.com/user-attachments/assets/0581e61c-704b-429f-bb03-2f115343d18c)

### Faster cells and Slower Cells

Cell delay in a digital logic circuit is influenced by the circuit's load, which is represented by capacitance. Faster charging or discharging of the capacitance results in a lower cell delay.

To speed up the charging/discharging process, wider transistors are used as they can provide more current, reducing cell delay. However, wider transistors also increase power consumption and area. On the other hand, narrower transistors reduce area and power consumption but lead to higher cell delay. Therefore, designing a circuit with low cell delay involves a trade-off between area and power.

![Screenshot from 2024-10-21 11-13-28](https://github.com/user-attachments/assets/b910f788-d5e3-4951-aaca-00eafaf3f7fe)

## Yosys flow

**1.**  start yosys.
          
```
yosys
```
![Screenshot from 2024-10-21 23-21-20](https://github.com/user-attachments/assets/81d73ff6-60b9-4e00-b9a5-7539855f71cb)


**2.** load the sky130 standard library.
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
![Screenshot from 2024-10-21 23-52-28](https://github.com/user-attachments/assets/4e85a896-b0f2-4cf7-9f05-10ccb6c0f049)

**3.** Read the design files
```
read_verilog good_mux.v
```
![Screenshot from 2024-10-21 23-22-38](https://github.com/user-attachments/assets/b5779804-acf0-4439-a1be-80a08891b5ef)


**4.** Synthesize the top level module
```
synth -top good_mux
```
![Screenshot from 2024-10-21 23-24-55](https://github.com/user-attachments/assets/479cfb0c-a487-4bf0-ab48-8e0432886559)

        
**5.** Map to the standard library
```
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
![Screenshot from 2024-10-21 23-25-23](https://github.com/user-attachments/assets/3f1d64d4-9b60-4cee-81a7-91fb949c7e3d)


**6.** Two view the result as a graphich use the show command.
```
show
```
![Screenshot from 2024-10-21 23-25-56](https://github.com/user-attachments/assets/14c8efc9-61e5-4507-b15a-b008368d7594)

**7.** To write the result netlist to a file use the write_veriog command. This will output the netlist to a file in the current directory.
```
write_verilog -noattr good_mux_netlist.v
```
![Screenshot from 2024-10-21 23-27-42](https://github.com/user-attachments/assets/2a7585eb-768a-4982-be4c-edbcb7c00cdf)

</details>




<details>  
<summary><strong>Day 2:</strong>Timing libs, hierarchical vs flat synthesis and efficient flop coding styles.</summary>

## Introduction to timing labs

Run the following commands to view the contents inside the .lib file:

```
cd ASIC/sky130RTLDesignAndSynthesisWorkshop/lib/

vim sky130_fd_sc_hd__tt_025C_1v80.lib

```
![Screenshot from 2024-10-22 00-13-47](https://github.com/user-attachments/assets/e30897a0-0b44-4845-af9f-03913def4804)



##  Cell library
 A standard cell library is a collection of characterized logic gates that can be used to implement digital circuits. The Liberty (.lib) files contain PVT parameters (Process, Voltage, Temperature) that can significantly impact circuit performance. Variations in manufacturing, changes in voltage, and fluctuations in temperature all play a role in affecting how the circuit functions.

![Screenshot from 2024-10-21 12-18-00](https://github.com/user-attachments/assets/e084adcb-14b2-4f9c-95e7-3a6c8a7ab6d8)

We can also find various flavours of AND gate

![Screenshot from 2024-10-22 00-15-50](https://github.com/user-attachments/assets/661954ad-5196-4272-8696-7da96dc68036)

![Screenshot from 2024-10-22 00-16-53](https://github.com/user-attachments/assets/85602a21-8eaf-4d21-ad35-3ff658cc49b3)

![Screenshot from 2024-10-22 00-17-13](https://github.com/user-attachments/assets/442f9542-681c-46f5-be79-e158e97add40)


We can observe that:
* and2_0 -- takes the least area, more delay and low power.
* and2_1 -- takes more area, less delay and high power.
* and2_2 -- takes the largest area, larger delay and highest power.

## Hierarchial synthesis vs Flat synthesis 

Hierarchical synthesis involves breaking down a complex design into various sub-modules, each of which is synthesized separately to produce gate-level netlists before being integrated. This approach enhances organization, allows for module reuse, and enables incremental design changes without impacting the entire system. In contrast, flat synthesis treats the entire design as a single unit during the synthesis process, resulting in a single netlist regardless of any hierarchical relationships. While flat synthesis can optimize certain designs, it becomes difficult to maintain, analyze, and modify the design due to its absence of structural modularity.

### Hierarchial synthesis  

Consider the verilog file ```multiple_modules.v``` which is given in the verilog_files directory
```
module sub_module2 (input a, input b, output y);
    assign y = a | b;
endmodule

module sub_module1 (input a, input b, output y);
    assign y = a&b;
endmodule


module multiple_modules (input a, input b, input c , output y);
    wire net1;
    sub_module1 u1(.a(a),.b(b),.y(net1));  //net1 = a&b
    sub_module2 u2(.a(net1),.b(c),.y(y));  //y = net1|c ,ie y = a&b + c;
endmodule
```
To perform hierarchical synthesis on the ```multiple_modules.v ``` file use the following commands:
```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog multiple_modules.v

synth -top multiple_modules

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show multiple_modules

write_verilog -noattr multiple_modules_hier.v

!vim multiple_modules_hier.v

```
When you do synth -top 'topmodulename' in yosys, it does an hierarchical synthesis. ie the different hierarchies between modules are preserved.

![Screenshot from 2024-10-22 00-19-41](https://github.com/user-attachments/assets/02e78d9e-aab8-4f19-9a32-4a06d57898df)


**Staistics of Multiple Modules**

![Screenshot from 2024-10-22 00-26-31](https://github.com/user-attachments/assets/dc544916-7657-4cf5-b7ab-428329a1b2c8)


**Realization of the Logic**
![Screenshot from 2024-10-22 00-27-10](https://github.com/user-attachments/assets/fba19846-50a8-4801-8565-506d80457fb6)



**Map to the standard library**

![Screenshot from 2024-10-22 00-26-52](https://github.com/user-attachments/assets/18eaae15-cf69-464b-8b2d-94085d8bf650)



**Netlist file**

![Screenshot from 2024-10-22 00-42-29](https://github.com/user-attachments/assets/b573206b-5e66-442d-aa1d-7624c1698a80)




#### Flat synthesis  
Merges all hierarchical modules in the design into a single module to create a flat netlist. To perform flat synthesis on the ```multiple_modules.v``` file type the following commands:
```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog multiple_modules.v

synth -top multiple_modules

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

flatten

show

write_verilog -noattr multiple_modules_flat.v

!vim multiple_modules_flat.v
```
![Screenshot from 2024-10-22 00-44-59](https://github.com/user-attachments/assets/d7395b7b-65e7-454a-909d-bc6bbc1b3d61)


**Realization of the Logic**

![Screenshot from 2024-10-22 00-45-42](https://github.com/user-attachments/assets/dd416b57-9833-4123-aae7-ef9224b780f7)

 
**Netlist file**

![Screenshot from 2024-10-22 00-46-16](https://github.com/user-attachments/assets/73502948-dbcd-4937-aced-f42e3150692c)



### Sub Module Level Synthesis
This method is preferred when multiple instances of same module are used. The synthesis is carried out once and is replicate multiple times, and the multiple instances of the same module are stitched together in the top module. This method is helpful when making use of divide and conquer algorithm


 ```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog multiple_modules.v

synth -top sub_module1

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show
```
![Screenshot from 2024-10-22 00-48-17](https://github.com/user-attachments/assets/e8b4f026-2c49-425d-ae27-c94c6c65a09b)



**Realization of the Logic**

![Screenshot from 2024-10-22 00-48-30](https://github.com/user-attachments/assets/24cc93ef-8771-4056-b563-3ee3cc46c377)




## Flop coding styles and optimization

Flip-Flops are an essential part of sequential logic in a circuit and here we explore the design and synthesis of various types of flip-flops. To prevent glitches in digital circuits, we use flip-flops to store intermediate values. This ensures that combinational circuit inputs remain stable until the clock edge, avoiding glitches and maintaining correct operation:

### Asynchronous Reset/set:

**Verilog Code for Asynchronous Reset:**

```
module dff_asyncres ( input clk ,  input async_reset , input d , output reg q );
always @ (posedge clk , posedge async_reset)
begin
	if(async_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```
**Verilog Code Asynchronous Set:**

```
module dff_async_set ( input clk ,  input async_set , input d , output reg q );
always @ (posedge clk , posedge async_set)
begin
	if(async_set)
		q <= 1'b1;
	else	
		q <= d;
end
endmodule
```

In this design, the `always` block is triggered by changes in the clock or the reset signal. The circuit is sensitive to the positive edge of the clock. When the reset/set signal goes low or high, the signal on the `q` line changes accordingly. Therefore, the behavior associated with the reset/set occurs immediately and does not wait for the positive edge of the clock.

### Synchronous Reset:

```
module dff_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
always @ (posedge clk )
begin
	if (sync_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```

#### FLIP FLOP SIMULATION

```
iverilog dff_asyncres.v tb_dff_asyncres.v 

ls

./a.out

gtkwave tb_dff_asyncres.vcd
```


**GTK WAVE OF ASYNCHRONOUS RESET**

![Screenshot from 2024-10-22 00-52-37](https://github.com/user-attachments/assets/2839b0d2-c942-4782-b746-f629a637581c)


**GTK WAVE OF ASYNCHRONOUS SET**

![Screenshot from 2024-10-22 00-52-42](https://github.com/user-attachments/assets/d7982dbc-01de-4f1a-8c2a-5dc0327574b9)


**GTK WAVE OF SYNCHRONOUS RESET**

![Screenshot from 2024-10-22 01-05-58](https://github.com/user-attachments/assets/14f23f78-a8cf-4bd3-a5f5-a32f073934c5)


#### FLIP FLOP SYNTHESIS

```

yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_asyncres.v

synth -top dff_asyncres

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show 
```
**Statistics of D FLipflop with Asynchronous Reset**

![Screenshot from 2024-10-22 01-10-55](https://github.com/user-attachments/assets/d6ca6259-9772-4b6f-9d08-14ef91a6d04a)

![Screenshot from 2024-10-22 01-11-41](https://github.com/user-attachments/assets/488ea1eb-5749-4d8d-9c3f-a08ad6f0a6d5)


**Realization of Logic**
![Screenshot from 2024-10-22 01-11-53](https://github.com/user-attachments/assets/c377c3d0-6a53-4b4b-8b9a-57b60e797334)


**Note:**  We wrote a flop with active high reset but the flop is having acting low reset so the tool inserted the inverter so (!(!(reset))) is just reset so at the end we got a flop with active high reset

**Statistics of D FLipflop with Asynchronous set**\
Follow the same steps as given above just the file name changes to dff_async_set.v

![Screenshot from 2024-10-22 01-15-55](https://github.com/user-attachments/assets/8ede4e67-1148-4588-b2ab-a5244e864807)


**Realization of Logic**

![Screenshot from 2024-10-22 01-16-18](https://github.com/user-attachments/assets/97c61ce4-00cf-4c48-b07c-f2d4e73d12b1)


**Note:**  We wrote a flop with active high set but the flop is having acting low set so the tool inserted the inverter so (!(!(set))) is just set so at the end we got a flop with active high set

**Statistics of D FLipflop with Synchronous Reset**

![Screenshot from 2024-10-22 01-17-13](https://github.com/user-attachments/assets/097fc8d1-2157-4c57-ab9f-f1d24904eb5c)


![Screenshot from 2024-10-22 01-17-26](https://github.com/user-attachments/assets/c5fd8509-3673-48e4-af72-1fc1230194cd)


**Realization of Logic**

![Screenshot from 2024-10-22 01-17-35](https://github.com/user-attachments/assets/cc74779a-bdb1-4acf-85a2-ded145cdf50c)



## Optimizations

### Example 1: mult_2.v 

**verilog code**

```
module mul2 (input [2:0] a, output [3:0] y);
assign y = a * 2;
endmodule
```

**truth table**

![Screenshot from 2024-10-21 15-35-31](https://github.com/user-attachments/assets/b53852d4-4db4-417e-8532-8f5e453fc84a)

We can see the multiplication of a number by 2 doesnt really need any extra hardware we just need to append the LSB's with zeroes and the remaining bits are the input bits of same, It can be realised by grouding the LSB's and wiring the input properly to the output.

Run the below code to view the netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog mult_2.v

synth -top mul2

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show 

write_verilog -noattr mult_2_net.v

!vim mult_2_net.v
```
**Statistics**

![Screenshot from 2024-10-22 01-20-58](https://github.com/user-attachments/assets/687bbc17-d6bb-4a29-a682-7204e7b271af)


**Realization of Logic**

![Screenshot from 2024-10-22 01-21-34](https://github.com/user-attachments/assets/e7afd25f-a96d-49ba-8ade-6457838143d3)

**Netlist**

![Screenshot from 2024-10-22 01-22-22](https://github.com/user-attachments/assets/6939a484-4129-4019-8b25-9ea211015c39)


### Example 2: mult_8.v

**verilog code**

```
module mult8 (input [2:0] a , output [5:0] y);
	assign y = a * 9;
endmodule
```

**logic behaviour**

![Screenshot from 2024-10-21 15-43-38](https://github.com/user-attachments/assets/a66a1c34-977d-446c-a462-fae3073a1425)


In this design the 3-bit input number "a" is multiplied by 9 i.e (a9) which can be re-written as (a8) + a . The term (a8) is nothing but a left shifting the number a by three bits. Consider that a = a2 a1 a0. (a8) results in a2 a1 a0 0 0 0. (a9)=(a8)+a = a2 a1 a0 a2 a1 a0 = aa(in 6 bit format). Hence in this case no hardware realization is required. The synthesized netlist of this design is shown below:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog mult_8.v

synth -top mult8

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr mult_8_net.v

```

**Statestics** 

![Screenshot from 2024-10-22 01-23-44](https://github.com/user-attachments/assets/5a8c3707-9a4d-426e-91e2-88f754a26b63)



**Realization of Logic**

![Screenshot from 2024-10-22 01-24-20](https://github.com/user-attachments/assets/1800ce2e-8318-4dc1-8826-c94fa278c57f)



**Netlist**
 
![Screenshot from 2024-10-22 01-25-13](https://github.com/user-attachments/assets/07e263ab-efe4-4973-8392-bc87848464d0)




</details>


<details>
<summary><strong>Day 3:</strong>Introduction to Combinational and sequential optimizations.</summary>

##  Introduction to Combinational Logic Optimization and sequential Optimization
There are two types of optimisations: Combinational and Sequential optimisations. These optimisations are done inorder to achieve designs that are efficient in terms of area, power, and performance.

**Combinational Optimization**

The techiniques used are:

- Constant Propagation (Direct Optimisation)
- Boolean Logic Optimisation (using K-Map or Quine McCluskey method)

**Constant Propagation:**

Consider the below circuit:

![image](https://github.com/user-attachments/assets/24fcec7b-7b46-4d73-b93d-a257883ef6e5)

The top circuit uses 6 transistors (3 NMOS and 3 PMOS), while the bottom circuit uses 2 transistors (1 NMOS and 1 PMOS) when input A is set to zero, turning the logic into an inverter.

**Boolean Logic Optimisation:**

Consider the below verilog code:

```assign y = a?(b?c:(c?a:0)):(!c)```

The ternary operator (?:) will make the circuit behave like a mux upon synthesis as shown below. 

![Screenshot from 2024-10-21 16-15-29](https://github.com/user-attachments/assets/77afa517-d6e6-498e-be71-f5dac5c0d777)


The circuit can be optimised as follows:

![Screenshot from 2024-10-21 16-15-54](https://github.com/user-attachments/assets/e5cfb3b9-1e20-4a13-ad2e-3a4774e03632)


**Example 1:**

Verilog code:

```
module opt_check (input a , input b , output y);
	assign y = a?b:0;
endmodule
```

The above code infers a multiplexer and since one of the inputs of the multiplexer is always connected to the ground it will infer an AND gate on optimisation.

Run the following commands for netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog opt_check.v

synth -top opt_check

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr opt_check_net.v

!vim opt_ckeck_net.v
```

![Screenshot from 2024-10-22 02-10-58](https://github.com/user-attachments/assets/7d52fc7e-b4c8-495a-ba3d-863f0c609776)


![31](https://github.com/user-attachments/assets/c925acbe-5385-4c4f-a3b9-aa69c00d3f58)


**Example 2:**

Verilog code:

```
module opt_check2 (input a , input b , output y);
	assign y = a?1:b;
endmodule
```

Since one of the inputs of the multiplexer is always connected to the logic 1 it will infer an OR gate on optimisation.The OR gate will be NAND implementation since NOR gate has stacked pmos while NAND implementation has stacked nmos.

Run the below code for netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog opt_check2.v

synth -top opt_check2

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr opt_check2_net.v

!vim opt_ckeck2_net.v
```

![Screenshot from 2024-10-22 02-13-44](https://github.com/user-attachments/assets/aa1e0b62-51bb-4304-8f24-094f79b22b3f)


![32](https://github.com/user-attachments/assets/08fe3c54-faf2-4093-ba77-83bc2971c509)


**Example 3:**

Verilog code:

```
module opt_check3 (input a , input b, input c , output y);
	assign y = a?(c?b:0):0;
endmodule
```

On optimisation the above design becomes a 3 input AND gate.

Run the below code for netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog opt_check3.v

synth -top opt_check3

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr opt_check3_net.v

!vim opt_ckeck3_net.v
```

![Screenshot from 2024-10-22 02-16-37](https://github.com/user-attachments/assets/3e7e8b97-19c0-421d-8510-dcbae2e8c9ee)

![33](https://github.com/user-attachments/assets/9764687e-9c97-46a6-bdc9-78e93d9bac15)

**Example 4:**

Verilog code:

```
module opt_check4 (input a , input b, input c , output y);
	assign y = a?(c?b:0):0;
endmodule
```

On optimisation the above design becomes a 2 input XNOR gate.

Run the below code for netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog opt_check4.v

synth -top opt_check4

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr opt_check4_net.v

!vim opt_ckeck4_net.v
```
![Screenshot from 2024-10-22 02-18-33](https://github.com/user-attachments/assets/c8ac3fd1-a5ec-4ec3-bf42-beb31ab977d9)

![34](https://github.com/user-attachments/assets/a9c21325-aba8-4e4c-89d6-680b7b38bcfc)

**Example 5:**

Verilog code:

```
module sub_module1(input a , input b , output y);
 assign y = a & b;
endmodule

module sub_module2(input a , input b , output y);
 assign y = a^b;
endmodule

module multiple_module_opt(input a , input b , input c , input d , output y);
wire n1,n2,n3;

sub_module1 U1 (.a(a) , .b(1'b1) , .y(n1));
sub_module2 U2 (.a(n1), .b(1'b0) , .y(n2));
sub_module2 U3 (.a(b), .b(d) , .y(n3));

assign y = c | (b & n1); 

endmodule
```

On optimisation the above design becomes a AND OR gate

Run the below code for netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog multiple_module_opt.v

synth -top multiple_module_opt

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

flatten

show

write_verilog -noattr multiple_module_opt_net.v

!vim multiple_module_opt_net.v
```

![Screenshot from 2024-10-22 02-19-49](https://github.com/user-attachments/assets/05449e0a-616a-4a29-9a47-800e1f76434d)


![35](https://github.com/user-attachments/assets/05b7a73b-546a-4fff-b1d9-052d4328a81d)



**Example 6:**

Verilog code:

```
module sub_module(input a , input b , output y);
	assign y = a & b;
endmodule

module multiple_module_opt2(input a , input b , input c , input d , output y);
		wire n1,n2,n3;
	sub_module U1 (.a(a) , .b(1'b0) , .y(n1));
	sub_module U2 (.a(b), .b(c) , .y(n2));
	sub_module U3 (.a(n2), .b(d) , .y(n3));
	sub_module U4 (.a(n3), .b(n1) , .y(y));
endmodule
```

On optimisation the above design becomes Y=0 

Run the below code for netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog multiple_module_opt2.v

synth -top multiple_module_opt2

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

flatten

show

write_verilog -noattr multiple_module_opt2_net.v

!vim multiple_module_opt_net2.v
```

![Screenshot from 2024-10-22 02-20-42](https://github.com/user-attachments/assets/d8c22380-808b-4d83-ac80-6948cd1bc58e)



![36](https://github.com/user-attachments/assets/0416007d-9701-426c-b020-ca7144719062)


**Sequential Logic Optimizations**

**Example 1:**

Verilog code:

```
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b0;
	else
		q <= 1'b1;
end
endmodule
```

Run the below code for netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_const1.v

synth -top dff_const1

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr dff_const1_net.v

!vim dff_const1_net.v
```


![Screenshot from 2024-10-22 02-21-58](https://github.com/user-attachments/assets/cf35c36b-e3ff-45f1-87b5-ff9e53052d68)


![Screenshot from 2024-10-21 17-50-00](https://github.com/user-attachments/assets/42f1d861-1789-40b5-9ad3-9bfab4d82e59)


GTKWave Output:

```
iverilog dff_const1.v tb_dff_const1.v

./a.out

gtkwave tb_dff_const1.vcd

```
![Screenshot from 2024-10-22 02-24-50](https://github.com/user-attachments/assets/dd219022-6899-4514-819f-7c446f665364)

**Example 2:**

Verilog code:

```
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b1;
	else
		q <= 1'b1;

end
endmodule
```

Run the below code for netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_const2.v

synth -top dff_const2

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr dff_const2_net.v

!vim dff_const2_net.v
```

![Screenshot from 2024-10-22 02-25-30](https://github.com/user-attachments/assets/3f557fb0-6129-4b26-94bd-d6509c98fac0)

![Screenshot from 2024-10-21 17-57-07](https://github.com/user-attachments/assets/437dc4e6-5ed5-4aa7-8cd9-716219293ae0)


GTKWave Output:

```
iverilog dff_const2.v tb_dff_const2.v

./a.out

gtkwave tb_dff_const2.vcd
```

![Screenshot from 2024-10-22 02-26-44](https://github.com/user-attachments/assets/77bd5984-edda-4dc6-85e8-0a9237ad643f)


**Example 3:**

Verilog code:

```
module dff_const3(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b0;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end
endmodule
```

Run the below code for netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_const3.v

synth -top dff_const3

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr dff_const3_net.v
```


![Screenshot from 2024-10-22 02-27-28](https://github.com/user-attachments/assets/0d01ecca-9fec-4ffd-8aab-83b9824cdc39)

![Screenshot from 2024-10-21 18-02-49](https://github.com/user-attachments/assets/5d0dbaa6-03d6-4650-9488-5ad58ddf6aaa)


GTKWave Output:

```
iverilog dff_const3.v tb_dff_const3.v

./a.out

gtkwave tb_dff_const3.vcd
```

![Screenshot from 2024-10-22 02-28-23](https://github.com/user-attachments/assets/d5c16cfd-3403-4cba-bc74-b38cd01d937f)


**Example 4:**

Verilog code:

```
module dff_const4(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b1;
	end
else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end
endmodule
```

Run the below code for netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_const4.v

synth -top dff_const4

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr dff_const4_net.v
```
![Screenshot from 2024-10-22 02-29-16](https://github.com/user-attachments/assets/418f6a87-be4b-4fd1-ad71-cb65217f9a8b)


![Screenshot from 2024-10-21 18-03-58](https://github.com/user-attachments/assets/0af3ff11-2c59-49ff-b9dd-49707a344c78)

 
GTKWave Output:

```
iverilog dff_const4.v tb_dff_const4.v

./a.out

gtkwave tb_dff_const4.vcd
```

![Screenshot from 2024-10-22 02-30-10](https://github.com/user-attachments/assets/87667026-f41a-497a-8e10-53e275c4a867)

**Example 5:**

Verilog code:

```
module dff_const5(input clk, input reset, output reg q);
reg q1;
always @(posedge clk, posedge reset)
	begin
		if(reset)
		begin
			q <= 1'b0;
			q1 <= 1'b0;
		end
	else
		begin
			q1 <= 1'b1;
			q <= q1;
		end
	end
endmodule
```

Run the below code for netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_const5.v

synth -top dff_const5

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr dff_const5_net.v
```

![Screenshot from 2024-10-22 02-30-52](https://github.com/user-attachments/assets/134299b2-74c6-4e5b-937f-878bad550b88)

![Screenshot from 2024-10-21 18-04-56](https://github.com/user-attachments/assets/1e6b6509-ede0-43e2-b877-7ab4f5047733)


GTKWave Output:

```
iverilog dff_const5.v tb_dff_const5.v

./a.out

gtkwave tb_dff_const5.vcd
```
![Screenshot from 2024-10-22 02-32-02](https://github.com/user-attachments/assets/1f84d234-3f56-4ddc-ab25-5fe610b39156)


**Sequential Logic Optimizations for unused outputs**

**Example 1:**

Verilog code:

```
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = count[0];
always @(posedge clk ,posedge reset)
begin
	if(reset)
		count <= 3'b000;
	else
		count <= count + 1;
end
endmodule
```

Run the below code for netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog counter_opt.v

synth -top counter_opt

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr counter_opt_net.v
```

![Screenshot from 2024-10-22 02-33-05](https://github.com/user-attachments/assets/5dd2d5fc-f7a2-4fa2-8cd8-71f96520865f)

![Screenshot from 2024-10-21 18-05-56](https://github.com/user-attachments/assets/d1fd37cc-af90-4aa7-9af2-20618f664a21)


GTKWave Output:

```
iverilog counter_opt.v tb_counter_opt.v

./a.out

gtkwave tb_counter_opt.vcd
```

![Screenshot from 2024-10-22 02-35-50](https://github.com/user-attachments/assets/50826c73-1dc9-4ea1-a84e-6a60754678f2)

Modified counter logic:

Verilog code:

```
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = {count[2:0]==3'b100};
always @(posedge clk ,posedge reset)
begin
if(reset)
	count <= 3'b000;
else
	count <= count + 1;
end
endmodule
```
Run the below code for netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog counter_opt2.v

synth -top counter_opt2

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr counter_opt_net2.v

```
![image](https://github.com/user-attachments/assets/6b0daf62-32e0-486e-adfd-71fd8c8edac6)

![Screenshot from 2024-10-21 18-07-39](https://github.com/user-attachments/assets/758cc273-b58d-4286-a891-bf9f6756c58e)


GTKWave Output:

```
iverilog counter_opt2.v tb_counter_opt2.v

./a.out

gtkwave tb_counter_opt2.vcd
```

![Screenshot from 2024-10-21 18-10-22](https://github.com/user-attachments/assets/ae5620f9-2bbb-4a74-bddb-1da1d42d427c)




</details>

<details>
<summary><strong>Day 4:</strong> GLS, blocking vs non-blocking and Synthesis-Simulation mismatch.</summary>

## Introduction to GLS and Synthesis-Simulation mismatch

Gate Level Simulation (GLS) is an important step in verifying digital circuits. It simulates the synthesized netlist, a lower-level representation of the design, using a testbench to check its logical accuracy and timing behavior. By comparing the simulation outputs with expected results, GLS ensures the synthesis process hasn't introduced any errors and that the design meets performance requirements.

Sensitivity lists are key for ensuring correct circuit behavior. An incomplete sensitivity list can result in unintended latches. Blocking and non-blocking assignments in `always` blocks behave differently, and improper use of blocking assignments can inadvertently create latches, leading to mismatches between synthesis and simulation. To prevent this, it's crucial to carefully review the circuit behavior and ensure the sensitivity list and assignments match the intended functionality.

![image](https://github.com/user-attachments/assets/fa59f230-4a0d-4c8e-9353-a01a9209a6d6)

**Example 1:**

Verilog code:

```
module ternary_operator_mux (input i0 , input i1 , input sel , output y);
assign y = sel?i1:i0;
endmodule
```

Simulation:

```
iverilog ternary_operator_mux.v tb_ternary_operator_mux.v

./a.out

gtkwave tb_ternary_operator_mux.vcd

```
![Screenshot from 2024-10-22 03-01-35](https://github.com/user-attachments/assets/7ad36936-16b0-433c-9ce6-9a5d2712af00)

Netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog ternary_operator_mux.v

synth -top ternary_operator_mux

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr ternary_operator_mux_net.v

```

![Screenshot from 2024-10-22 03-02-57](https://github.com/user-attachments/assets/05377ee5-5a1d-489f-b473-9889a8f6f1f8)

![Screenshot from 2024-10-22 03-02-42](https://github.com/user-attachments/assets/5f5748dc-f143-4d13-a8a9-1150654954cf)

![Screenshot from 2024-10-22 03-03-39](https://github.com/user-attachments/assets/11606d89-3c87-441a-8d3c-9926b8dc78b2)


GLS:

```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v

./a.out

gtkwave tb_ternary_operator_mux.vcd
```
![Screenshot from 2024-10-22 03-04-52](https://github.com/user-attachments/assets/0a5d3723-a1f5-41b3-89e7-a388e5756028)


In this case there is no mismatch between the waveforms before and after synthesis

**Example 2:**

Verilog code:

```
module bad_mux (input i0 , input i1 , input sel , output reg y);
always @ (sel)
begin
	if(sel)
		y <= i1;
	else 
		y <= i0;
end
endmodule
```

Simulation:

```
iverilog bad_mux.v tb_bad_mux.v

./a.out

gtkwave tb_bad_mux.vcd
```

![Screenshot from 2024-10-22 03-06-01](https://github.com/user-attachments/assets/ae7486ce-3a51-4195-828c-de76eb0898d7)


Netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog bad_mux.v

synth -top bad_mux

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr bad_mux_net.v

```
![Screenshot from 2024-10-22 03-07-01](https://github.com/user-attachments/assets/580b8dc6-43e5-40a1-a691-de94ab34b589)

![Screenshot from 2024-10-22 03-06-49](https://github.com/user-attachments/assets/5957ae59-ffb8-40d6-a361-fa13b74faa71)

![Screenshot from 2024-10-22 03-07-34](https://github.com/user-attachments/assets/019e61f9-8255-40fd-b909-b0f065d9919f)



GLS:

```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v

./a.out

gtkwave tb_bad_mux.vcd
```
![Screenshot from 2024-10-22 03-09-28](https://github.com/user-attachments/assets/3b9c8918-c592-4230-a510-56f37247e4b0)


In this case there is a synthesis and simulation mismatch. While performing synthesis yosys has corrected the sensitivity list error.

**Labs on Synthesis-Simulation mismatch for blocking statements**

Verilog code:

```
module blocking_caveat (input a , input b , input  c, output reg d); 
reg x;
always @ (*)
begin
d = x & c;
x = a | b;
end
endmodule
```

Simulation:

```
iverilog blocking_caveat.v tb_blocking_caveat.v

./a.out

gtkwave tb_blocking_caveat.vcd
```
![Screenshot from 2024-10-22 03-10-52](https://github.com/user-attachments/assets/8c3967e9-6c71-4cf8-84fa-f4c9e8d4efd8)


Netlist:

```
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog blocking_caveat.v

synth -top blocking_caveat

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr blocking_caveat_net.v
```
![Screenshot from 2024-10-22 03-11-49](https://github.com/user-attachments/assets/aa31f4f9-82e7-44b1-b63c-48da2a1ef775)

![Screenshot from 2024-10-22 03-11-37](https://github.com/user-attachments/assets/3df7f963-8320-4498-ab35-0e8282abbe29)

![Screenshot from 2024-10-22 03-13-06](https://github.com/user-attachments/assets/ca1afb7c-bac4-4bea-9ff3-b32b3bb3263f)


GLS:

```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_caveat_net.v tb_blocking_caveat.v

./a.out

gtkwave tb_blocking_caveat.vcd
```
![Screenshot from 2024-10-22 03-14-06](https://github.com/user-attachments/assets/19fb879b-9ae6-4b2c-aeb9-0d26ff5d4390)


In this case there is a synthesis and simulation mismatch. While performing synthesis yosys has corrected the latch error.

</details>

</details>

---
LAB SESSION - 10
---

## Synthesizing RISC-V and comparing output with functional (RTL) simulation.


### Steps 

* Copy the ```src``` folder from your ```BabySoC``` folder to your ```sky130RTLDesignAndSynthesisWorkshop``` folder in your ```VLSI``` folder from previous lab.

Now go the following Directory: 

```
cd /home/jack/VLSI/sky130RTLDesignAndSynthesisWorkshop/src/module
```

### Synthesis: 

```
yosys       

read_liberty -lib /home/jack/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog clk_gate.v

read_verilog rvmyth.v

synth -top rvmyth

abc -liberty /home/jack/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

write_verilog -noattr rvmyth_net.v

!gvim rvmyth_net.v

exit
```

![Screenshot from 2024-10-24 01-32-08](https://github.com/user-attachments/assets/bddefbb4-7bce-4d48-8d88-9707260eae3f)

![Screenshot from 2024-10-24 01-33-13](https://github.com/user-attachments/assets/57786619-5642-4168-93b8-01913d486d89)

![Screenshot from 2024-10-24 01-37-19](https://github.com/user-attachments/assets/76340291-814b-43ad-a138-b33479703ad9)



Now to observe the output waveform of synthesised RISC-V
```
iverilog ../../my_lib/verilog_model/primitives.v ../../my_lib/verilog_model/sky130_fd_sc_hd.v rvmyth.v testbench.v vsdbabysoc.v avsddac.v avsdpll.v clk_gate.v

./a.out

gtkwave dump.vcd
```

![image](https://github.com/user-attachments/assets/9d4ee5d0-6908-4137-b76b-ca8be24e3c3a)

![Screenshot from 2024-10-24 01-40-13](https://github.com/user-attachments/assets/27e862eb-a7bc-49d4-bd6d-ee92ff1960c4)

![Screenshot from 2024-10-24 01-41-20](https://github.com/user-attachments/assets/2ff43905-31ac-493b-a523-f454776470df)


### Realization: 

![PHOTO-2024-10-24-01-55-37](https://github.com/user-attachments/assets/f400bd2e-0285-4dcf-991a-46662df07a2b)

![PHOTO-2024-10-24-01-56-14](https://github.com/user-attachments/assets/9cb37544-d272-42b8-9006-3f3e86f2fc13)

![PHOTO-2024-10-24-02-46-15](https://github.com/user-attachments/assets/2319d29c-ec92-4d2b-8c91-677e2ced564f)



## RTL Simulations

 ### Command Steps :
 ```
cd BabySoC

iverilog -o ./pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module/

./pre_synth_sim.out

gtkwave pre_synth_sim.vcd

```
![Screenshot from 2024-10-24 01-48-25](https://github.com/user-attachments/assets/07b9f63f-9e73-4ad6-9520-9f0eca7277e3)

![Screenshot from 2024-10-24 01-25-43](https://github.com/user-attachments/assets/a3e23876-147d-4dc9-93f4-71df7d96f245)

![Screenshot from 2024-10-24 01-26-11](https://github.com/user-attachments/assets/d4e8d840-abe9-4f4f-a270-54de11c0423d)

## LAB SESSION - 11

### Static Timing Analysis (STA)
Static Timing Analysis (STA) is a method used to verify the timing performance of digital circuits without needing dynamic simulation. It evaluates whether a circuit meets timing requirements by examining the paths that data takes from inputs to outputs, taking into account the delays caused by gates and interconnections. STA detects setup and hold time violations, ensuring that data remains stable around clock edges. It also considers clock characteristics such as frequency, skew, and jitter, and operates under worst-case delay scenarios to ensure reliability under different conditions. Tools like Synopsys PrimeTime and Cadence Tempus automate this analysis, helping to identify timing problems early to ensure dependable circuit operation at desired speeds.

STA is crucial in digital design for multiple reasons. It guarantees that data signals propagate within specified time limits for accurate outputs and detects violations, ensuring that flip-flops and sequential elements operate reliably. STA helps optimize performance by identifying critical paths that limit the maximum operating frequency and facilitates early detection of timing issues, reducing the need for expensive redesigns. It also aids in balancing performance with power efficiency by examining how changes in clock frequency affect power consumption. Automated STA tools efficiently manage complex designs, accounting for variations in manufacturing, temperature, and voltage to maintain consistent performance.

### Reg-to-Reg Path
A reg-to-reg path links two sequential elements, like flip-flops, in a digital circuit. It is a crucial aspect of STA, as it verifies data transfer between registers via combinational logic. Reg-to-reg paths are essential for ensuring accurate data transfer and synchronization, especially in pipelined or sequential designs. STA assesses compliance with setup and hold times, ensuring data stability at the registers. The timing analysis also considers delays through the intervening combinational logic, with critical reg-to-reg paths affecting the circuit's maximum frequency. When registers operate in different clock domains, STA must address additional factors such as metastability and synchronization.

### Clk-to-Reg Path
A clk-to-reg path links a clock signal to a register, ensuring that the register functions correctly in response to clock edges. This path assesses the delay from the clock's source to the register, factoring in delays through buffers or routing. During setup analysis, it determines the timing for when data must arrive at the register in relation to the clock edge. Clock delay influences the timing of data capture, and both setup and hold timing are examined, particularly in the context of clock jitter or variations. When registers operate in different clock domains, additional considerations are required to maintain reliable synchronization.

### STA for synthesized Risc-V core using time period of 11.8 ns.
To verify that our synthesized RISC-V Core module meets its timing constraints, we will generate setup and hold timing reports. These reports will confirm that data signals propagate correctly throughout the core. Run the following commands:
```
set PERIOD 11.8

set_units -time ns

create_clock [get_pins {pll/CLK}] -name clk -period $PERIOD
set_clock_uncertainty -setup  [expr $PERIOD * 0.05] [get_clocks clk]
set_clock_transition [expr $PERIOD * 0.05] [get_clocks clk]
set_clock_uncertainty -hold [expr $PERIOD * 0.08] [get_clocks clk]
set_input_transition [expr $PERIOD * 0.08] [get_ports ENb_CP]
set_input_transition [expr $PERIOD * 0.08] [get_ports ENb_VCO]
set_input_transition [expr $PERIOD * 0.08] [get_ports REF]
set_input_transition [expr $PERIOD * 0.08] [get_ports VCO_IN]
set_input_transition [expr $PERIOD * 0.08] [get_ports VREFH]
```

Now, run the below commands:
```
cd VSDBabySoc/src

sta

read_liberty -min ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_liberty -min ./lib/avsdpll.lib

read_liberty -min ./lib/avsddac.lib

read_liberty -max ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_liberty -max ./lib/avsdpll.lib

read_liberty -max ./lib/avsddac.lib

read_verilog ../output/synth/vsdbabysoc.synth.v

link_design vsdbabysoc

read_sdc ./sdc/vsdbabysoc_synthesis.sdc

report_checks -path_delay min_max -format full_clock_expanded -digits 4
```

![Screenshot from 2024-10-28 23-48-14](https://github.com/user-attachments/assets/d9caae7f-f07e-4917-9cb4-8cecbb9f0222)

Setup Time:

![image(1)](https://github.com/user-attachments/assets/c98643f1-0bfd-4872-86c7-67d3f258ecd0)


Hold Time:

![image](https://github.com/user-attachments/assets/49a57c30-fdb4-49ec-81e0-d364b02b3c47)


<details>
<summary><strong>Lab Session 12:</strong>Post Synthesis Static Timing Analysis comparison for various sky libraries.</summary>

## STA comparison for various sky libraries

Store all the sky `lib` files in a folder named `timing_libs`. Now, go to `VSDBabySoC/src` and create a tickle file `sta_across_pvt.tcl` . The below block is the content of the tickle file:

```
set list_of_lib_files(1) "sky130_fd_sc_hd__ff_100C_1v65.lib"
set list_of_lib_files(2) "sky130_fd_sc_hd__ff_100C_1v95.lib"
set list_of_lib_files(3) "sky130_fd_sc_hd__ff_n40C_1v56.lib"
set list_of_lib_files(4) "sky130_fd_sc_hd__ff_n40C_1v65.lib"
set list_of_lib_files(5) "sky130_fd_sc_hd__ff_n40C_1v76.lib"
set list_of_lib_files(6) "sky130_fd_sc_hd__ff_n40C_1v95.lib"
set list_of_lib_files(7) "sky130_fd_sc_hd__ff_n40C_1v95_ccsnoise.lib.part1"
set list_of_lib_files(8) "sky130_fd_sc_hd__ff_n40C_1v95_ccsnoise.lib.part2"
set list_of_lib_files(9) "sky130_fd_sc_hd__ff_n40C_1v95_ccsnoise.lib.part3"
set list_of_lib_files(10) "sky130_fd_sc_hd__ss_100C_1v40.lib"
set list_of_lib_files(11) "sky130_fd_sc_hd__ss_100C_1v60.lib"
set list_of_lib_files(12) "sky130_fd_sc_hd__ss_n40C_1v28.lib"
set list_of_lib_files(13) "sky130_fd_sc_hd__ss_n40C_1v35.lib"
set list_of_lib_files(14) "sky130_fd_sc_hd__ss_n40C_1v40.lib"
set list_of_lib_files(15) "sky130_fd_sc_hd__ss_n40C_1v44.lib"
set list_of_lib_files(16) "sky130_fd_sc_hd__ss_n40C_1v60.lib"
set list_of_lib_files(17) "sky130_fd_sc_hd__ss_n40C_1v60_ccsnoise.lib.part1"
set list_of_lib_files(18) "sky130_fd_sc_hd__ss_n40C_1v60_ccsnoise.lib.part2"
set list_of_lib_files(19) "sky130_fd_sc_hd__ss_n40C_1v60_ccsnoise.lib.part3"
set list_of_lib_files(20) "sky130_fd_sc_hd__ss_n40C_1v76.lib"
set list_of_lib_files(21) "sky130_fd_sc_hd__tt_025C_1v80.lib"
set list_of_lib_files(22) "sky130_fd_sc_hd__tt_100C_1v80.lib"

for {set i 1} {$i <= [array size list_of_lib_files]} {incr i} {
read_liberty ./timing_libs/$list_of_lib_files($i)
read_verilog ../output/synth/vsdbabysoc.synth.v
link_design vsdbabysoc
read_sdc ./sdc/vsdbabysoc_synthesis.sdc
check_setup -verbose
report_checks -path_delay min_max -fields {nets cap slew input_pins fanout} -digits {4} > ./sta_output/min_max_$list_of_lib_files($i).txt

}

```
![Screenshot from 2024-11-05 02-00-05](https://github.com/user-attachments/assets/798301a7-72d6-4a21-b520-df3f4ade8fe7)

constraints file:

![Screenshot from 2024-11-05 02-02-24](https://github.com/user-attachments/assets/21984a0a-fabf-4d0a-ae8c-49a838dcfd57)


Now, run the following commands:

```
cd VSDBabySoC/src

sta

source sta_across_pvt.tcl
```
![Screenshot from 2024-11-05 02-03-30](https://github.com/user-attachments/assets/848f27c8-23a2-479c-8810-191f3915a221)

table:

![Screenshot from 2024-11-05 01-26-17](https://github.com/user-attachments/assets/1f55bc20-569d-49d4-8dd6-8a416934e22e)


Graphs:

**Worst Setup Slack:**

![Screenshot from 2024-11-05 01-26-53](https://github.com/user-attachments/assets/d437bba9-82b5-4295-adec-157f6be437bb)


**Worst Hold Slack:**

![Screenshot from 2024-11-05 01-27-08](https://github.com/user-attachments/assets/e2807021-5f28-47f9-8d1c-5631551735ea)


<details>
<summary><strong>Timing Reports:</strong></summary>
	
**sky130_fd_sc_hd__ff_100C_1v65.lib:**

!![Screenshot from 2024-11-05 01-31-45](https://github.com/user-attachments/assets/6cd3f908-cb27-46de-97fa-25c3245b015f)

![Screenshot from 2024-11-05 01-31-54](https://github.com/user-attachments/assets/3e6bb830-70bd-427c-a4b9-9c30a0ed764c)

**sky130_fd_sc_hd__ff_100C_1v95.lib:**

![Screenshot from 2024-11-05 01-32-06](https://github.com/user-attachments/assets/f4cee021-8c3c-4778-8967-519e63c435b9)
![Screenshot from 2024-11-05 01-32-11](https://github.com/user-attachments/assets/064440c6-1dab-4117-9a62-ebcb0e10f08e)


**sky130_fd_sc_hd__ff_n40C_1v56.lib:**
![Screenshot from 2024-11-05 01-32-20](https://github.com/user-attachments/assets/cc278ca8-3969-4e7f-a46f-846bd89eb4bd)


![Screenshot from 2024-11-05 01-32-28](https://github.com/user-attachments/assets/5ac5a8bf-9c5c-4282-bc62-e12d3fc5b8d1)

**sky130_fd_sc_hd__ff_n40C_1v65.lib:**

![Screenshot from 2024-11-05 01-32-43](https://github.com/user-attachments/assets/00e02b4c-a11f-4e68-af09-8d2fe9c381c8)

![Screenshot from 2024-11-05 01-32-48](https://github.com/user-attachments/assets/026f0d2f-c3b1-44ea-959c-5b9c150e46d8)

**sky130_fd_sc_hd__ff_n40C_1v76.lib:**

![Screenshot from 2024-11-05 01-32-56](https://github.com/user-attachments/assets/e5577e13-cce2-4ba4-860f-92d04c8e977a)

![Screenshot from 2024-11-05 01-33-01](https://github.com/user-attachments/assets/f138a42f-3cad-451f-a3b1-1d18f9a941ff)

**sky130_fd_sc_hd__ff_n40C_1v95.lib:**

![Screenshot from 2024-11-05 01-33-10](https://github.com/user-attachments/assets/5182cb19-6c23-4fa4-a399-f58bbf689964)

![Screenshot from 2024-11-05 01-33-14](https://github.com/user-attachments/assets/89b0a570-7993-4c3c-9ffd-32680b19ea0b)

**sky130_fd_sc_hd__ff_n40C_1v95_ccsnoise.lib.part1:**

![Screenshot from 2024-11-05 01-33-21](https://github.com/user-attachments/assets/1040ca33-ad49-43df-9485-8b3896d98fcd)

![Screenshot from 2024-11-05 01-33-27](https://github.com/user-attachments/assets/8ecc74ac-5c65-4d49-bff1-5bca4ac7f54b)

**sky130_fd_sc_hd__ff_n40C_1v95_ccsnoise.lib.part2:**

![Screenshot from 2024-11-05 01-33-36](https://github.com/user-attachments/assets/058b067d-c214-4dfa-9c56-a17e322a6d70)

![Screenshot from 2024-11-05 01-33-41](https://github.com/user-attachments/assets/89620caa-fb9c-4223-b92a-0641ae7e389d)


**sky130_fd_sc_hd__ff_n40C_1v95_ccsnoise.lib.part3:**

![Screenshot from 2024-11-05 01-33-50](https://github.com/user-attachments/assets/627a4c1e-8d1d-45ad-8b26-6109be1ba02b)

![Screenshot from 2024-11-05 01-33-55](https://github.com/user-attachments/assets/9ab63ad1-1ad1-4aa0-acc8-5538adbddb8e)


**sky130_fd_sc_hd__ss_100C_1v40.lib:**

![Screenshot from 2024-11-05 01-34-04](https://github.com/user-attachments/assets/864df0a4-0a7d-45e4-a51d-a4f811278e8e)

![Screenshot from 2024-11-05 01-34-09](https://github.com/user-attachments/assets/c0b3f587-84c7-48bd-b67d-bb91ac8790de)

**sky130_fd_sc_hd__ss_100C_1v60.lib:**

![Screenshot from 2024-11-05 01-34-17](https://github.com/user-attachments/assets/bee08190-38b4-44a9-a26e-a58322381945)

![Screenshot from 2024-11-05 01-34-23](https://github.com/user-attachments/assets/029ed47f-e601-4981-ad30-345ae5377650)

**sky130_fd_sc_hd__ss_n40C_1v28.lib:**
![Screenshot from 2024-11-05 01-34-34](https://github.com/user-attachments/assets/018f3c10-924a-4a60-b204-49d9ada7bcec)

![Screenshot from 2024-11-05 01-34-45](https://github.com/user-attachments/assets/f26d09a4-987f-4751-b60d-884a3bb31a50)

**sky130_fd_sc_hd__ss_n40C_1v35.lib:**
![Screenshot from 2024-11-05 01-34-54](https://github.com/user-attachments/assets/5ebc7f04-4582-4404-bf69-d8e6ead24eea)

![Screenshot from 2024-11-05 01-35-03](https://github.com/user-attachments/assets/176853f5-3c94-463b-be37-ceb1a5f7dc2c)

**sky130_fd_sc_hd__ss_n40C_1v40.lib:**

![Screenshot from 2024-11-05 01-35-11](https://github.com/user-attachments/assets/bf6d5bda-2fbc-4a56-8944-0fbaefa59858)

![Screenshot from 2024-11-05 01-35-17](https://github.com/user-attachments/assets/ebf43604-dcc5-4eaf-8532-e3f49fae8005)

**sky130_fd_sc_hd__ss_n40C_1v44.lib:**

![Screenshot from 2024-11-05 01-35-27](https://github.com/user-attachments/assets/b264abcc-0792-4a90-b136-a6f6189a4c4e)

![Screenshot from 2024-11-05 01-35-34](https://github.com/user-attachments/assets/28b102c7-99fc-417d-8f2d-33d37e219f0c)

**sky130_fd_sc_hd__ss_n40C_1v60.lib:**

![Screenshot from 2024-11-05 01-35-47](https://github.com/user-attachments/assets/89f2c308-459e-4375-90e8-0d5ccb45d269)

![Screenshot from 2024-11-05 01-35-54](https://github.com/user-attachments/assets/18e1b3ef-8a6f-49c9-988b-31ef5455501a)

**sky130_fd_sc_hd__ss_n40C_1v60_ccsnoise.lib.part1:**

![Screenshot from 2024-11-05 01-36-02](https://github.com/user-attachments/assets/615ba6c1-4478-4580-974d-19c5ff35ce2e)

![Screenshot from 2024-11-05 01-36-08](https://github.com/user-attachments/assets/8c36dbfd-0903-44d8-898d-a1b1f8fc9b30)


**sky130_fd_sc_hd__ss_n40C_1v60_ccsnoise.lib.part2:**

![Screenshot from 2024-11-05 01-36-16](https://github.com/user-attachments/assets/fedd5de0-e6d4-4d2e-a433-78eeb7f1604e)

![Screenshot from 2024-11-05 01-36-23](https://github.com/user-attachments/assets/d22cd520-5955-4504-8524-91d5352086a6)

**sky130_fd_sc_hd__ss_n40C_1v60_ccsnoise.lib.part3:**

![Screenshot from 2024-11-05 01-36-33](https://github.com/user-attachments/assets/5a026b7f-778e-4ed1-981c-5996f858bf76)

![Screenshot from 2024-11-05 01-36-38](https://github.com/user-attachments/assets/dcf9dcd9-5ba4-48b1-ab30-2ca437622073)

**sky130_fd_sc_hd__ss_n40C_1v76.lib:**

![Screenshot from 2024-11-05 01-36-47](https://github.com/user-attachments/assets/d42f74f7-a0f6-4c76-bfec-3dae623507d0)

![Screenshot from 2024-11-05 01-36-52](https://github.com/user-attachments/assets/722aa579-19a7-4ac7-a9c5-4eb1f00cb827)

**sky130_fd_sc_hd__tt_025C_1v80.lib:**
![Screenshot from 2024-11-05 01-37-01](https://github.com/user-attachments/assets/da174c5e-4fc1-4afd-a199-33fe4ea5daab)

![Screenshot from 2024-11-05 01-37-07](https://github.com/user-attachments/assets/ed3bb282-7172-4ef5-a944-9aae081d8088)

**sky130_fd_sc_hd__tt_100C_1v80.lib:**

![Screenshot from 2024-11-05 01-37-14](https://github.com/user-attachments/assets/fd753578-499d-489a-bb83-0b17eb4e6065)

![Screenshot from 2024-11-05 01-37-19](https://github.com/user-attachments/assets/2a2dd691-3414-4b89-bfab-cc9b5ad084c2)


</details>

</details>



</details>
<summary><strong>Lab Session 13</strong>  Advanced Physical Design using OpenLane using Sky130</summary>

<details>
<summary><strong>Day-1:</strong>  Inception of open-source EDA, OpenLane and Sky130 PDK</summary>

## Inception of open-source EDA, OpenLane and Sky130 PDK
**QFN-48 Package**: The QFN-48 is a compact leadless package with 48 connection pads around its perimeter, providing excellent thermal and electrical performance, ideal for high-density applications.

![Screenshot from 2024-11-11 14-32-57](https://github.com/user-attachments/assets/3f1237c7-c6a6-453f-9655-636f8778eb9c)

**Chip**: An integrated circuit (IC) on a silicon substrate with various functional blocks like memory, processors, and I/O, designed for specific electronics applications.

![Screenshot from 2024-11-11 14-33-11](https://github.com/user-attachments/assets/32a46c31-bde4-46b7-807a-5f09e7a42a82)

**Pads**: Small metallic contact points on a chip or package, used to connect internal circuitry with external connections, enabling signal transfer.

**Core**: The main processing unit of a chip, containing functional logic optimized for power and performance.

**Die**: The section of a silicon wafer containing an individual IC before packaging, hosting all the active circuitry of the chip.

![Screenshot from 2024-11-11 14-33-23](https://github.com/user-attachments/assets/ff797bc6-ea8e-4bd9-be22-e3cf8ce4df95)

**IPs (Intellectual Properties)**: Pre-designed functional modules, such as USB controllers or memory interfaces, licensed for reuse in various designs to save development time and costs.

![Screenshot from 2024-11-11 14-33-32](https://github.com/user-attachments/assets/4e0b10e5-eea2-4b68-a42c-3db071423e38)

---

### Software to Hardware Execution Flow

When running an application on hardware, it first enters the system software layer, which translates it to binary form. Key components here include the Operating System (OS), Compiler, and Assembler.

1. **OS**: Breaks down application functions written in high-level languages (e.g., C, Java) and passes them to a compiler.
2. **Compiler**: Translates functions into hardware-specific low-level instructions.
3. **Assembler**: Converts these instructions into binary code, which the hardware can execute.

![Screenshot from 2024-11-11 14-33-50](https://github.com/user-attachments/assets/0f07a5f9-9ce0-457e-a7af-1caffa64f809)

For instance, take a stopwatch app running on a RISC-V core. The OS generates a function in C, which is sent to the compiler. The compiler produces RISC-V-specific instructions adapted to the architecture. These instructions are then processed by the assembler, converting them to binary code. This binary code is then integrated into the chip layout, allowing the hardware to execute the intended functionality.

![Screenshot from 2024-11-11 14-34-04](https://github.com/user-attachments/assets/1d9b1e0e-1170-4709-98b9-3f2f71f28098)

For the above stopwatch the below figure shows the input and output of the compiler and assembler.

![Screenshot from 2024-11-11 14-34-16](https://github.com/user-attachments/assets/205f26fb-62b5-40d4-8bf2-ef96fc9ff711)

The compiler produces instructions specific to the architecture, and the assembler converts these into corresponding binary patterns. To execute them on hardware, an RTL (written in a Hardware Description Language) interprets and implements the instructions. This RTL design is then synthesized into a netlist, represented by interconnected logic gates. Finally, the netlist undergoes physical design implementation, preparing it for chip fabrication.

![Screenshot from 2024-11-11 14-34-28](https://github.com/user-attachments/assets/f4df4c33-62e9-4a78-9f14-98282aa9c2a2)

---

### Components of ASIC Design

- **RTL IPs**: Verified circuit blocks (adders, flip-flops) in HDL, used to accelerate complex designs.
- **EDA Tools**: Automates ASIC tasks like synthesis, optimization, and timing analysis, ensuring performance meets requirements.
- **PDK Data**: Semiconductor foundry files defining the manufacturing process, ensuring ASIC designs are fabrication-ready.

![Screenshot from 2024-11-11 14-34-40](https://github.com/user-attachments/assets/610fc21f-8262-4d4b-b868-01a9d84b40ca)

---

### Simplified RTL to GDSII Flow

![Screenshot from 2024-11-11 14-34-49](https://github.com/user-attachments/assets/9d8d2664-476d-4cd6-b5b3-7e0aed241a2f)

1. **RTL Design**: Describes circuit function using HDLs like Verilog or VHDL.
2. **RTL Synthesis**: Converts RTL to a gate-level netlist with optimized cells.
3. **Floor and Power Planning**: Lays out major components, power grid, and I/O.
4. **Placement**: Allocates cells to minimize wirelength and signal delay.
5. **Clock Tree Synthesis (CTS)**: Distributes clock signals uniformly to reduce skew.
6. **Routing**: Connects components while meeting design rules.
7. **Sign-off**: Final verification, confirming design readiness for fabrication.
8. **GDSII Generation**: Creates the layout for chip production.

---

### OpenLane ASIC Flow Overview

![Screenshot from 2024-11-11 14-34-58](https://github.com/user-attachments/assets/414fbce3-7665-4375-9123-42a11f41ccec)

1. **RTL Synthesis and Technology Mapping**: Uses Yosys and ABC.
2. **Static Timing Analysis**: Uses OpenSTA.
3. **Floor Planning**: Utilizes init_fp, ioPlacer, pdn, tapcell.
4. **Placement**: Managed by RePLace, Resizer, OpenPhySyn, and OpenDP.
5. **Clock Tree Synthesis**: Uses TritonCTS.
6. **Fill Insertion**: Uses OpenDP.
7. **Routing**: Uses FastRoute/CU-GR (global) and TritonRoute/DR-CU (detailed).
8. **SPEF Extraction**: OpenRCX for parasitic data.
9. **GDSII Output**: Uses Magic and KLayout.
10. **Design Rule Checks**: Magic and KLayout.
11. **Layout vs. Schematic Check**: Uses Netgen.
12. **Antenna Checks**: Handled by Magic.

---

### OpenLane Directory Structure

```plaintext
├── OpenLane            # Tool directory
│   ├── designs         # Holds all designs
│   │   └── picorv32a   # Example design
├── pdks                # PDK-related files
│   ├── skywater-pdk    # Skywater 130nm PDKs
│   ├── open-pdks       # Scripts for open-source tool compatibility
│   ├── sky130A         # Open-source compatible PDK variant
│   │   ├── libs.ref    # Node-specific files (e.g., timing, tech LEF)
│   │   ├── libs.tech   # Tool-specific files (e.g., for KLayout)
```

### Running Synthesis in OpenLane

1. **Navigate and start OpenLane**:
   ```bash
   cd Desktop/work/tools/openlane_working_dir/openlane
   docker
   ./flow.tcl -interactive
   package require openlane 0.9
   prep -design picorv32a
   run_synthesis
   ```

   ![image](https://github.com/user-attachments/assets/dec2ca6b-a438-4cb2-8a0b-47406ec6c1fa)


3. **View the Netlist**:
   ```bash
   cd designs/picorv32a/runs/11-11_16-15/results/synthesis/
   gedit picorv32a.synthesis.v
   ```

   ![image](https://github.com/user-attachments/assets/30826177-af4f-4f48-b012-b9600ccbb8d4)
   ![image](https://github.com/user-attachments/assets/b5753837-5783-4ed3-9a03-53a99fa6fb1c)
   
5. **Yosys Report**:
   ```bash
   cd ../..
   cd reports/synthesis
   gedit 1-yosys_4.stat.rpt
   ```

  ![image](https://github.com/user-attachments/assets/305e3e81-4a07-4875-968c-372adfc16186)

 ![image](https://github.com/user-attachments/assets/f785e48d-1ca3-4629-ae30-432c6eda9cf9)


  

**Report:**

```
28. Printing statistics.

=== picorv32a ===

   Number of wires:              14596
   Number of wire bits:          14978
   Number of public wires:        1565
   Number of public wire bits:    1947
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:              14876
     sky130_fd_sc_hd__a2111o_2       1
     sky130_fd_sc_hd__a211o_2       35
     sky130_fd_sc_hd__a211oi_2      60
     sky130_fd_sc_hd__a21bo_2      149
     sky130_fd_sc_hd__a21boi_2       8
     sky130_fd_sc_hd__a21o_2        57
     sky130_fd_sc_hd__a21oi_2      244
     sky130_fd_sc_hd__a221o_2       86
     sky130_fd_sc_hd__a22o_2      1013
     sky130_fd_sc_hd__a2bb2o_2    1748
     sky130_fd_sc_hd__a2bb2oi_2     81
     sky130_fd_sc_hd__a311o_2        2
     sky130_fd_sc_hd__a31o_2        49
     sky130_fd_sc_hd__a31oi_2        7
     sky130_fd_sc_hd__a32o_2        46
     sky130_fd_sc_hd__a41o_2         1
     sky130_fd_sc_hd__and2_2       157
     sky130_fd_sc_hd__and3_2        58
     sky130_fd_sc_hd__and4_2       345
     sky130_fd_sc_hd__and4b_2        1
     sky130_fd_sc_hd__buf_1       1656
     sky130_fd_sc_hd__buf_2          8
     sky130_fd_sc_hd__conb_1        42
     sky130_fd_sc_hd__dfxtp_2     1613
     sky130_fd_sc_hd__inv_2       1615
     sky130_fd_sc_hd__mux2_1      1224
     sky130_fd_sc_hd__mux2_2         2
     sky130_fd_sc_hd__mux4_1       221
     sky130_fd_sc_hd__nand2_2       78
     sky130_fd_sc_hd__nor2_2       524
     sky130_fd_sc_hd__nor2b_2        1
     sky130_fd_sc_hd__nor3_2        42
     sky130_fd_sc_hd__nor4_2         1
     sky130_fd_sc_hd__o2111a_2       2
     sky130_fd_sc_hd__o211a_2       69
     sky130_fd_sc_hd__o211ai_2       6
     sky130_fd_sc_hd__o21a_2        54
     sky130_fd_sc_hd__o21ai_2      141
     sky130_fd_sc_hd__o21ba_2      209
     sky130_fd_sc_hd__o21bai_2       1
     sky130_fd_sc_hd__o221a_2      204
     sky130_fd_sc_hd__o221ai_2       7
     sky130_fd_sc_hd__o22a_2      1312
     sky130_fd_sc_hd__o22ai_2       59
     sky130_fd_sc_hd__o2bb2a_2     119
     sky130_fd_sc_hd__o2bb2ai_2     92
     sky130_fd_sc_hd__o311a_2        8
     sky130_fd_sc_hd__o31a_2        19
     sky130_fd_sc_hd__o31ai_2        1
     sky130_fd_sc_hd__o32a_2       109
     sky130_fd_sc_hd__o41a_2         2
     sky130_fd_sc_hd__or2_2       1088
     sky130_fd_sc_hd__or2b_2        25
     sky130_fd_sc_hd__or3_2         68
     sky130_fd_sc_hd__or3b_2         5
     sky130_fd_sc_hd__or4_2         93
     sky130_fd_sc_hd__or4b_2         6
     sky130_fd_sc_hd__or4bb_2        2

   Chip area for module '\picorv32a': 147712.918400

```

**Report Summary**:

```
Flop ratio = Number of D Flip flops = 1613  = 0.1084
             ______________________   _____
             Total Number of cells    14876
```

- **Wire Count**: 14,596
- **Cell Count**: 14,876, including specific cells like `sky130_fd_sc_hd__a2111o_2`, `sky130_fd_sc_hd__and2_2`.
- **D Flip-flops**: 1,613 with a flop ratio of 0.1084

</details>

