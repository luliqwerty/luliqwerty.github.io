---
title: 二叉树的存储结构
date: 2024-11-20
tags: [数据结构, 树]
categories: 学习
---

# 二叉树的存储结构

## 顺序存储

按满二叉树的结点层次编号，依次存放二叉树中的数据元素。双亲和孩子的关系存储在编号上

```c
typedef TElemType SqBiTree[100];
SqBiTree bt;
```

缺点：深度为 k 的且只有 k 个节点的单支树需要 2^k^ - 1 的一维数组

## 链式存储



```c
typedef struct BiNode {
    TElemType data;
    struct BiNode *lchild;
    struct BiNode *rchild;
} BiNode, *BiTree;
```

quiz：在 n 个结点的二叉链表中，有 ___ 个指针域

## 三叉链表

能够很快的找到一个结点的双亲

```c
typedef struct TriTNode {
    TElemType data;
    struct TriTNode *parent;
    struct TriTNode *lchild;
    struct TriTNode *rchild;
} TriTNode, *TriTree;
```

