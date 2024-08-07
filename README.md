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
   When you type the above command, a list of opcode appears in the terminal. to obtain the main section of the program we need to type /main as shown below.
   ```
   /main
   ```
   ![WhatsApp Image 2024-08-07 at 6 57 48 PM (1)](https://github.com/user-attachments/assets/6a1ab32e-3991-4cb1-88b3-bc239ef0eb81)
   ![WhatsApp Image 2024-08-07 at 6 57 48 PM (2)](https://github.com/user-attachments/assets/3df29bd7-424a-480b-a70c-68273485446f)

6. **Observation-2 :**

   We can see there are 11 lines of opcode in the main section.

7. **Conclusion :**
   
   The compilation in the Ofast procedure is more optimised.

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

   Here, 0x100b0 is the address of the start of main function.
   Since register a2 is present at 0x100b0, we'll check register a0 before and after the execution of instructions.
   
4. **Execution of the following commands to check he contents of register a0 before and after running the instructions.**
   ```
   reg 0 a0
   ```
   
5. **Move PC to location 100b8 using the following command.**
   ```
   until pc 0 100b8
   ```
   
6. **Checking the contents of sp.**
   ```
   reg 0 sp
   ```
