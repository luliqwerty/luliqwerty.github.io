---
title: Hexo自动转义文章URL为拼音
date: 2024-12-25
tags: hexo
categories: tools
---

## 使用场景
Hexo博客中导入的部分外部库不支持中文Url，故需要转义

## 解决方法
1. 先安装 hexo-permalink-pinyin 插件
    ```shell
    npm i hexo-permalink-pinyin --save
    ```

2. 打开Hexo配置文件 _config.yml ，添加如下内容：
    ```shell
    permalink_pinyin:
    enable: true
    separator: "-"
    #exclude: #排除的文章，排出后不会进行转义
    ```
