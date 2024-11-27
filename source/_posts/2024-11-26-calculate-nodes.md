---
title: 计算二叉树节点个数
date: 2024-11-26
tags: [数据结构, 树]
categories: 学习
---
# 计算二叉树的结点个数

1. 如果是空树，那么结点个数为 0

2. 否则，整棵树结点个数 = 左子树结点个数 + 右子树结点个数 + 1

    ```c
    int calculateNode(BiTree tree) {
        if (tree == NULL) {
            return 0;
        } else {
            return 	calculateNode(tree->lchild) +
                	calculateNode(tree->rchild) + 1;
        }
    }
    ```

    