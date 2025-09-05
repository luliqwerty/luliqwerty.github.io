---
layout: post
title: How to use Git
date: 2024-07-02
tags: Git
categories: 实用工具
---
## 将本地仓库上传到远程仓库

将新建的本地仓库上传到 GitHub 需要以下几个步骤：首先在 GitHub 上创建新的空仓库，然后将本地仓库推送到远程仓库。

**操作步骤：**

```bash
# 初始化本地 Git 仓库
git init

# 连接到远程 GitHub 仓库
git remote add origin https://github.com/luliqwerty/note.git

# 添加文件到暂存区
git add yourChangedFile

# 重命名分支为 main（-M 表示强制重命名）
git branch -M main

# config
git config --global user.name="luliqwerty"
git config --global user.email="hluli182@gmail.com"

# 提交更改到本地仓库
# commit 用于将暂存区的修改保存到仓库历史记录中，生成唯一的快照
git commit -m "first commit"

# 推送到远程仓库
git push origin main
```

## git config

config 是用于 git 识别你的身份的，当你使用 `git commit` 的时候，git 会用你的配置创建 commit infomations。

```bash
git config --global user.name
git config --global user.email
git config --global user.passward

# 获取配置
git config get user.name
```

## git clone
### 下载一个仓库的特定分支

- `-b <分支名>`：指定要克隆的分支。
- `--single-branch`：表示只克隆该分支，不下载其他分支的历史记录（节省时间和空间）。
- 如果你省略 `--single-branch`，Git 会下载整个仓库的所有分支，但当前检出的是你指定的分支。

```bash
git clone -b <分支名> --single-branch <仓库地址>

git clone -b dev --single-branch https://github.com/example/repo.git
这会只下载 `dev` 分支的内容，而不是整个仓库的所有分支。

```

## git remote

当你已经克隆了仓库到本地后，可以使用以下 Git 命令查看仓库的相关信息：

### 1. 查看远程仓库地址

使用以下命令查看当前仓库的远程 URL：

```bash
git remote -v
```

**输出示例：**

```
origin  https://github.com/yourusername/your-repo.git (fetch)
origin  https://github.com/yourusername/your-repo.git (push)
```

这会显示当前本地仓库关联的远程仓库地址。

### 2. 添加上游仓库

将原仓库添加为 `upstream`（名称可以自定义，但通常用 `upstream`）：

```bash
git remote add upstream git@github.com:原仓库所有者/原仓库名.git
```

> **注意**：URL 可以是 `HTTPS`（`https://github.com/...`）或 `SSH`（`git@github.com:...`），推荐使用 SSH（需配置 GitHub SSH Key）。

**验证是否添加成功**

再次运行 `git remote -v`，现在应该能看到 `origin`（你的 Fork）和 `upstream`（原仓库）：

```bash
origin    git@github.com:你的用户名/仓库名.git (fetch)
origin    git@github.com:你的用户名/仓库名.git (push)
upstream  git@github.com:原仓库所有者/原仓库名.git (fetch)
upstream  git@github.com:原仓库所有者/原仓库名.git (push)
```

**本地、自己的 Github Repository、上游仓库** 三者的交互

```bash
git fetch upstream  # 获取上游仓库的最新变更
git checkout main   # 切换到你的主分支（如 main、master） 
git merge upstream/main  # 合并上游的 main 分支到你的本地分支
git pull upstream main  # 拉取上游 main 分支并自动合并
git push origin main # 推送更新到你的 Fork 仓库
```

### 3. 删除远程关联
```bash
git remote remove upstream
git remote remove origin
```

## git commit

git commit 的作用是上传你对文件的修改，然后 git 会生成一个 **快照 snapshot**，git 记录的不是文件的变化，而是每一个阶段都保存一个仓库快照。

### git commit -m (message)

这个命令用于提交 commit message，你可以添加你对这部分代码修改的描述。

