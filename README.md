# ASIC-Design

## LAB SESSION-1

### AIM
To verify and compile a C program to find sum from 1 to n using GCC and RISC-V GNU compiler.

### PROCEDURE
#### Task-1 : Compile the C program code using GCC compiler.
1. **Code :**
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

2. **Compile the code :**
   ```
   gcc sum.c
   ```
   
3. **Run the code :**
   ```
   ./a.out
   ```
   
4. **Output :**
   ```
   The sum from 1 to 10 is 55
   ```

5. **Conclusion :** 
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
