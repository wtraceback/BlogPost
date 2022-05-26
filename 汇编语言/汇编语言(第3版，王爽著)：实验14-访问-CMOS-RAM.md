![实验14 访问 CMOS RAM](./汇编语言(第3版，王爽著)：实验14-访问-CMOS-RAM.assets/17731575-0408a40a4b23e189.png)

### 1. 思路：
> 1. 可以仿照 **实验13 的第 3 个例子** 的编程思想
> 2. 将要显示的时间格式预先定义好，从 CMOS RAM 中获取到的 BCD 码，将其转换成对应的 ASCII 码字符后，再替换输出格式中的某些字符
> 3. 将 CMOS RAM 中，存放当前时间的地址信息先列出来（年、月、日、时、分、秒 的存放单元） ，方便代码遍历的去获取相关的信息

### 2. 代码实现1：
> 文件名：exp14.asm
> 方法：调用 BIOS 和 DOS 提供的中断例程，显示字符串
```
assume cs:code

code segment
    db 'yy/MM/dd HH:mm:ss', '$'
    address: db 9,8,7,4,2,0

    start:
        mov ax, cs
        mov ds, ax
        mov si, 0
        mov di, offset address      ; CMOS RAM 中地址信息的索引

        mov cx, 6
    s:
        push cx

        mov al, ds:[di]
        out 70h, al
        in al, 71h                  ; 从 CMOS RAM 读取 BCD 码

        mov ah, al
        mov cl, 4
        shr ah, cl
        and al, 00001111b
        add ah, 30h
        add al, 30h                 ; 将读取到的 BCD 码经过一系列的转换，转为对应的 ASCII 码字符（显存中识别的是 ASCII 码）

        mov ds:[si], ah
        mov ds:[si+1], al           ; 将转换后的 ASCII 码字符放入预先定义好的字符串中

        add si, 3
        inc di
        pop cx
        loop s


        mov ah, 2                   ; 置光标
        mov bh, 0                   ; 第 0 页
        mov dh, 13                  ; dh 中放行号
        mov dl, 32                  ; dl 中放列号
        int 10h

        mov dx, 0                   ; 设定要读取的字符串地址，ds:dx 指向字符串的首地址 data:0
        mov ah, 9
        int 21h                     ; 调用 BIOS 和 DOS 提供的中断例程，显示字符串

        mov ax, 4c00h
        int 21h
code ends

end start
```

![代码实现1 程序运行示意图](./汇编语言(第3版，王爽著)：实验14-访问-CMOS-RAM.assets/17731575-076de74a70270315.png)


### 3. 代码实现2：
> 文件名：exp14-2.asm
> 方法：将要显示的字符送入到显卡上的显存中，显示字符串

```
assume cs:code

data segment
    db 'yy/MM/dd HH:mm:ss'
    db 9,8,7,4,2,0
data ends

code segment
    start:
        mov ax, data
        mov ds, ax
        mov si, 0
        mov di, 17                  ; CMOS RAM 中地址信息的索引

        mov cx, 6
    s:
        push cx

        mov al, ds:[di]
        out 70h, al
        in al, 71h                  ; 从 CMOS RAM 读取 BCD 码

        mov ah, al
        mov cl, 4
        shr ah, cl
        and al, 00001111b
        add ah, 30h
        add al, 30h                 ; 将读取到的 BCD 码经过一系列的转换，转为对应的 ASCII 码字符（显存中识别的是 ASCII 码）

        mov ds:[si], ah
        mov ds:[si+1], al           ; 将转换后的 ASCII 码字符放入预先定义好的字符串中

        add si, 3
        inc di
        pop cx
        loop s


        mov ax, data
        mov ds, ax
        mov si, 0
        mov cx, 17

        mov ax, 0b800h
        mov es, ax
        mov di, 160 * 13 + 32 * 2   ; 将二进制数送到显存缓冲区（显卡上的显存中），显卡会用 ASCII 码的规则解释显存中的内容，会立即在显示器上显示出来

    show_str:
        mov al, ds:[si]
        mov ah, 02h                 ; 字符属性
        mov es:[di], ax

        inc si
        add di, 2
        loop show_str

        mov ax, 4c00h
        int 21h
code ends

end start
```

![代码实现2 程序运行示意图](./汇编语言(第3版，王爽著)：实验14-访问-CMOS-RAM.assets/17731575-ec3ce09da12753a6.png)