```bash
git commit -m "fix: OOM error"
```

### git commit -amend

有一种常见的撤消发生在你提交得太早，可能忘记添加一些文件，或者你搞砸了你的提交信息。如果你想重做那个提交，做你忘记的额外更改，暂存它们，然后使用 `--amend` 选项再次提交：

```bash
git commit -amend
```

🌰：
```bash
git commit -m 'Initial commit'
git add forgotten_file
git commit --amend
```

## git pull
git pull 用于从远程仓库获取更新，并将这些更新合并到你的本地分支中。它实际上是两个 Git 命令的组合：git fetch 和 git merge。

### git pull 的详细步骤：

1. **git fetch**:
   - git pull 首先执行 git fetch，它从远程仓库（如 origin）获取所有最新的提交和更新，但不对本地分支进行修改。
   - git fetch 会更新你本地的远程分支（例如 origin/main）以匹配远程仓库中的内容。
2. **git merge**:
   - 在 git fetch 完成后，git pull 会执行 git merge，将刚刚获取到的更新合并到你当前的本地分支中。
   - 如果你的本地分支与远程分支之间有冲突，Git 会提示你手动解决冲突，然后继续完成合并。

### 例子：

假设你正在本地的 main 分支上工作，而远程仓库的 main 分支已经有了一些新的提交。

1. **执行 git pull**：
   - Git 首先执行 git fetch，获取远程仓库 main 分支的最新提交，并将其存储在本地的 origin/main 分支中。
   - 接着，Git 将 origin/main 的更新合并到你当前的 main 分支中。
2. **结果**：
   - 你的本地 main 分支现在包含了远程仓库中的最新更改。

### git pull 的选项：

```
git pull --rebase
```
  - 使用 --rebase 选项，git pull 不会直接合并，而是会将你的本地提交应用在获取到的远程更新之上。这样可以保持提交历史更加线性。
 
```
git pull local_branch_name remote_branch-name
```
  - 如果你只想从特定的远程分支拉取更新，你可以指定远程仓库名和分支名。

git pull 是在团队合作中频繁使用的命令，用于保持你的本地代码与远程仓库同步。
在即将 push 的时候一定要先 pull。
## git push
### 推送和拉取分支

推送本地分支到远程：

```bash
git push origin <branch-name>
```

拉取远程分支：

```bash
git pull origin <remote-branch-name>
```

**等价操作：**

```bash
# 以下两种操作等价
git fetch origin + git merge origin main = git pull origin main

# 创建并切换到远程分支的本地副本
git checkout -b <branch-name> origin/<branch-name>
```

### 推送本地新分支到远程新分支

推送本地分支到远程，并设置跟踪关系
```bash
git push -u origin branch-name

# 或者完整写法
git push --set-upstream origin branch-name
```

本地分支名和远程分支名不同
```bash
git push -u origin local-branch-name:remote-branch-name
```

### 强制推送

| 命令                   | 安全性    | 说明              |
| -------------------- | ------ | --------------- |
| `--force-with-lease` | ✅ 更安全  | 检查远程分支是否被其他人更新过 |
| `--force`            | ⚠️ 有风险 | 直接覆盖，可能丢失其他人的提交 |
这种情况最终会得到一个提交 — 第二个提交将替换第一个提交的结果。

## Git 分支的作用与意义

Git 分支（Branch）是版本控制系统的核心概念，用于管理代码开发过程中的不同工作线。分支系统为现代软件开发提供了强大的协作和管理能力。

### 1. 并行开发

分支允许多个开发者在同一项目中并行工作，互不干扰。每个开发者可以在独立的分支上开发新功能、修复 Bug 或进行技术实验，而不会影响主分支的稳定代码。

### 2. 开发阶段隔离

通过不同分支来代表代码开发的不同阶段，实现清晰的工作流管理：

