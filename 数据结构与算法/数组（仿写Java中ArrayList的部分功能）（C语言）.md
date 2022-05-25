```
#include <stdio.h>
#include <stdlib.h>     // 包含了 malloc 和 exit 函数
#include <stdbool.h>    // 包含 bool 类型

struct Array {          // 定义了一个数据类型，该数据类型的名字是 struct Array，该数据类型有三个成员
    int *base;          // 存储的是数组第一个元素的地址
    int len;            // 数组所能容纳的最大元素的个数
    int cnt;            // 当前数组有效元素的个数
};

// 函数声明
void initArray(struct Array *p, int length);
bool appendArray(struct Array *p, int val);
bool insertArray(struct Array *p, int pos, int val);
bool deleteArray(struct Array *p, int pos, int *delVal);
bool isEmpty(struct Array *p);
bool isFull(struct Array *p);
void sortArray(struct Array *p);
void showArray(struct Array *p);
void inversionArray(struct Array *p);

int main() {
    struct Array arr;
    int val;

    initArray(&arr, 10);
    showArray(&arr);
    appendArray(&arr, 10);
    appendArray(&arr, 1);
    appendArray(&arr, 11);
    appendArray(&arr, 0);
    appendArray(&arr, 7);
    showArray(&arr);

    /*
    if (insertArray(&arr, 4, 0)) {
        printf("数据插入成功\n");
    } else {
        printf("数据插入失败\n");
    }
    showArray(&arr);
    */

    /*
    if ( deleteArray(&arr, 5, &val) ) {
        printf("删除成功\n");
        printf("您删除的元素时： %d\n", val);
    } else {
        printf("删除失败\n");
    }
    showArray(&arr);
    */

    printf("排序后的数据\n");
    sortArray(&arr);
    showArray(&arr);

    printf("反转后的数据\n");
    inversionArray(&arr);
    showArray(&arr);

    return 0;
}

// 数组的初始化
void initArray(struct Array *p, int length) {
    p->base = (int *)malloc(sizeof(int) * length);

    if (NULL == p->base) {
        printf("动态内存分配失败\n");
        exit(-1);    // 终止整个程序
    } else {
        p->len = length;
        p->cnt = 0;
    }

    return;
}

// 往数组中添加一个元素
bool appendArray(struct Array *p, int val) {
    if ( isFull(p) ) {
        printf("数组当前的状态是填满状态，不能再添加\n");
        return false;
    } else {
        p->base[p->cnt] = val;
        p->cnt = p->cnt + 1;
        return true;
    }
}

// 往数组中插入一个元素
bool insertArray(struct Array *p, int pos, int val) {
    if ( isFull(p) ) {
        return false;
    }

    if (pos <= 0 || pos > (p->cnt + 1)) { // pos 应该小于或等于 cnt + 1
        return false;
    }

    for (int i = (p->cnt - 1); i >= pos - 1; i--) {
        p->base[i + 1] = p->base[i];
    }
    p->base[pos - 1] = val;
    p->cnt = p->cnt + 1;

    return true;
}

// 根据指定的位置，删除数组中的某个元素
bool deleteArray(struct Array *p, int pos, int *delVal) {
    if ( isEmpty(p) ) {
        return false;
    }

    if (pos <= 0 || pos > p->cnt ) {
        return false;
    }

    *delVal = p->base[pos - 1];
    for (int i = pos; i < p->cnt; i++) {
        p->base[i - 1] = p->base[i];
    }
    p->cnt = p->cnt - 1;

    return true;
}

// 判断数组是否为空
bool isEmpty(struct Array *p) {
    if (0 == p->cnt) {
        return true;
    } else {
        return false;
    }
}

// 判断数组是否已填满
bool isFull(struct Array *p) {
    if (p->cnt == p->len) {
        return true;
    } else {
        return false;
    }
}

// 对数组进行排序：冒泡排序
void sortArray(struct Array *p) {
    int n = p->cnt;
    int temp;

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n - 1 - i; j++) {
            if (p->base[j] > p->base[j + 1]) {
                temp = p->base[j + 1];
                p->base[j + 1] = p->base[j];
                p->base[j] = temp;
            }
        }
    }

    return;
}

// 展示数组中的数据
void showArray(struct Array *p) {
    if ( isEmpty(p) ) {
        printf("数组为空！\n");
    } else {
        for (int i = 0; i < p->cnt; i++) {
            printf("%d  ", p->base[i]);
        }

        printf("\n");
    }
}

// 对数组进行反转
void inversionArray(struct Array *p) {
    if ( isEmpty(p) == false ) {
        int i = 0;
        int j = p->cnt - 1;
        int temp;

        while (i < j) {
            temp = p->base[i];
            p->base[i] = p->base[j];
            p->base[j] = temp;
            i++;
            j--;
        }
    }

    return;
}
```
