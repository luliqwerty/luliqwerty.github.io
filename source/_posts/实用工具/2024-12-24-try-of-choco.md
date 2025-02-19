---
title: windows命令行Chocolatey的使用
date: 2024-12-24
tags: chocolatey
categories: tools
---

## Chocolatey安装和使用

### 前言

Linux 有 apt-get、apt

Mac 有 brew

Windows 呢？

显而易见，虽然官方不提供，但是无所不能的程序员肯定是可以实现的呀！！！

接下来就是 **[Chocolatey 下载链接](https://chocolatey.org/install#individual)** 的主场啦

### 安装 choco

1. cmd 管理员：

    ```
    @"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
    ```

2. powershell 管理员

    ```
    Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
    ```

接下来就等待安装完成，如果安装失败请到官网或者互联网找解决方案

运行 `choco` 或者 `choco -?` 检查是否正确安装

### 使用 choco

1. choco search 关键字

2. choco install [package]

    ```shell
    choco install 软件包名称
    // 安装Node：
    choco install nodejs.install //最新版本，当前是11.6.0
    choco install nodejs-lts     //lts的最新版本，当前是10.15.0
    
    // 安装git：
    choco install git.install
    // 安装Chrome：
    choco install googlechrome
    // 安装VS Code：
    choco install vscode
    // 安装7-zip：
    choco install 7zip.install
    // 安装IntelliJ IDEA：
    choco install intellijidea-community //社区版
    choco install intellijidea-ultimate  //旗舰版
    ```

3. 更新包
    choco upgrade 软件包名称

4. 卸载安装包
    choco uninstall 软件包名称

5. 如果你不想在命令行中搜索和安装包的话，可以安装ChocolateyGUI，这是一个图形化的界面

    choco install chocolateygui

    安装之后输入 chocolateygui 打开图形界面安装你想安装的软件即可

除了在命令行中搜索软件包，还可以直接在Chocolatey网站上搜索软件包，网址是：[Chocolatey Software | Packages](https://community.chocolatey.org/packages)，在网站上有一些同名的软件包，不同之处在于一个后面有Install，另一个则没有。

这两者的区别是：有Install的软件包在安装之后，会在控制面板的添加和删除程序中找到。