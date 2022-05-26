### 1. 先序中序后序遍历的原理
#### （1） 先序遍历
规则：
1）先访问根节点
2）再先序遍历左子树
3）最后先序遍历右子树
![先序遍历示意图](./链式二叉树的前序中序后序遍历(递归实现)（C语言）.assets/17731575-1161072f059b891a.png)
#### （2） 中序遍历
规则：
1）中序遍历左子树
2）再访问根节点
3）最后中序遍历右子树
![中序遍历示意图](./链式二叉树的前序中序后序遍历(递归实现)（C语言）.assets/17731575-04be29bfffcabfa6.png)
#### （3） 后序遍历
规则：
1）先后序遍历左子树
2）再后序遍历右子树
3）最后访问根节点
![后序遍历示意图](./链式二叉树的前序中序后序遍历(递归实现)（C语言）.assets/17731575-551ff7b1e5db95c2.png)


### 2. 代码实现
#### （1） 链式二叉树的前序中序后序遍历(递归实现代码)
```
#include <stdio.h>
#include <stdlib.h>     // malloc

typedef struct TreeNode {
    char data;                       // 数据域
    struct TreeNode * lchild;       // 左孩子指针
    struct TreeNode * rchild;       // 右孩子指针
} TreeNode;

TreeNode *createBTree();
void preOrderTraversal(TreeNode *T);
void inOrderTraversal(TreeNode *T);
void postOrderTraversal(TreeNode *T);

int main() {
    TreeNode * BTree = createBTree();

    printf("前序遍历的结果：\n");
    preOrderTraversal(BTree);
    printf("\n\n");

    printf("中序遍历的结果：\n");
    inOrderTraversal(BTree);
    printf("\n\n");

    printf("后序遍历的结果：\n");
    postOrderTraversal(BTree);
    printf("\n\n");

    return 0;
}

// 创建一个链式二叉树
TreeNode *createBTree() {
    TreeNode *pa = (TreeNode *)malloc(sizeof(TreeNode));
    TreeNode *pb = (TreeNode *)malloc(sizeof(TreeNode));
    TreeNode *pc = (TreeNode *)malloc(sizeof(TreeNode));
    TreeNode *pd = (TreeNode *)malloc(sizeof(TreeNode));
    TreeNode *pe = (TreeNode *)malloc(sizeof(TreeNode));

    pa->data = 'A';
    pa->lchild = pb;
    pa->rchild = pc;

    pb->data = 'B';
    pb->lchild = pd;
    pb->rchild = pe;

    pc->data = 'C';
    pc->lchild = pc->rchild = NULL;

    pd->data = 'D';
    pd->lchild = pd->rchild = NULL;

    pe->data = 'E';
    pe->lchild = pe->rchild = NULL;

    return pa;
}

// 二叉树的前序遍历递归算法
void preOrderTraversal(TreeNode *T) {
    if (NULL == T) {
        return;
    } else {
        printf("%c ", T->data);             // 显示节点数据，可以更改为其他对节点的操作
        preOrderTraversal(T->lchild);       // 再先序遍历左子树
        preOrderTraversal(T->rchild);       // 最后先序遍历右子树
    }
}

// 二叉树的中序遍历递归算法
void inOrderTraversal(TreeNode *T) {
    if (NULL == T) {
        return;
    } else {
        inOrderTraversal(T->lchild);        // 中序遍历左子树
        printf("%c ", T->data);             // 显示节点数据，可以更改为其他对节点的操作
        inOrderTraversal(T->rchild);        // 最后中序遍历右子树
    }
}

// 二叉树的后序遍历递归算法
void postOrderTraversal(TreeNode *T) {
    if (NULL == T) {
        return;
    } else {
        postOrderTraversal(T->lchild);      // 先后序遍历左子树
        postOrderTraversal(T->rchild);      // 再后序遍历右子树
        printf("%c ", T->data);             // 显示节点数据，可以更改为其他对节点的操作
    }
}
```
#### （2） 示意图
![链式二叉树的前序中序后序遍历(递归实现)-示意图](./链式二叉树的前序中序后序遍历(递归实现)（C语言）.assets/17731575-a5da4fdc6e63c9c4.png)