- **主分支（main/master）**：存储生产环境的稳定代码
- **开发分支（develop）**：集成所有开发者的代码，进行整体测试
- **功能分支（feature branches）**：开发新功能或特性
- **修复分支（hotfix branches）**：修复生产环境的紧急问题
- **发布分支（release branches）**：准备发布版本

### 3. 常见工作流模式

#### Git Flow

Git Flow 定义了严格的分支策略和命名约定，适用于大型团队协作开发：

```text
master
|
develop
|\
| feature-1
| feature-2
| hotfix-1
```

#### GitHub Flow

GitHub Flow 是更简洁的分支工作流，适用于持续交付的项目：

```text
master
|\
| feature-1
| feature-2
```

#### 推荐的分支管理策略

[![Github Flow](https://i.postimg.cc/HLq0YYQW/How-To-Use-Git.png)](https://postimg.cc/WD7qSPWx)

### 4. 安全试验与回滚

分支为开发者提供了安全的试验环境。如果试验失败，可以轻松删除分支并回滚到之前的状态，完全不影响其他分支的工作进展。

### 5. 代码审查与协作

通过 Pull Request（合并请求）机制，分支使代码审查变得更加便利。开发者可以在代码合并前通过 PR 请求团队成员进行审查和测试，有效提高代码质量并减少错误。

### 6. 持续集成与部署

在 CI/CD 流程中，分支可以触发自动化测试和部署流程。每当有新代码推送到指定分支时，系统会自动运行测试和部署脚本，确保代码质量和部署稳定性。

## Git 分支的具体使用方法

掌握 Git 分支的创建、切换、合并、删除等操作是高效使用 Git 的基础。

### 1. 创建分支

创建新分支：

```bash
git branch <branch-name>
```

### 2. 切换分支

切换到已有分支：

```bash
git checkout <branch-name>
```

创建并切换到新分支：

```bash
git checkout -b <branch-name>
```

### 3. 查看分支

查看所有本地分支：

```bash
git branch
```

查看远程分支：

```bash
git branch -r
```

查看本地和远程所有分支：

```bash
git branch -a
```

### 4. 合并分支

将指定分支的更改合并到当前分支。例如，将 `feature-xyz` 分支合并到 `master` 分支：

```bash
git checkout master
git merge feature-xyz
```

### 5. 删除分支

**删除本地分支：**

```bash
git branch -d <branch-name>
```

**删除远程分支：**

```bash
git push origin --delete <branch-name>
```

### 6. 解决合并冲突

合并过程中遇到冲突时的处理步骤：

1. **识别冲突文件**：Git 会标记冲突的文件，例如：
    
    ```
    <<<<<<< HEAD
    This is content from the current branch.
    =======
    This is content from the branch being merged.
    >>>>>>> feature-xyz
    ```
    
2. **手动解决冲突**：编辑文件，保留需要的更改，删除冲突标记
    
3. **标记冲突已解决**：
    
    ```bash
    git add <file-name>
    ```
    
4. **完成合并**：
    
    ```bash
    git commit
    ```

### 8. 重命名分支

重命名当前分支：

```bash
git branch -m <new-branch-name>
```

重命名其他分支：

```bash
git branch -m <old-branch-name> <new-branch-name>
```

### 9. 查看分支历史

查看当前分支的提交历史：

```bash
git log
```

查看特定分支的提交历史：

```bash
git log <branch-name>
```

## git tag 打标签

 Git Tags 是一种特殊的指针，指向特定的提交记录。

1. 轻量级标签 (Lightweight Tags) 本质上就是一个指向特定 commit 对象的引用，类似于分支，但不会移动。它们存储在 `.git/refs/tags/` 目录下，每个标签文件包含一个 40 字符的 SHA-1 哈希值，指向目标 commit。

```
.git/refs/tags/v1.0  # 文件内容：abc123def456...（commit 的 SHA-1）
```

2. 附注标签 (Annotated Tags) 是Git 的完整对象，有自己的 SHA-1 标识。它包含：

- 标签名称
- 创建者信息（姓名、邮箱）
- 创建时间
- 标签消息
- 指向的对象（通常是 commit）的 SHA-1

Git 会创建一个 tag 对象存储这些元数据，然后在 `.git/refs/tags/` 中存储指向这个 tag 对象的引用。

### 创建标签

**轻量级标签**（最简单）：

```bash
git tag v1.0
git tag stable
```

**附注标签**（推荐，包含更多信息）：

```bash
git tag -a v1.0 -m "版本 1.0 发布"
git tag -a v2.1.3 -m "修复重要 bug"
```

### 为历史提交打标签

```bash
# 为特定 commit 打标签
git tag -a v1.0 9fceb02 -m "版本 1.0"

# 使用 commit 的部分哈希值
git tag v0.9 a1b2c3d
```

### 查看标签

```bash
# 列出所有标签
git tag

# 按模式过滤标签
git tag -l "v1.*"
git tag --list "v2.0*"

# 查看标签详细信息
git show v1.0

# 查看标签和对应的 commit
git tag -n
git tag -n5  # 显示标签信息的前5行
```

### 推送标签

```bash
# 推送单个标签
git push origin v1.0

# 推送所有标签
git push origin --tags

# 推送所有标签（包括轻量级标签）
git push --follow-tags
```

### 删除标签

```bash
# 删除本地标签
git tag -d v1.0

# 删除远程标签
git push origin --delete v1.0
# 或者
git push origin :refs/tags/v1.0
```

### 检出标签

```bash
# 检出到标签（分离头指针状态）
git checkout v1.0

# 基于标签创建新分支
git checkout -b hotfix-v1.0 v1.0
```

### 实用技巧

```bash
# 查看当前 commit 的所有标签
git tag --points-at HEAD

# 查看标签之间的差异
git diff v1.0 v1.1

# 验证带签名的标签
git tag -v v1.0

# 创建带 GPG 签名的标签
git tag -s v1.0 -m "签名标签"
```

## RollBack 回退代码

Git 日志 (`git log`) 记录了仓库的完整提交历史。通过这些记录，你可以恢复到特定提交或撤销某些更改。
### git log
#### 1. 查看提交历史

使用 `git log` 查看提交历史，获取目标提交的哈希值：

```bash
git log [--oneline] # 一条记录输出一行
```

**输出示例：**

```plaintext
commit abcdef1234567890abcdef1234567890abcdef12
Author: Your Name <your.email@example.com>
Date:   Tue Jun 28 12:34:56 2023 +0000

    Commit message

commit 1234567890abcdef1234567890abcdef12345678
Author: Another Author <another.email@example.com>
Date:   Mon Jun 27 11:22:33 2023 +0000

    Another commit message
```

### git checkout

临时查看

适用于临时查看某个提交的状态：

```bash
git checkout <commit-hash>
```

例如：

```bash
git checkout abcdef1234567890abcdef1234567890abcdef12
```

返回到最新提交：

```bash
git checkout master
```

### git reset

用于永久回退到某个提交：

- **软重置**：保留工作区和暂存区的更改

    ```bash
    git reset --soft <commit-hash>
    ```

- **混合重置**（默认）：保留工作区更改，清除暂存区

    ```bash
    git reset --mixed <commit-hash>
    ```

- **硬重置**：丢弃工作区和暂存区的所有更改

 ```bash
    git reset --hard <commit-hash>
    ```

### git revert

创建新提交来撤销指定提交，不改变提交历史：

```bash
git revert <commit-hash>
```

### git restore
想要删除未加入暂存区的文件（没有 git add file ）

```bash
git restore .
git restore file
```
### git rebase

删除最近的 n 个 commit，但是不回退你修改过的文件，仅删除 n 个 commit 最后文件的内容和最后一次 commit 内容一致。

- `git reset --soft` 是一个非常安全的操作，因为它不会丢失代码，只是将提交“撤回”，但保留改动在暂存区。
```bash
# 两个命令功能相同
git rebase -i HEAD~n

git reset --soft HEAD~n
```

### git clean

Git 删除未跟踪的文件有以下几种方法：

#### 删除未跟踪的文件

```bash
# 删除所有未跟踪的文件
git clean -f

# 删除未跟踪的文件和目录
git clean -fd
```

#### 先预览再删除（推荐）

```bash
# 预览将要删除的文件
git clean -n

# 预览将要删除的文件和目录
git clean -nd

# 确认后再删除
git clean -f
```

#### 交互式删除

```bash
# 交互式选择要删除的文件
git clean -i
```

#### 删除特定类型的文件

```bash
# 删除所有 .log 文件
git clean -f "*.log"

# 删除特定目录下的未跟踪文件
git clean -f path/to/directory/
```

#### 常用选项说明：

- `-f` (force): 强制删除
- `-d`: 删除目录
- `-n` (dry-run): 预览模式，不实际删除
- `-i`: 交互式模式
- `-x`: 删除被 `.gitignore` 忽略的文件
- `-X`: 只删除被 `.gitignore` 忽略的文件

#### 安全做法：

```bash
# 1. 先查看未跟踪的文件
git status

# 2. 预览要删除的文件
git clean -n

# 3. 确认后删除
git clean -f
```

#### 注意事项：

- `git clean` 是**不可逆**的操作，删除的文件无法恢复
- 建议先使用 `-n` 参数预览
- 默认不会删除 `.gitignore` 中的文件，除非使用 `-x` 参数

### 重要注意事项

- `git reset --hard` 会丢失所有未提交的更改，请谨慎使用
- 在共享分支上进行 `git reset` 操作前，务必与团队沟通
- `git revert` 是更安全的撤销方式，因为它保持提交历史完整性

## 重新绑定远程仓库 URL

当需要更改远程仓库地址时：

```bash
git remote set-url origin <new-url>
git remote -v  # 验证更改
```

## 如何创建 Pull Request

Pull Request (PR) 是向上游仓库提交代码更改的标准方法，特别适用于没有直接写权限的情况。

### 完整操作流程

#### 1. Fork 目标仓库

在目标仓库页面点击右上角的 **"Fork"** 按钮，将仓库副本复制到你的 GitHub 账户。

#### 2. 克隆你的 Fork 仓库

```bash
git clone https://github.com/your-username/forked-repository.git
cd forked-repository
```

#### 3. 创建功能分支

**这里建议将每一个新功能分为不同的分支进行工作，每个分支都从主仓库中 `git checkout -b new-branch` 而来，然后每一个新功能用一个新的分支来开发。**

在进行更改前创建新分支：

```bash
git checkout -b my-feature-branch
```

#### 4. 进行更改并提交

完成开发工作后提交更改：

```bash
git add .
git commit -m "详细描述你的更改内容"
```

#### 5. 推送到你的 Fork

```bash
git push origin my-feature-branch
```

#### 6. 创建 Pull Request

1. 访问你的 Fork 仓库页面
2. 点击 **"Compare & pull request"** 按钮
3. 填写清晰的标题和详细的描述说明
4. 选择目标仓库和分支（通常是 `main` 或 `master`）
5. 点击 **"Create pull request"** 提交

#### 7. 等待审查

提交后，上游仓库的维护者会审查你的更改，可能会：

- 提供反馈和修改建议
- 直接合并你的代码
- 请求进一步的修改

### 成功创建 PR 的关键要素

- **清晰的标题**：简洁地概括更改内容
- **详细的描述**：说明更改的原因、影响和测试情况
- **单一职责**：每个 PR 专注于一个功能或修复
- **代码质量**：确保代码符合项目的编码规范

通过遵循这些步骤和最佳实践，你可以高效地参与开源项目或团队协作开发。