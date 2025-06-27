---
title: 建立二叉树
date: 2024-11-26
tags: 树
categories: 数据结构
---



# 建立二叉树

## 按照先序遍历序列建立二叉树的二叉链表

[![2024-11-26.jpg](https://i.postimg.cc/Gty4P5pd/2024-11-26.jpg)](https://postimg.cc/8fTkNwW0)

在这个例子中，已知一个序列不能唯一确定一棵二叉树

```c
typedef struct BiTreeNode {
    int data;
    struct BiTreeNode *lchild;
    struct BiTreeNode *rchild;
} *BiTree;

//  一般空结点会用特殊字符表示，自己特判就行
//  ABC##DE#G##F###
// 分别是C  E  G F A 的空结点
BiTree preOrderCreatTree(BiTree tree) {
    char ch;
    scanf("%c", &ch);
    if (ch == '#') {
        return NULL;
    } else {
        BiTree node = (BiTreeNode)malloc(sizeof(BiTreeNode)); 	// 生成新结点
        node->data = c;											// 构造根结点
        preOrderCreatTree(node->lchild);						// 构造左子树
        preOrderCreatTree(node->rchild);						// 构造右子树
    }
}
```

