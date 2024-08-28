---
layout: post
title: How to use Git
date: 2024-07-04
tags: Git
---

## 本地仓库上传到远端仓库（repositories）

> 将新建的本地仓库上传到 GitHub 涉及几个步骤，包括在 GitHub 上创建新的空仓库，然后将本地仓库推送到这个新创建的远程仓库。以下是详细的步骤：

### 步骤1: 创建本地仓库

1. 打开终端。
2. 导航到你想要创建新仓库的目录：

    ```bash
    cd path/to/your/project
    ```

3. 初始化一个新的 Git 仓库：

    ```bash
    git init
    ```

4. 添加文件到仓库：

    ```bash
    git add .
    ```

5. 提交文件：

    ```bash
    git commit -m "Commit information"
    ```

### 步骤 2: 在 GitHub 上创建新仓库

1. 打开浏览器并登录到 GitHub。
2. 点击右上角的 `+` 按钮，然后选择 `New repository`。
3. 填写仓库名称和描述，选择是否公开（Public）或私有（Private），然后点击 `Create repository`。

### 步骤 3: 将本地仓库连接到 GitHub 仓库

1. 在 GitHub 新仓库页面，你会看到一些提示，包括如何将本地仓库连接到这个新仓库。
2. 在终端中，执行以下命令，将本地仓库连接到 GitHub 上的新仓库：

    ```bash
    git remote add origin https://github.com/luliqwerty/your-repo.git
    ```

    记得将 `yourusername` 替换为你的 GitHub 用户名，将 `your-repo` 替换为你新创建的仓库名称。

### 步骤 4: 推送本地仓库到 GitHub

1. 推送本地仓库的内容到 GitHub 仓库：

    ```bash
    git push -u origin master
    ```

    如果你使用的是其他分支而不是 `master`，将 `master` 替换为你的分支名称。

## 查看本地仓库关联的远端仓库

> 如果你已经克隆了仓库到本地，可以使用 Git 命令查看文件所属的仓库信息。

1. **打开终端**：
   
- 导航到包含你感兴趣文件的本地仓库目录。
  
2. **查看当前仓库的远程地址**：
    - 使用以下命令查看当前仓库的远程 URL：

    ```bash
    git remote -v
    ```

    输出示例：

    ```
    origin  https://github.com/yourusername/your-repo.git (fetch)
    origin  https://github.com/yourusername/your-repo.git (push)
    ```

    这会显示当前仓库的远程 URL，告诉你这个本地仓库关联的远程仓库。

3. **查看当前分支**：
    - 使用以下命令查看当前所在的分支：

    ```bash
    git branch
    ```

    当前分支会有一个星号 `*` 标识。

4. **查看文件的提交历史**：
    - 使用以下命令查看特定文件的提交历史，帮助你了解文件的修改和提交记录：

    ```bash
    git log -- path/to/your/file
    ```

    这会显示指定文件的提交记录，包括每次修改的提交信息和提交者。

## Git branch 的作用

>  Git 分支（Branch）是版本控制系统中的一个核心概念，用于管理代码开发过程中的不同工作线。分支的主要作用包括以下几个方面：

### 1. 并行开发

分支允许多个开发者在同一项目中同时工作而不会干扰彼此。每个开发者可以在自己的分支上开发新功能、修复Bug或者进行实验，而不影响主分支上的稳定代码。

### 2. 隔离不同开发阶段

不同的分支可以代表代码开发的不同阶段，例如开发、测试和生产。常见的分支策略包括：

- **主分支（master 或 main）**：通常用于存储生产环境的稳定代码。
- **开发分支（develop）**：用于集成所有开发者的代码，进行整体测试。
- **功能分支（feature branches）**：用于开发新功能或特性。
- **修复分支（hotfix branches）**：用于修复生产环境中的紧急问题。
- **发布分支（release branches）**：用于准备发布的版本。

