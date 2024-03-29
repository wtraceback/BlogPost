## （一） Bochs 软件说明

Bochs 是一款开源的虚拟机软件，它用软件来模拟处理器**取指令**和**执行指令**的过程，以及整个计算机硬件。当它开始运行时，就直接模拟计算机的加电启动过程。

Bochs 本身是一个虚拟机，类似于 VirtualBox，可以作为类似于 VirtualBox 的虚拟机来使用，因此它很容易让你单步跟踪硬盘的启动过程，查看寄存器的内容和机器状态。所以，Bochs 虚拟机可以用来查看程序的运行效果，也可以用来调试程序。

## （二） 调试相关
Bochs 虚拟机启动后，首先在当前的工作文件夹下寻找并读入配置文件 bochsrc.bxrc，然后按它的参数调整当前虚拟机的各种“软硬件”配置和工作参数。

就像一台真正的计算机一样，Bochs 的“处理器”在加电之后，要开始取指令并执行指令。但是，与真正的处理器不同，Bochs 在执行它启动之后的第一条指令时，会停下来，等待你的调试命令。

![](Bochs%20下的程序调试入门.assets/2022-06-26-10-26-28.png)

### 1. 调试的命令
- s 命令
单步执行命令 s （step），用于执行一条指令，然后停下来，显示下一条即将执行的指令
- b 命令
断点指令 b （break），所谓断点，就是事先设置一个 物理内存地址，当处理器执行到这个地址时，就自动停下来。例如，在主引导程序加载到物理内存地址 0x7c00 处打断点：b 0x7c00
- c 命令
c（continue）命令是持续执行的意思，该命令要求处理器不间断地持续执行指令，如果设置了断点，会在断点处停下来
- r 命令
r（register）命令用来显示通用寄存器的内容
- sreg 命令
sreg 命令用来显示段寄存器的内容
- xp 命令
xp（eXamine memory at Physical address）命令用来显示指定物理内存地址处的内容（xp 命令每次只显示一个双字）
显示一个双字：xp 0xB8000
显示两个双字：xp/2 0xB8000
显示 n 个双字：xp/n 0xB8000
- q 命令
q（quit）命令用来退出 Bochs 调试过程
- n 命令
使用 n 命令，可以让 Bochs 自动完成循环过程，并在循环体外的下一条指令前停住。
在执行 rep movsb、rep movsw 和 loop 指令的时候，它们可能会重复执行或循环成千上万次，使用单步调试会很麻烦，也使得我们无法调试循环体后面的程序，这时就可以使用 n 命令来快速结束循环过程，指向循环体外的下一条指令（另一方法，使用 b 命令打断点在循环体外的程序）
- u 命令
u 命令是反汇编命令（反汇编的意思是根据机器指令代码生成可读的汇编语言指令，正好与汇编过程相反。）,u 命令可以使用两个参数：
1). 第一个参数是跟在 \/ 后面的数字，指定反汇编出多少条指令，如：u/2
2). 第二个参数用于指定一个内存地址，Bochs 从这里开始反汇编操作
u 命令执行后，会显示反汇编出来的汇编指令以及各自所在的物理地址，这时可以使用 b 命令打断点，然后再用 c 命令来连续执行
- info 命令
info 命令可以显示多种类型的处理器信息，显示标志寄存器的状态
a. “info eflags” 可以显示标志寄存器的状态；标志名称为小写，则说明该标志为“0”，标志名称为大写，则说明该标志为“1”，（intel8086 的标志寄存器为 FLAGS，在 32 位处理器上扩展为 EFLAGS）
b. “info gdt” 命令用来查看全局描述符表 GDT 的内容
c. “info ldt” 命令用来查看局部描述符表 LDT 的内容
d. “info tss” 命令用来任务状态段 TSS 的内容
- print-stack 命令
在 Bochs 中 print-stack 是查看栈的命令，它可以带一个参数，用于指定显示多少数据。如果不使用参数，则默认显示当前栈中的 16 个字
- creg 命令
命令 “creg” （control register）可以用来查看控制寄存器的内容


### 2. 调试命令的使用示意图
![](Bochs%20下的程序调试入门.assets/调试过程.png)


## （三） 参考书籍
[《x86汇编语言：从实模式到保护模式》](https://book.douban.com/subject/20492528/)