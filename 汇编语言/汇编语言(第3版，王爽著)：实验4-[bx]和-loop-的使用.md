![实验4](./汇编语言(第3版，王爽著)：实验4-[bx]和-loop-的使用.assets/17731575-dff90551cf1126ab.png)


#### （1）编程，向内存 0:200~0:23F 一次传送数据 0~63(3FH)。
```
assume cs:code

code segment

    mov ax, 0020h
    mov ds, ax
    mov bx, 0

    mov cx, 64                  ; (cx)=64，循环 64 次

s:  mov ds:[bx], bl             ; 将一个字节送入内存单元 0020:(bx) 中
    inc bx                      ; (bx)=(bx)+1
    loop s

    mov ax, 4c00h
    int 21h

code ends

end
```

#### （2）编程，向内存 0:200~0:23F 一次传送数据 0~63(3FH)，程序中只能使用 9 条指令，9 条指令中包括“mov ax, 4c00h” 和 “int 21h”。
```
代码同上
```

#### （3）下面的程序的功能是将 “mov ax, 4c00h”  之前的指令复制到内存 0:200 处，补全程序。上机调试，跟踪运行结果。
代码补全：
![代码补全](./汇编语言(第3版，王爽著)：实验4-[bx]和-loop-的使用.assets/17731575-ceb9d86b43eb4aca.png)
代码补全的分析：（需要用 debug 来查看 “mov ax, 4c00H” 前面的字节数）
![代码补全分析](./汇编语言(第3版，王爽著)：实验4-[bx]和-loop-的使用.assets/17731575-11b40b77c6eba268.png)

debug 跟踪结果：
![debug 跟踪结果](./汇编语言(第3版，王爽著)：实验4-[bx]和-loop-的使用.assets/17731575-1692ce9857adf64d.png)