- **常见工作流:**

  - Git Flow

  Git Flow 是一种常见的分支管理工作流，它定义了一套严格的分支策略和命名约定，以便于大型团队协作开发。

  ```text
  master
  |
  develop
  |\
  | feature-1
  | feature-2
  | hotfix-1
  ```

  - GitHub Flow

  GitHub Flow 是一种更为简洁的分支工作流，适用于持续交付的项目。

  ```text
  master
  |\
  | feature-1
  | feature-2
  ```



### **This is a good strategy to organize git branches:**

![Github Flow](/images/postImgs/HowToUseGit.png)

### 3. 试验和回滚

分支允许开发者在不影响主代码库的情况下进行试验。如果试验失败，可以轻松地删除分支并回滚到之前的状态，而不影响其他分支上的工作。

### 4. 代码审查和协作

通过 Pull Request（合并请求），分支使得代码审查变得更加容易。开发者可以在提交代码之前通过 Pull Request 请求其他团队成员审查和测试代码，这有助于提高代码质量和减少错误。

### 5. 持续集成和部署

在持续集成（CI）和持续部署（CD）流程中，分支可以用来自动化测试和部署。每当有新的代码推送到某个分支时，CI/CD 系统可以自动运行测试和部署脚本，确保新代码不会引入错误并能顺利部署。

总结起来，Git 分支是项目开发过程中管理并行工作、隔离开发阶段、进行试验和回滚、进行代码审查和协作以及实现持续集成和部署的强大工具。通过合理使用分支，可以显著提高开发效率和代码质量。

## Git branch 的使用

使用 Git 分支的方法包括创建、切换、合并、删除分支以及处理冲突等操作。以下是一些详细的步骤和命令，帮助你有效地使用 Git 分支。

### 1. 创建分支

要创建一个新的分支，可以使用以下命令：

```bash
git branch <branch-name>
```

### 2. 切换分支

要切换到已有的分支，可以使用以下命令：

```bash
git checkout <branch-name>
```

或者直接创建并切换到一个新的分支：

```bash
git checkout -b <branch-name>
```

### 3. 查看分支

要查看所有分支，可以使用以下命令：

```bash
git branch
```

要查看远程分支：

```bash
git branch -r
```

要查看本地和远程所有分支：

```bash
git branch -a
```

### 4. 合并分支

将一个分支的更改合并到当前分支。例如，将 `feature-xyz` 分支合并到当前分支（如 `master`）：

```bash
git checkout master
git merge feature-xyz
```

### 5. 删除分支

删除本地分支：

```bash
git branch -d <branch-name>
```

删除远程分支：

```bash
git push origin --delete <branch-name>
```

### 6. 解决合并冲突

在合并过程中，可能会遇到冲突。Git 会标记冲突的文件，你需要手动解决这些冲突。

1. 打开冲突文件，Git 会标记冲突部分，例如：

    ```
    <<<<<<< HEAD
    This is content from the current branch.
    =======
    This is content from the branch being merged.
    >>>>>>> feature-xyz
    ```

2. 编辑文件，保留需要的更改，删除冲突标记。

3. 添加解决冲突的文件：

    ```bash
    git add <file-name>
    ```

4. 完成合并：

    ```bash
    git commit
    ```

### 7. 推送和拉取分支

推送本地分支到远程：

```bash
git push origin <branch-name>
```

拉取远程分支：

