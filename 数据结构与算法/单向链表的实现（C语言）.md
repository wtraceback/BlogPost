```
#include <stdio.h>
#include <stdlib.h>         // 包含了 malloc 和 exit 函数
#include <stdbool.h>        // 包含了 bool 数据类型

typedef struct node {
    int value;              // 数据域
    struct node *next;      // 指针域
} Node;                     // Node 等价于 struct node

// 函数声明
Node * createList(void);
void traverseList(Node *pHead);
bool isEmpty(Node *pHead);
int lengthList(Node *pHead);
bool insertList(Node *pHead, int pos, int val);
bool deleteList(Node *pHead, int pos, int *val);
void sortList(Node *pHead);

int main() {
    Node *pHead = NULL;        // 头指针

    pHead = createList();
    traverseList(pHead);
    printf("\n");

    /*
    if (isEmpty(pHead) ) {
        printf("链表为空！\n");
    } else {
        printf("链表不为空！\n");
    }
    */

    // printf("链表的长度为：len = %d\n", lengthList(pHead));

    /*
    if ( insertList(pHead, 1, 10) == true ) {
        printf("数据插入成功！\n");
    } else {
        printf("数据插入失败！\n");
    }
    traverseList(pHead);
    printf("\n");
    */

    /*
    int value;
    if ( deleteList(pHead, 1, &value) == true ) {
        printf("数据删除成功！\n");
        printf("被删除的值是 value = %d\n", value);
    } else {
        printf("数据删除失败！\n");
    }
    traverseList(pHead);
    printf("\n");
    */

    printf("链表的长度为：len = %d\n", lengthList(pHead));

    printf("对链表进行排序\n");
    sortList(pHead);
    traverseList(pHead);
    printf("\n");

    return 0;
}

// 链表初始化：创建一个非循环单向链表，并将该链表的头节点的地址返回
Node * createList(void) {
    int len;    // 用来存放有效节点的个数

    // 分配了一个不存放有效数据的头节点
    Node *head = (Node *)malloc(sizeof(Node));
    if (NULL == head) {
        printf("动态内存分配失败，程序终止！\n");
        exit(-1);
    }
    head->next = NULL;

    // 临时指针变量，用来移位
    Node * pTemp = head;

    printf("请输入您需要生成的链表节点的个数: len = ");
    scanf("%d", &len);

    int val;
    for (int i = 0; i < len; i++) {
        printf("请输入第 %d 个节点的值: ", i + 1);
        scanf("%d", &val);

        Node *t = (Node *)malloc(sizeof(Node));
        if (NULL == t) {
            printf("动态内存分配失败，程序终止！\n");
            exit(-1);
        }

        t->value = val;
        t->next = NULL;
        pTemp->next = t;
        pTemp = t;
    }

    return head;
}

// 遍历链表
void traverseList(Node *pHead) {
    Node *p = pHead->next;

    while (NULL != p) {
        printf("%d  ", p->value);
        p = p->next;
    }
    printf("\n");

    return;
}

// 判断链表是否为空
bool isEmpty(Node *pHead) {
    if (NULL == pHead->next) {
        return true;
    } else {
        return false;
    }
}

// 求链表的长度
int lengthList(Node *pHead) {
    int len = 0;
    Node *p = pHead->next;

    while (NULL != p) {
        p = p->next;
        len = len + 1;
    }

    return len;
}

// 插入新的节点，在第 pos 个节点上
bool insertList(Node *pHead, int pos, int val) {
    int len = lengthList(pHead);

    // 判断插入的位置是否在允许的范围内
    if (pos <= 0 || pos > len + 1 ) {
        return false;
    }

    // 将指针 p 调整到需要插入的位置的前一个，即 pos - 1
    Node *p = pHead;
    for (int i = 1; i <= pos - 1; i++)  {
        p = p->next;
    }

    // 分配新的节点
    Node *n = (Node *)malloc(sizeof(Node));
    if (NULL == n) {
        printf("动态内存分配失败！\n");
        exit(-1);
    }
    n->value = val;

    // 将新的节点插入 p 节点的后面
    n->next = p->next;
    p->next = n;

    return true;
}

// 删除节点：删除第 pos 个节点，并释放该节点的内存
bool deleteList(Node *pHead, int pos, int *val) {
    int len = lengthList(pHead);

    // 判断删除的位置是否在允许的范围内
    if (pos <= 0 || pos > len ) {
        return false;
    }

    // 将指针 p 调整到需要删除的位置的前一个节点，即 pos - 1
    Node *p = pHead;
    for (int i = 1; i <= pos - 1; i++)  {
        p = p->next;
    }

    // 先将 p 节点后面的节点赋值给 q，然后再替换掉 p 节点后面的节点
    Node *q = p->next;    // q 指向被删除的节点
    *val = q->value;
    p->next = p->next->next;

    // 释放 q 指针变量所指向的节点所占用的内存
    free(q);
    q = NULL;

    return true;
}

/*
// 对链表进行排序：冒泡排序，将整个节点以及指向都改变了
void sortList(Node *pHead) {
	int n = lengthList(pHead);

    for (int i = 0; i < n - 1; i++) {
        Node *q = pHead;
        for (int j = 0; j < n - 1 - i; j++, q=q->next) {
            if (q->next->value > q->next->next->value) {
                Node *t = q->next;
                q->next = t->next;
                t->next = q->next->next;
                q->next->next = t;
            }
        }
    }
}
*/

// 对链表进行排序：冒泡排序，只改变各个节点里面的值，不改变指向
void sortList(Node *pHead) {
	int n = lengthList(pHead);

    for (int i = 0; i < n - 1; i++) {
        Node *q = pHead->next;
        for (int j = 0; j < n - 1 - i; j++, q=q->next) {
            if (q->value > q->next->value) {
                int value = q->value;
                q->value = q->next->value;
                q->next->value = value;
            }
        }
    }
}
```
