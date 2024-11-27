---
title: 计算二叉树的叶子数
date: 2024-11-26
tags: [数据结构, 树]
categories: 学习
---
# 计算二叉树的叶子数

叶子数即是没有左右孩子的结点

```c
int calculateLeaf(BiTree tree) {
    if (tree == NULL) {
        return 0;
    } else if(tree->lchild && tree->rchild) {
        return 1;
    } else {
        return 	calculateLeaf(tree->lchild) +
            	calculateLeaf(tree->rchild);
    }
}
```