```bash
git fetch origin
git checkout -b <branch-name> origin/<branch-name>
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

## Git 恢复 log 中的代码

在 Git 中，日志 (`git log`) 记录了仓库的所有提交历史。你可以使用这些日志信息来恢复到特定的提交或撤销某些提交。以下是一些常见的操作方法：

### 1. 查看日志

首先，使用 `git log` 查看提交历史，找到你想恢复的提交的哈希值（commit hash）。

```bash
git log
```

输出示例：

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

### 2. 恢复到特定提交

#### 方法一：使用 `git checkout`

这种方法适用于临时查看某个提交的状态。

```bash
git checkout <commit-hash>
```

例如：

```bash
git checkout abcdef1234567890abcdef1234567890abcdef12
```

要返回到最新的提交（HEAD），使用：

```bash
git checkout master
```

#### 方法二：使用 `git reset`

这种方法用于永久回退到某个提交，并丢弃之后的所有提交。

- **软重置**：保留工作区和暂存区的更改。

    ```bash
    git reset --soft <commit-hash>
    ```

    例如：

    ```bash
    git reset --soft abcdef1234567890abcdef1234567890abcdef12
    ```

- **混合重置**（默认）：保留工作区的更改，但清除暂存区。

    ```bash
    git reset --mixed <commit-hash>
    ```

    例如：

    ```bash
    git reset --mixed abcdef1234567890abcdef1234567890abcdef12
    ```

- **硬重置**：丢弃工作区和暂存区的所有更改。

    ```bash
    git reset --hard <commit-hash>
    ```

    例如：

    ```bash
    git reset --hard abcdef1234567890abcdef1234567890abcdef12
    ```

### 3. 撤销特定提交

#### 使用 `git revert`

这种方法用于创建一个新的提交来撤销指定的提交，而不会改变提交历史。

```bash
git revert <commit-hash>
```

例如：

```bash
git revert abcdef1234567890abcdef1234567890abcdef12
```

### 注意事项

- 使用 `git reset --hard` 会丢失工作区和暂存区的所有更改，务必谨慎。
- 在共享分支（如 `master` 或 `develop`）上进行 `git reset` 操作时，可能会影响其他协作者的工作，建议先与团队沟通。
- `git revert` 是一种更安全的撤销方式，因为它不会改变提交历史，只会在提交历史上增加一个新的撤销提交。

通过这些步骤和命令，你可以根据 `git log` 恢复到特定的提交，或撤销某些提交，以便于管理代码版本和回滚更改。

## git push 正常但 Github 不刷新的问题

### 因为我是菜鸡所以我还没有解决 sorry ~ ！(我3个G的电子书想传上去但是一直不行)

## 重新绑定远程的 URL

```bash
git remote set-url origin <url>
git remote -v
```

在 Git 中创建 Pull Request (PR) 是一种提交代码更改到上游仓库的标准方法，尤其是当你没有直接的写权限时。以下是一个详细的步骤，教你如何通过 GitHub 创建 Pull Request：

## how to pull request

### 1. Fork 仓库

在你想贡献代码的仓库页面，点击右上角的 **“Fork”** 按钮。这会将该仓库的一个副本复制到你的 GitHub 账户下。

### 2. 克隆你的 Fork 仓库

将你 Fork 的仓库克隆到本地机器：

```bash
git clone https://github.com/your-username/forked-repository.git
cd forked-repository
```

### 3. 创建一个新分支

在进行更改之前，创建一个新的分支来进行你的开发工作：

```bash
git checkout -b my-feature-branch
```

### 4. 进行更改并提交

在新分支上进行你的更改。完成后，提交你的更改：

```bash
git add .
git commit -m "描述你的更改内容"
```

### 5. 推送更改到你的 Fork

将你的更改推送到你 Fork 的仓库：

```bash
git push origin my-feature-branch
```

### 6. 在 GitHub 上创建 Pull Request

1. 访问你的 Fork 仓库页面（例如 `https://github.com/your-username/forked-repository`）。
2. 你会看到一个关于你刚刚推送的分支的通知，点击 **“Compare & pull request”** 按钮。
3. 填写 Pull Request 的标题和描述，确保详细说明你的更改。
4. 选择要合并到的上游仓库和分支（通常是 `main` 或 `master`）。
5. 点击 **“Create pull request”** 按钮。

### 7. 等待审查和反馈

你的 Pull Request 现在已提交。上游仓库的维护者会审查你的更改，并可能提供反馈或直接合并你的代码。通过这些步骤，你可以成功地在 GitHub 上创建一个 Pull Request，将你的代码更改提交到上游仓库。记住，要仔细填写 PR 的标题和描述，以便审查者能够理解你的更改内容。
