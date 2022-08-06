## （一） make + makefile
1. make + makefile 相当于类 Unix 系统下的项目管理工具
2. make 是一个命令工具，是一个用来解释 makefile 文件中包含的指令的命令工具
3. makefile 是一个文件，里面包含了一整个项目的编译和处理规则；里面定义了一系列的规则来指定，哪些文件需要先编译，哪些文件需要后编译，哪些文件需要重新编译，甚至于进行更复杂的功能操作
4. 也就是说项目编译的规则在 makefile 文件中，然后使用 make 命令去读取、解释、执行 makefile 文件中的命令
![](Makefile%20入门.assets/2022-08-06-09-43-03.png)

## （二） 为什么要用 make + makefile
使用 gcc 编译器来编译 C 程序的应该都知道，如果我们只编译一个 .c 文件还比较简单， 但是当我们要编译多个文件，或者不同文件夹中的文件，而且这些文件都是相关联的，用 gcc 命令逐个去编译的话，就会很混乱而且工作量庞大。

当一个工程中的源文件不计其数，其按类型、功能、模块分别放在若干个目录中，哪些文件需要先编译，哪些文件需要后编译，哪些文件需要重新编译，每次都手动来操作的话，想想都头大，这时候我们就应该借助构建工具来简化我们的编译操作，也就是 make + makefile

## （三） makefile 文件中的命令书写规范
因为项目的所有编译规则全部都规范到了 makefile 文件中，所以现在我们来了解一下 makefile 里面编译和处理规则的编写规范

### 1. 一般情况下使用 gcc 编译单个 C 语言源文件
```
gcc -o main main.c
```

### 2. 使用 makefile 来管理
1. 创建一个 makefile 文件
2. 在 makefile 文件中编写相关的命令
```
main: main.c
    gcc -o main main.c
```
3. 在终端中直接输入 **make** 然后回车，**make** 会自动寻找当前目录下的 makefile 文件，然后读取文件中的内容并解释执行


### 3. makefile 文件中命令的规范
![](Makefile%20入门.assets/2022-08-06-10-21-32.png)


### 4. 编译多个源文件的情况
#### （1） 多个源文件
tool.h
```
int find_max(int arr[], int length);
```

tool.c
```
#include "tool.h"

int find_max(int arr[], int length) {
    int r = arr[0];
    int i;

    for (i = 0; i < length; i++) {
        if (arr[i] > r) {
            r = arr[i];
        }
    }

    return r;
}
```

main.c
```
#include <stdio.h>
#include "tool.h"

int main() {
    int arr[] = {1, 8, 3, 4, 5};
    int r = find_max(arr, 5);
    printf("max = %d\n", r);

    return 0;
}
```

#### （2） 直接使用 gcc 编译
```
gcc  main.c tool.c -o main
```

#### （3） 使用 makefile 文件来管理
1. 方式一（不推荐）
```
main: main.c tool.c
    gcc  main.c tool.c -o main
```

2. 方式二（推荐）
```
main: main.c tool.o
    gcc main.c tool.o -o main

tool.o: tool.c
    gcc -c tool.c
```

3. 为什么推荐 **方式二** 呢？
因为不用每次都重新编译生成 tool.o ，这样可以加快编译链接的速度。
如果 tool.o 已存在，重新执行 **make** 命令的时候，会先判断 tool.c 是否有改动，如果有改动，则重新生成 tool.o ，如果没有改动，则直接使用已存在的 tool.o 文件，省掉了重新编译生成 tool.o 的环节。


### 5. 添加删除快捷指令
在源代码发布前，只保留 .c 和 .h 文件，删除其它编译生成的相关文件
```
main: main.c tool.o
    gcc main.c tool.o -o main

tool.o: tool.c
    gcc -c tool.c

clean:
    rm *.o main
```
在终端输入 **make** 相当于编译整个项目，然后生成对应的目标文件
在终端输入 **make clean** ，会执行 **rm \*.o main** 命令，删除当前目录下所有 .o 文件和 main 文件

### 6. makefile 文件中设置变量
1. 未添加变量的情况下：添加编译参数 -std=c99
```
main: main.c tool.o
    gcc -std=c99 main.c tool.o -o main

tool.o: tool.c
    gcc -std=c99 -c tool.c

clean:
    rm *.o main
```

2. 添加变量的情况下：添加编译参数 -std=c99
```
CFLAGS = -std=99

main: main.c tool.o
    gcc $(CFLAGS) main.c tool.o -o main

tool.o: tool.c
    gcc $(CFLAGS) -c tool.c

clean:
    rm *.o main
```


### 7. 一个 makefile 文件，用来生成多个可执行文件
```
CFLAGS = -std=99

all: main0 main1

main0: main0.c tool.o
    gcc $(CFLAGS) main0.c tool.o -o main0

main1: main1.c tool.o
    gcc $(CFLAGS) main1.c tool.o -o main1

tool.o: tool.c
    gcc $(CFLAGS) -c tool.c

clean:
    rm *.o main0 main1
```
- **make** 命令执行的时候，只会找到第一个要生成的目标，也就是 all，会运行一下 all，然后发现它有两个依赖，就是 main0 和 main1 ，接着找到 main0 和 main1 ，编译它们由此生成了两个执行文件
- clean 没有被依赖到，所以不会执行，如果 clean 命令要被执行的话，需要我们手动执行，就是在终端输入 **make clean**
- 由此可推理出：**make** 就相当于执行了第一个要生成的目标文件，即 **make all** ，只要我们知道第一个所要生成的目标文件名，就可以通过 **“make 目标文件名”** 来生成


## （四） 参考
[Makefile 的写法](https://www.bilibili.com/video/BV1Mx411m7fm?spm_id_from=333.999.0.0&vd_source=4859610c015d401745b90b6ed755b9d6)
[跟我一起写Makefile (PDF重制版)](https://github.com/seisman/how-to-write-makefile)