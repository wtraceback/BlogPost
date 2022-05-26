给定区间 [−2^31, 2^31] 内的 3 个整数 A、B 和 C，请判断 A+B 是否大于 C。

## 输入格式：
输入第 1 行给出正整数 T (≤10)，是测试用例的个数。随后给出 T 组测试用例，每组占一行，顺序给出 A、B 和 C。整数间以空格分隔。

## 输出格式：
对每组测试用例，在一行中输出 Case #X: true 如果 A+B>C，否则输出 Case #X: false，其中 X 是测试用例的编号（从 1 开始）。

## 输入样例：
>4
1 2 3
2 3 4
2147483647 0 2147483646
0 -2147483648 -2147483647

## 输出样例：
>Case #1: false
Case #2: true
Case #3: true
Case #4: false


![华丽的分割线](./【PAT乙级】1011-A+B-和-C-(15-分)-C语言实现.assets/17731575-4d594a48e79489a6.jpg)


## 代码实现：
```
#include <stdio.h>

int main() {
    int n;
    if (scanf("%d", &n)) {
        // 接收输入
        long long int a, b, c;
        int arr[10] = {0};
        for (int i = 0; i < n; i++) {
            if (scanf("%lld %lld %lld", &a, &b, &c) == 3) {
                if (a + b > c) {
                    arr[i] = 1;
                } else {
                    arr[i] = 0;
                }
            }
        }

        // 打印输出
        for (int j = 0; j < n; j++)  {
            if (arr[j] == 1) {
                printf("Case #%d: true\n", j + 1);
            } else {
                printf("Case #%d: false\n", j + 1);
            }
        }
    }

    return 0;
}

// 解法2：
// 定义 a、b、c 三个 long long 的数组，用于接收数据，最后再进行比较输出，缺点是空间消耗更大
```
