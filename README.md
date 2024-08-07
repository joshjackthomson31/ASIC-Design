# ASIC-Design

## LAB SESSION-1

### AIM
To verify and compile a C program to find sum from 1 to n using GCC and RISC-V GNU compiler.

### PROCEDURE
#### Task-1 : Compile the C program code using GCC compiler
```markdown
#include <stdio.h>
int main() 
{
    int n = 5;
    int sum = 0;
    for( int i = 1; i <= n; i++)
    {
      sum += i;
    }
    printf("The sum from 1 to %d is %d\n", n, sum);
    return 0;
}
```
