---
layout: article
tags: git github
title: "Git&Github相关指令"
---

## 修改git用户

```bash
git config --global user.email "MonkiFantasy@outlook.com"
git config --global user.name "MonkiFantasy"
```

## 个人向查看git log方式

```bash
 git log --all --graph --decorate --oneline
```

## git checkout 用法

1. 根据哈希码查看详细信息

```bash
git checkout cc30c4b
```

2. 切换分支

```bash
git checkout master
```

## github 解决push相关问题（玄学）

```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```

```cmd
ipconfig/flushdns
```

## github添加远程仓库

```bash
git remote add 仓库名 仓库地址
```

## github修改远程仓库地址

```bash
git remote set-url 仓库名 新仓库地址
```

## git pull&git fetch

`git pull` 和 `git fetch` 都是用于从远程仓库获取更新的 Git 命令，但它们在用法和行为上有一些区别。

1. **git fetch：**

   - `git fetch` 命令用于从远程仓库获取更新，但并不会自动将这些更新合并到当前分支。
   - 运行 `git fetch` 会下载远程仓库中的所有分支和提交，然后将这些信息存储在本地，但你的当前分支不会受到任何影响。
   - 这个命令通常用于查看远程仓库的更新情况，以便在合适的时候决定是否要进行合并操作。

   用法示例：

   ```bash
   git fetch origin
   ```

2. **git pull：**

   - `git pull` 命令是 `git fetch` 和合并操作（`git merge`）的结合。
   - 运行 `git pull` 会首先运行 `git fetch`，从远程仓库获取更新，然后将这些更新自动合并到当前分支。
   - `git pull` 的默认行为是合并远程仓库的对应分支的更新到当前分支。

   用法示例：

   ```bash
   git pull origin master
   ```

区别总结：

- `git fetch` 只是获取远程仓库的更新，不会自动合并到当前分支。
- `git pull` 获取远程仓库的更新，并自动将这些更新合并到当前分支。

使用哪个命令取决于你的需求。如果你想查看远程仓库的更新情况而不想立即合并，可以使用 `git fetch`。如果你想获取远程仓库的更新并立即合并到当前分支，可以使用 `git pull`。在使用 `git pull` 时，可以通过添加参数指定要合并的分支。

## 生成ssh密钥

```bash
ssh-keygen -t rsa -C +邮箱
```

生成密钥后，复制~/.ssh路径下**id_rsa.pub**秘钥文件添加到github-->settings-->SSH 和 GPG 密钥-->添加 SSH 密钥
