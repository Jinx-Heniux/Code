# 线性表



```c
# 2022考研计算机王道数据结构：第二章线性表02 顺序表的定义
# https://youtu.be/9PJWoU06GsE


# 顺序表的实现--静态分配
# 问题：InitList(SqList L) or InitList(SqList &L) or InitList(SqList *L)
# 解答：InitList(SqList *L) 需要接收一个指针，指向SqList类型的数据；
#      调用时，传递该SqList类型数据的地址，InitList(&L);

#include <stdio.h>
 
#define MaxSize 10

typedef struct {
    int data[MaxSize];
    int length;
} SqList;

void InitList(SqList *L) {
    for(int i=0; i<MaxSize; i++) {
        L->data[i] = 0;
    }
    L->length = 0;
}

int main() {
    SqList L;
    InitList(&L);
    
    for(int i=0; i<MaxSize; i++)
        printf("data[%d]=%d\n", i, L.data[i]);
    
    return 0;
}

```



```c
# 顺序表的实现--动态分配



#include <stdio.h>
#include <stdlib.h>

#define InitSize 10

typedef struct {
    // ElemType *data;
    // ElemType = int
    int *data;
    int MaxSize;
    int length;
} SeqList;

void InitList(SeqList *L) {
    //用 malloc 函数申请一片连续的存储空间
    L->data = (int *)malloc(sizeof(int)*InitSize);
    L->length = 0;
    L->MaxSize = InitSize;
}

int main() {
    SeqList L;
    InitList(&L);
    
    for(int i=0; i<L.MaxSize; i++)
        printf("data[%d]=%d\n", i, L.data[i]);
    
    return 0;
}
```

