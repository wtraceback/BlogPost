这是我之前写的一个小游戏，在windows下的Dev-C++中用C语言来实现的，通过不断的清屏，打印，清屏，打印来实现动态效果，不过功能不完善，有几个缺陷的地方。

## 1. 实现效果（使用 w a s d 四个键来控制上下左右的移动） ：

![实现效果](./Windows%E4%B8%8B%E4%BD%BF%E7%94%A8C%E8%AF%AD%E8%A8%80%E5%AE%9E%E7%8E%B02048.assets/175613_xWkJ_2363215.gif)

源代码（如果在Dev-C++中编译运行的时候报错，请将注释符号 <font color='red'> // </font> 换为 <font color='red'> /**/ </font> 来进行注释）：


## 2. 源码
```
#include <stdio.h>
#include <stdlib.h>     // for rand() and srand() and exit()
#include <time.h>       // for time()
#include <conio.h>      // for getch()
#include <windows.h>    // for system()

void init(void);      // 初始化数组跟赋值第一个随机二维数组元素
void draw(void);      // 绘制4 * 4方格图
void play(void);      // 控制移动方向
void to_up(void);     // 向上移动
void to_down(void);  // 像下移动
void to_left(void);   // 向左移动
void to_right(void);  // 向右移动
void add_number(void);  // 加新的数

int a[4][4];
int empty;

int
main(void) {
    printf("****************************\n");
    printf("            2048            \n\n");
    printf("Control by:\n"
            " w/s/a/d or W/S/A/D\n");
    printf("press q or Q quit game!\n");
    printf("****************************\n");
    printf("Press any key to continue . . .\n");
    getch();
    system("cls");

    init();
    draw();
    while(1)
        play();


    return 0;
}

void
init(void) {
    int i, j;

    for(i = 0; i < 4; ++i)
        for(j = 0; j < 4; ++j)
            a[i][j] = 0;
    srand(time(0));
    i = rand() % 4;
    j = rand() % 4;
    a[i][j] = 2;
    empty = 15;
}

void
draw(void) {
    int i, j;

    for(i = 0; i < 4; ++i) {          // 一个方格由三根竖线组成
        for(j = 0; j < 4; ++j)     // 第一排竖线 每个竖线之间占5个格
            printf("|    ");
        printf("|\n");

        for(j = 0; j < 4; ++j) {   // 第二排竖线与数字
            if(a[i][j] == 0)
                printf("|    ");
            else
                printf("|%4d", a[i][j]);
        }
        printf("|\n");

        for(j = 0; j < 4; ++j)     // 第三排竖线加底线
            printf("|____");
        printf("|\n");
    }
}

void
play(void) {
    int ch;

    ch = getch();
    switch(ch) {
        case 'w':    // 向上移动
        case 'W':
            to_up();
            system("cls");
            add_number();
            draw();
            break;
        case 's':    // 向下移动
        case 'S':
            to_down();
            system("cls");
            add_number();
            draw();
            break;
        case 'a':    // 向左移动
        case 'A':
            to_left();
            system("cls");
            add_number();
            draw();
            break;
        case 'd':     // 向右移动
        case 'D':
            to_right();
            system("cls");
            add_number();
            draw();
            break;
        case 'q':   // 退出游戏
        case 'Q':
            exit(0);
            break;
        default:
            printf("\nwrong type!!!\n\n");
            printf("please type :\n");
            printf("w/s/a/d or W/S/A/D\n");
            break;
    }
}

void
to_up(void) {
    int x, y, i;

    for(y = 0; y < 4; ++y) {     // 从上向下合并相同的方块
        for(x = 0; x < 4; ++x) {
            if(a[x][y] == 0)
                ;
            else {
                for(i = x + 1; i < 4; ++i) {
                    if(a[i][y] == 0)
                        ;
                    else if(a[x][y] == a[i][y]) {
                        a[x][y] += a[i][y];
                        a[i][y] = 0;
                        ++empty;
                        x = i;
                        break;
                    }
                    else {
                        //x = i - 1;
                        break;
                    }
                }
            }
        }
    }

    for(y = 0; y < 4; ++y)    // 向上移动箱子
        for(x = 0; x < 4; ++x) {
            if(a[x][y] == 0)
                ;
            else {
                for(i = x; (i > 0) && (a[i - 1][y] == 0); --i) {
                    a[i - 1][y] = a[i][y];
                    a[i][y] = 0;
                }
            }
        }
}

void
to_down(void) {
    int x, y, i;

    for(y = 0; y < 4; ++y)  // 向下合并相同的方格
        for(x = 3; x >= 0; --x) {
            if(a[x][y] == 0)
                ;
            else {
                for(i = x - 1; i >= 0; --i) {
                    if(a[i][y] == 0)
                        ;
                    else if(a[x][y] == a[i][y]) {
                        a[x][y] += a[i][y];
                        a[i][y] = 0;
                        ++empty;
                        x = i;
                        break;
                    }
                    else
                        break;
                }
            }
        }

    for(y = 0; y < 4; ++y)  // 向下移动方格
        for(x = 3; x >= 0; --x) {
            if(a[x][y] == 0)
                ;
            else {
                for(i = x; (i < 3) && (a[i + 1][y] == 0); ++i) {
                    a[i + 1][y] = a[i][y];
                    a[i][y] = 0;
                }
            }
        }
}

void
to_left(void) {
    int x, y, i;

    for(x = 0; x < 4; ++x)   // 向左合并相同的方格
        for(y = 0; y < 4; ++y) {
            if(a[x][y] == 0)
                ;
            else {
                for(i = y + 1; i < 4; ++i) {
                    if(a[x][i] == 0)
                        ;
                    else if(a[x][y] == a[x][i]) {
                        a[x][y] += a[x][i];
                        a[x][i] = 0;
                        ++empty;
                        y = i;
                        break;
                    }
                    else
                        break;
                }
            }
        }

    for(x = 0; x < 4; ++x)  // 向左移动方格
        for(y = 0; y < 4; ++y) {
            if(a[x][y] == 0)
                ;
            else {
                for(i = y; (i > 0) && (a[x][i - 1] == 0); --i) {
                    a[x][i - 1] = a[x][i];
                    a[x][i] = 0;
                }
            }
        }
}

void
to_right(void) {
    int x, y, i;

    for(x = 0; x < 4; ++x)  // 向右合并相同的方格
        for(y = 3; y >= 0; --y) {
            if(a[x][y] == 0)
                ;
            else {
                for(i = y - 1; i >= 0; --i) {
                    if(a[x][i] == 0)
                        ;
                    else if(a[x][y] == a[x][i]) {
                        a[x][y] += a[x][i];
                        a[x][i] = 0;
                        ++empty;
                        y = i;
                        break;
                    }
                    else
                        break;
                }
            }
        }

    for(x = 0; x < 4; ++x)   // 向右移动方格
        for(y = 3; y >= 0; --y) {
            if(a[x][y] == 0)
                ;
            else {
                for(i = y; (i < 3) && (a[x][i + 1] == 0); ++i) {
                    a[x][i + 1] = a[x][i];
                    a[x][i] = 0;
                }
            }
        }
}

void
add_number(void) {
    int temp, number;
    int x, y;

    if(empty > 0) {     // 找出空格
        srand(time(0));
        do {
            x = rand() % 4;
            y = rand() % 4;
        } while(a[x][y] != 0);

        number = rand();
        temp = number % 2;

        if(temp == 1) {  // 判断是生成数字2，还是数字4
            a[x][y] = 2;
            --empty;
        }
        if(temp == 0) {
            a[x][y] = 4;
            --empty;
        }
    }

}
```