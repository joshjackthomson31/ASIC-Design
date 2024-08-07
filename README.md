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
       int n = 10;
       int sum = 0;
       for( int i = 1; i <= n; i++)
       {
         sum += i;
       }
       printf("The sum from 1 to %d is %d\n", n, sum);
       return 0;
   }
   ```
   ![WhatsApp Image 2024-08-07 at 3 21 34 PM](https://github.com/user-attachments/assets/929bf6fd-8063-4c88-b2f3-8dd9b71323dd)

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
   The sum from 1 to 10 is 55
   ```
   ![WhatsApp Image 2024-08-07 at 3 21 34 PM (2)](https://github.com/user-attachments/assets/e4d271cf-b84a-4e35-8cd7-f49e9b09db42)

6. **Conclusion :**

   In the above snap, it can be seen that the C program is complied successfully and the output is also correctly calculated i.e., 55 ( 1+2+3+4+5+6+7+8+9+10 = 55).

#### Task-2 : Compile the C program code using RISC-V GNU compiler.
1. **Compile the code :**
   ```
   riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum.o sum.c
   ```

2. **Run the code :**
   ```
   riscv64-unknown-elf-objdump -d sum.o | less
   ```

3. **Output :**
