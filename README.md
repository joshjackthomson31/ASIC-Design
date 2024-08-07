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
