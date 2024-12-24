---
title: 复制二叉树
date: 2024-11-26
tags: 树
categories: 数据结构
---
# 复制二叉树

复制二叉树需要复制所有结点的值

递归实现

```c
void copy(BiTree source, BiTree copy) {
    if (source == NULL) {
        copy = NULL;
        return;
    } else {
        // 分配新结点
        copy = (BiTree)malloc(sizeof(BiTreeNode));
        // 复制根结点的值
        copy->data = source->data;
        // 复制左子树
        copy(source->lchild, copy->lchild);
        // 复制右子树
        copy(source->rchild, copy->rchild);
    }
}
```

