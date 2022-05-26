输入两个非负 10 进制整数 A 和 B (≤2^30 − 1)，输出 A+B 的 D (1<D≤10)进制数。

## 输入格式：
输入在一行中依次给出 3 个整数 A、B 和 D。

## 输出格式：
输出 A+B 的 D 进制数。

## 输入样例：
>123 456 8

## 输出样例：
>1103


![华丽的分割线](./【PAT乙级】1022-D进制的A+B-(20-分)-C语言实现.assets/17731575-c8e7ea59100ad676.jpg)



## 代码实现：
```
#include <stdio.h>

int main() {
    int a, b, d;
    if (scanf("%d %d %d", &a, &b, &d) == 3) {
        int sum = a + b;
        int arr[32] = {0};    // int 类型有 32 个二进制位

        // 将数值转换为指定的进制
        // 使用 do while 是为了处理 sum=0 的情况
        int n = 0;
        do {
            arr[n] = sum % d;
            sum = sum / d;
            n++;
        } while (sum != 0);

        // 打印输出数组中的进制数
        for (int i = n - 1; i >= 0; i--) {
            printf("%d", arr[i]);
        }
        printf("\n");
    }

    return 0;
}
```
