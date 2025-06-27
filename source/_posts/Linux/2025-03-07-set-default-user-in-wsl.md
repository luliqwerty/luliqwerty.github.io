---
title: 修改 WSL 默认用户为 root
date: 2025-03-07
categories: Linux
tags: [wsl2, 踩坑]
---
## 前言

> 这是在我学习 `Redis` 的过程中遇到的问题

因为在学习过程不能每一次手动重启 `Redis` 所以需要修改 **Redis.conf** 文件。

然后就发现想从 Windows 文件资源管理器打开 WSL 文件的时候爆了 **没有权限** 的问题。

解决方法如下：
## 修改 WSL 默认用户为 root
修改默认用户： 在 Windows 终端中执行以下命令，将默认用户改为 root：
```cmd
C:\Users\你的用户名\AppData\Local\Microsoft\WindowsApps\发行版名称.exe config --default-user root
```
例如，对于 Ubuntu 24.04，命令为：
```cmd
C:\Users\Administrator\AppData\Local\Microsoft\WindowsApps\ubuntu2404.exe config --default-user root
```
重启 WSL： 修改完成后，重启 WSL：
```bash
wsl --shutdown
wsl
```
这样，通过 \\\wsl$ 访问文件时将以 root 用户身份操作，避免权限不足的问题 