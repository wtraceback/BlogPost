### 1. “int 9h 中断例程” 和 “int 16h 中断例程”：
#### （1） 使用 int 9h 中断例程对键盘输入的处理
> 键盘输入将引发 9 号中断，BIOS 提供了 int 9 中断例程。CPU 在 9 号中断发生后，执行 int 9 中断例程，从 60h 端口读出扫描码，并将其转化为相应的 ASCII 码或状态信息，**存储在内存的指定空间（键盘缓冲区或状态字节）中**。（键盘缓冲区是用**环形队列**结构管理的内存区）。
> 一般的键盘输入，在 CPU 执行完 int 9 中断例程后，都放到了键盘缓冲区中。键盘缓冲区中有 16 个字单元，可以存储 15 个按键的扫描码和对应的 ASCII 码。

#### （2） 使用 int 16h 中断例程读取键盘缓冲区
> BIOS 提供了 int 16h 中断例程供程序员调用。int 16h 中断例程中包含的一个最重要的功能是从键盘缓冲区中读取一个键盘输入，该功能的编号为 0。下面的指令从键盘缓冲区中读取一个键盘输入，并将其从键盘缓冲区中删除：
>&nbsp;&nbsp;&nbsp;&nbsp;mov ah, 0
>&nbsp;&nbsp;&nbsp;&nbsp;int 16h
>&nbsp;&nbsp;&nbsp;&nbsp;结果：读取出的数据默认写到 ax 寄存器中，(ah)=扫描码，(al)=ASCII码

> **int 16h 中断例程的 0 号子程序的功能：**
> 1. 检测键盘缓冲区中是否有数据；
> 2. 没有则等待，继续做第 1 步；
> 3. 读取键盘缓冲区第一个字单元中的键盘输入；
> 4. 将读取的扫描码送入 ah 中，ASCII 码送入 al；
> 5. 将已读取的键盘输入从键盘缓冲区中删除。

#### （3） BIOS 的 int 9h 中断例程和 int 16h 中断例程是一对相互配合的程序。
> 1. **int 9h 中断例程向键盘缓冲区中写入**（int 9h 中断例程是在有按键按下的时候向键盘缓冲区中写入数据，状态键除外，状态键是存储在状态字节中）。
> 2. **int 16h 中断例程从键盘缓冲区中读出**（int 16h 中断例程是在应用程序对其进行调用的时候，将数据从键盘缓冲区中读出）。

#### （4） 简略示意图
![使用 BIOS 进行键盘输入的简略示意图](./汇编语言(第3版，王爽著)：第-17-章-使用-BIOS-进行键盘输入的学习总结.assets/17731575-d4e25a7cec1bbf33.png)


### 2. 练习 1：
> 编程，接收用户的键盘输入，输入“r”，将屏幕上的字符设置为红色；输入“g”，将屏幕上的字符设置为绿色；输入“b”，将屏幕上的字符设置为蓝色。


> 文件名： pro172.asm

```
assume cs:code

code segment
    start:
        mov ah, 0
        int 16h             ; 将数据从键盘缓冲区中读出，读取出的数据默认写到 ax 寄存器中，(ah)=扫描码，(al)=ASCII码

        mov ah, 1           ; 设置 ah 存放的是字符属性
        cmp al, 'r'         ; 如果对应的 ASCII 码是 r，则将屏幕上的字符设置为红色
        je red
        cmp al, 'g'
        je green
        cmp al, 'b'
        je blue
        jmp short sret

    red:
        shl ah, 1
    green:
        shl ah, 1
    blue:
        mov bx, 0b800h
        mov es, bx
        mov bx, 1
        mov cx, 2000
    s:
        and byte ptr es:[bx], 11111000b
        or es:[bx], ah
        add bx, 2
        loop s

    sret:
        mov ax, 4c00h
        int 21h
code ends

end start
```

![练习1-运行效果图 1](./汇编语言(第3版，王爽著)：第-17-章-使用-BIOS-进行键盘输入的学习总结.assets/17731575-d4281a96ae23d2a8.png)

![练习1-运行效果图 2](./汇编语言(第3版，王爽著)：第-17-章-使用-BIOS-进行键盘输入的学习总结.assets/17731575-baa9cc159582d656.png)

