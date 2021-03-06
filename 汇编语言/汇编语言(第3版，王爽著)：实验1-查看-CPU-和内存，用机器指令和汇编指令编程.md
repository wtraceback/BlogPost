## （一） 预备知识：Debug 的使用
### 1. 什么是 Debug？
Debug 是 DOS、Windows 都提供的实模式（8086 方式）程序的调试工具。使用它，可以查看 CPU 各种寄存器中的内容、内存的情况和在机器码级跟踪程序的运行。

### 2. 我们用到的 Debug 功能
- 用 Debug 的 R 命令查看、改变CPU寄存器的内容
- 用 Debug 的 D 命令查看内存中的内容
- 用 Debug 的 E 命令改写内存中的内容
- 用 Debug 的 U 命令将内存中的机器指令翻译成汇编指令
- 用 Debug 的 T 命令执行一条机器指令
- 用 Debug 的 A 命令以汇编指令的格式在内存中写入一条机器指令
- 用 Debug 的 Q 命令可以退出 Debug

>关于在 Windows 中使用 Debug，请看我的上一篇文章:
> [64 位 Windows 下配置汇编用的 Debug 环境（win7和win10都适用](https://www.jianshu.com/p/20b28e6d0e7a)

### 3. 命令的使用
#### （1） R 命令
> 功能：
> 使用  R 命令查看、改变CPU寄存器的内容
- 进入 Debug
![进入 Debug](./汇编语言(第3版，王爽著)：实验1-查看-CPU-和内存，用机器指令和汇编指令编程.assets/17731575-a450d8efe9c9e038.png)

- 使用 R 命令查看 CPU 中各个寄存器中的内容
![使用 R 命令查看 CPU 中各个寄存器中的内容](./汇编语言(第3版，王爽著)：实验1-查看-CPU-和内存，用机器指令和汇编指令编程.assets/17731575-0b4e55d0e31ff557.png)


- 使用 R 命令修改寄存器的内容
> 用法：
> **r 寄存器名称**

![使用 R 命令修改寄存器的内容](./汇编语言(第3版，王爽著)：实验1-查看-CPU-和内存，用机器指令和汇编指令编程.assets/17731575-9b414d31ba83223f.png)


#### （2） D 命令
> 功能：
> 使用 D 命令查看内存中的内容


> 用法：（默认查看 128 个内存单元的内容）
> **d 段地址:偏移地址**
> 或指定查看范围：
> **d 段地址:起始偏移地址 结尾偏移地址**

![使用 D 命令查看内存 073f:0100 处的内容](./汇编语言(第3版，王爽著)：实验1-查看-CPU-和内存，用机器指令和汇编指令编程.assets/17731575-185b05929862ff12.png)

#### （3） E 命令
> 功能：
> 使用 E 命令改写内存中的内容（也可视为写入机器指令）

> 用法一：
> **e 段地址:起始偏移地址 数据 数据 数据 ······**
> 用法二：（以提问的方式来逐个地修改从某一地址开始的内存单元中的内容，以从 073f:10 单元开始为例）
> **1. 输入 e 073f:10，按 Enter 键**
> **2. 在光标的 . 后面输入想要写入的数据（输入数据然后按空格；单单按空格则视为不修改，跳到下一单元）**
> **3. 希望修改的内存单元改写完毕后，按 Enter 键，E 命令操作结束**

![使用 E 命令改写内存中的内容](./汇编语言(第3版，王爽著)：实验1-查看-CPU-和内存，用机器指令和汇编指令编程.assets/17731575-2e7f7d24cfe84157.png)

#### （4） U 命令
> 功能：
> 使用 U 命令查看内存中机器码的含义（将内存中的机器指令翻译成汇编指令）

> 用法：
> **u 段地址:偏移地址**

- 先用 E 命令向内存中写入机器指令，再用 U 命令将内存单元中的内容翻译为汇编指令
> 机器指令&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对应的汇编指令
> b80100&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;mov ax, 0001
> b90200&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;mov cx, 0002
> 01c8&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;add ax, cx

![用 U 命令将内存单元中的内容翻译为汇编指令](./汇编语言(第3版，王爽著)：实验1-查看-CPU-和内存，用机器指令和汇编指令编程.assets/17731575-31f6462a566bd63e.png)

#### （5） T 命令
> 功能：
> 使用 T 命令执行 CS:IP 指向的机器指令

![使用 T 命令执行 CS:IP 指向的指令](./汇编语言(第3版，王爽著)：实验1-查看-CPU-和内存，用机器指令和汇编指令编程.assets/17731575-f6303f185c809f2c.png)

#### （6） A 命令
> 功能：
> 使用 A 命令以汇编指令的格式在内存中写入一条机器指令

前面使用了 E 命令写入机器指令，这样写起来很不方便，也不直观，最好能直接以汇编指令的形式写入指令。
![使用 A 命令从 073f:0f 开始的内存单元中写入指令](./汇编语言(第3版，王爽著)：实验1-查看-CPU-和内存，用机器指令和汇编指令编程.assets/17731575-c79a0c23412ea484.png)


## （二） 实验任务
### 1. 将下面三条指令写入从 2000：0 开始的内存单元中，利用这三条指令计算 2 的 3 次方
```
mov ax, 1
add ax, ax
jmp 2000:0003
```
![实验1：利用三条指令计算](./汇编语言(第3版，王爽著)：实验1-查看-CPU-和内存，用机器指令和汇编指令编程.assets/17731575-dce625a8bb8d8e0e.png)

### 2. 查看主板内存中的内容
PC 机主板上的 ROM 中写有一个生产日期，在内存 FFF00H~FFFFFH 的某几个单元中，请找出这个日期并试图改变它。
![查看主板内存中的内容](./汇编语言(第3版，王爽著)：实验1-查看-CPU-和内存，用机器指令和汇编指令编程.assets/17731575-b1935e2c472ae5e5.png)

PC 机主板上的 ROM 写的生产日期是：01/01/92
由于内存 FFF00H~FFFFFH 属于 ROM，只能读，不能写，所以写入数据的操作是没有结果的

### 3. 向内存 B8100H 开始的单元中填写数据
```
// 如：
e b810:0000 01 01 02 02 03 03 04 04
```
请先填写不同的数据，观察产生的现象；再改变填写的地址，观察产生的现象。

![观察到的现象](./汇编语言(第3版，王爽著)：实验1-查看-CPU-和内存，用机器指令和汇编指令编程.assets/17731575-e2c54fec183f0dfa.png)

可以看到屏幕出现了些彩色的图案，这些是写入的数据对应的 ASCII 码字符；
而 b810:0 所对应的内存地址是显存地址空间（如下图所示），所以往显存中写入信息，显卡会将它输出到显示器上。

![8086PC 机内存地址空间分配](./汇编语言(第3版，王爽著)：实验1-查看-CPU-和内存，用机器指令和汇编指令编程.assets/17731575-ebbfbb5b5ea0f1ed.png)
