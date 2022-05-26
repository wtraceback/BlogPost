给定一个长度不超过 10^4 的、仅由英文字母构成的字符串。请将字符重新调整顺序，按 StringString.... （注意区分大小写）这样的顺序输出，并忽略其它字符。当然，六种字符的个数不一定是一样多的，若某种字符已经输出完，则余下的字符仍按 String 的顺序打印，直到所有字符都被输出。例如 gnirtSSs 要调整成 StringS 输出，其中 s 是多余字符被忽略。

## 输入格式：
输入在一行中给出一个长度不超过 10^4 的、仅由英文字母构成的非空字符串。

## 输出格式：
在一行中按题目要求输出排序后的字符串。题目保证输出非空。

## 输入样例：
> sTRidlinSayBingStrropriiSHSiRiagIgtSSr

## 输出样例：
> StringStringSrigSriSiSii

![华丽的分割线](./【PAT乙级】1108-String复读机-(20-分)-C语言实现.assets/17731575-4c6c40de3ead009c.jpg)


## 代码实现：
```
#include <stdio.h>
#include <string.h>

int main() {
    char arr[10005];
    if (scanf("%s", arr) == 1) {
        int target[6] = {0};
        char targetStr[7] = "String";
        int len = strlen(arr);

        // 获取输入的字符中 String 这几个字符的个数
        for (int i = 0; i < len; i++) {
            switch(arr[i]) {
                case 'S':
                    target[0] += 1;
                    break;
                case 't':
                    target[1] += 1;
                    break;
                case 'r':
                    target[2] += 1;
                    break;
                case 'i':
                    target[3] += 1;
                    break;
                case 'n':
                    target[4] += 1;
                    break;
                case 'g':
                    target[5] += 1;
                    break;
                default:
                    ;
            }
        }

        // 获取 target 数组中字符数最多的个数
        // 这边的 target 定义的时候，只能是 int 类型，不能写成 char 类型
        int n = target[0];
        for (int i = 0; i < 6; i++) {
            if (n < target[i]) {
                n = target[i];
            }
        }

        // 打印输出
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < 6; j++) {
                if (target[j] != 0) {
                    // 不断的递减目标数组中字符个数的数目，直到字符数目为 0
                    target[j] -= 1;
                    printf("%c", targetStr[j]);
                }
            }
        }
        printf("\n");
    }

    return 0;
}
```
