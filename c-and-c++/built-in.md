# 0

## sizeof()

```c
/* c语言中使用sizeof()输出各种数据类型的大小_mb5fd86ddc9c8d5的技术博客_51CTO博客 */
/* https://blog.51cto.com/u_15057858/3346950 */

#include <stdio.h>

int main(void)
{
    printf("short: %zd.\n", sizeof(short));
    printf("int: %zd.\n", sizeof(int));
    printf("long: %zd.\n", sizeof(long));
    printf("long long: %zd.\n\n", sizeof(long long));
    
    printf("char: %zd.\n\n", sizeof(char));
    
    printf("float: %zd.\n", sizeof(float));
    printf("double: %zd.\n", sizeof(double));
    printf("long double: %zd.\n", sizeof(long double));
    
    return 0;    
}

```



```c
/* C 语言使用 sizeof() 运算符打印变量大小 – 91 技术博客 */
/* https://www.91tech.org/archives/2843 */
/*C program to print size of variables using sizeof() operator.*/
 
#include <stdio.h>
 
int main()
{
     
    char    a       ='A';           
    int     b       =120;
    float   c       =123.0f;
    double  d       =1222.90;
    char    str[]   ="Hello";
 
    printf("\nSize of a: %lu.",sizeof(a));
    printf("\nSize of b: %lu.",sizeof(b));
    printf("\nSize of c: %lu.",sizeof(c));
    printf("\nSize of d: %lu.",sizeof(d));
    printf("\nSize of str: %lu",sizeof(str));
 
    return 0;
}

```