![练习1-运行效果图 3](./汇编语言(第3版，王爽著)：第-17-章-使用-BIOS-进行键盘输入的学习总结.assets/17731575-ceab507995dac6e1.gif)

### 3. 练习 2：
> 字符串的输入，需要具备以下的功能：
> （1）在输入的同时需要显示这个字符串；
> （2）一般在输入回车符后，字符串输入结束；
> （3）能够删除已经输入的字符。


> 文件名： pro173.asm

```
assume cs:code

stack segment
    db 1024 dup (0)                                 ; charstack 的子程序中使用 dw 定义字型数据，所以这里应该写 2^16 = 65536
stack ends

code segment
    start:
        mov ax, stack
        mov ds, ax
        mov si, 0                                   ; ds:si 指向字符栈空间
        mov dh, 13                                  ; 字符串在屏幕上显示的行数
        mov dl, 32                                  ; 字符串在屏幕上显示的列数

        call getstr                                 ; 调用子程序，获取字符串的输入

        mov ax, 4c00h
        int 21h


    ; 接收字符串输入的子程序
    getstr:
        push ax
    getstrs:
        mov ah, 0
        int 16h                                     ; 将数据从键盘缓冲区中读出，读取出的数据默认写到 ax 寄存器中，(ah)=扫描码，(al)=ASCII码
        cmp al, 20h
        jb nochar                                   ; ASCII 小于 20h，说明不是字符
        mov ah, 0
        call charstack                              ; 字符入栈
        mov ah, 2
        call charstack                              ; 显示栈中的字符
        jmp getstrs
    nochar:
        cmp ah, 0eh                                 ; 退格键的扫描码
        je backspace
        cmp ah, 1ch                                 ; Enter 键的扫描码
        je enter
        jmp getstrs
    backspace:
        mov ah, 1
        call charstack                              ; 字符出栈
        mov ah, 2
        call charstack                              ; 显示栈中的字符
        jmp getstrs
    enter:
        mov al, 0
        mov ah, 0
        call charstack                              ; 0 入栈
        mov ah, 2
        call charstack                              ; 显示栈中的字符

        pop ax
        ret


    ; 子程序：字符栈的入栈、出栈和显示
    ; 参数说明：(ah)=功能号，0 表示入栈，1 表示出栈，2 表示显示
    ;           ds:si 指向字符栈空间
    ;           对于 0 号功能：(al)=入栈字符
    ;           对于 1 号功能：(al)=返回字符
    ;           对于 2 号功能：(dh)、(dl)=字符串在屏幕上显示的行、列位置
    charstack:
        jmp short charstart
        table dw charpush, charpop, charshow
        top dw 0                                    ; 栈顶

    charstart:
        push bx
        push dx
        push di
        push es

        cmp ah, 2
        ja sret                                     ; 大于 2，则跳转返回
        mov bl, ah
        mov bh, 0
        add bx, bx                                  ; table 使用 dw 定义数据，所以需要 *2
        jmp word ptr table[bx]

    charpush:
        mov bx, top
        mov [si][bx], al
        inc top
        jmp sret

    charpop:
        cmp top, 0
        je sret
        dec top
        mov bx, top
        mov al, [si][bx]
        jmp sret

    charshow:
        mov bx, 0b800h
        mov es, bx
        mov al, 160
        mov ah, 0
        mul dh                                      ; 8 位乘法，结果放在 ax 中
        mov di, ax
        add dl, dl
        mov dh, 0
        add di, dx                                  ; 行*160 + 列*2

        mov bx, 0

    charshows:
        cmp bx, top
        jne noempty
        mov byte ptr es:[di], ' '
        jmp sret
    noempty:
        mov al, [si][bx]
        mov es:[di], al
        mov byte ptr es:[di+2], ' '
        inc bx
        add di, 2
        jmp charshows

    sret:
        pop es
        pop di
        pop dx
        pop bx
        ret
code ends

end start
```

![练习2-运行效果图](./汇编语言(第3版，王爽著)：第-17-章-使用-BIOS-进行键盘输入的学习总结.assets/17731575-23543f2f68b1153f.gif)
