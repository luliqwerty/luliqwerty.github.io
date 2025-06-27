---
title: 计算二叉树的深度
date: 2024-11-26
tags: 树
categories: 数据结构
---
# 计算二叉树的深度

1. 如果是空树，那么深度为 0

2. 否则，递归计算左子树的深度 m，递归计算右子树的深度记为 n，整棵树的深度是 max(m, n) + 1

    ```c
    int calculateDepth(BiTree tree) {
        if (tree == NULL) {
            return 0;
        } else {
            m = calculateDepth(tree->lchild);
            n = calculateDepth(tree->rchild);
            if (m > n) return m + 1;
            else return n + 1;
        }
    }
    ```

    

