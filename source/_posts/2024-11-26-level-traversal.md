---
title: 树的层次遍历
date: 2024-11-26
tags: [数据结构, 树]
categories: 学习
---
# 树的层次遍历

把树看成从上到下的 n 层，一层一层访问，每一层从左到右访问每一个节点，每一个节点只访问一次

算法设计思路：**使用一个队列**

1. 将根节点入队
2. 在队列不为空的情况下访问每一个节点，如果他有左右孩子那么都入队

    ```c
    typedef struct BiTreeNode {
        int data;
        struct BiTreeNode *lchild;
        struct BiTreeNode *rchild;
    } *BiTree;

    // 也就是所谓的 DFS 算法
    void levelVisit(Queue q, BiTree tree) {
        q.push(tree);
        while (!q.empty()) {
            BiTree node = q.top();
            if (node.lchild) {
                q.push(node.lchild);
            }
            if (BiNode.rchild) {
                q.push(node.rchild);
            }
            q.pop();
        }
    }
    ```

