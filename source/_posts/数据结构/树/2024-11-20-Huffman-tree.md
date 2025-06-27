---
title: 哈夫曼树
date: 2024-11-20
tags: 树
categories: 数据结构
---
# 哈夫曼树的构造

构造哈夫曼树的口诀

1. 构造森林全是根
2. 选用两小造新树
3. 删除两小添新根
4. 重复2、3剩单根



顺序存储 哈夫曼树 存储结构

```c
typedef struct HTNode {
    int weight;		// 权值
    int parent;		// 双亲结点
    int lchild;		// 左子树
    int rchild;		// 右子树
}HTNode, *HuffmanTree;
```

初始状态所有的结点都是独立的一棵树（根节点），parent, lchild, rchild 都是 0