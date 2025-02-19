---
title: Spring Boot 版本和 JDK 版本
date: 2025-02-11
tags: Lombok
categories: JavaWeb
---

> 起因是我在学习过程中看见别人的 IDEA 界面上的工具按钮很多，而我的界面仅有几个不是很实用的按钮，所以打算设置一下 appearance，结果不知道出了什么问题我的程序运行不了了，我想着恢复默认设置就能解决问题...... 然后遇到了如下报错

```java
java: java.lang.NoSuchFieldError: Class com.sun.tools.javac.tree.JCTree$JCImport does not have member field 'com.sun.tools.javac.tree.JCTree qualid'
```

在 `stack overflow` 找到如下解决方案，最终我并没有更改 Lombok 的版本，而是改掉了 `JDK` 的版本。

![image-20250211150433799](https://i.postimg.cc/VffHhR3k/image-20250211150433799.png?dl=1)