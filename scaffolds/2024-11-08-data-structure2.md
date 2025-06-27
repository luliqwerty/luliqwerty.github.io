---
title: 数据结构学习 (二)
date: 2024-11-08 22:00:00
tags: 数据结构
categories: 学习
---
# 栈

**栈的定义：** **栈**（**stack**）是限定仅在表尾进行插入或删除操作的线性表。因此，对栈来说，表尾端有特殊含义，称为**栈顶（top）**，表头端称为**栈底（bottom）**。不含元素的空表称为空栈。

栈的所有修改都只能在表尾操作，弹出栈顶元素称为**弹栈（pop）**，往栈压入元素称为**压栈（push）**，退栈的第一个元素始终是栈顶元素，所以栈是一种**后进先出（LIFO - Last In First Out）**的数据结构。

**栈的数据结构定义**

```c
typedef int elementType;
typedef struct stack {
    elementType *data;		// stack element
    elementType *top;		// top pointer
    int capacity;   		// stack capacity
};
```



# 队列