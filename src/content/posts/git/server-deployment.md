---
title: 服务器部署 Git 代码
published: 2025-04-21
tags:
  - git
toc: false
lang: zh
---

## 服务器部分

1. 随便找个文件夹创建空`git`仓库
    
    ```bash
    # 前往 /opt 文件创建 Bare 仓库cd /opt
    # test-git 可以换成任何文件名# Bare 仓库即是没有工作路径的 Git 仓库, 可以从本地运行 git push 将代码推送到当前的 Bare 仓库中git init test-git.git --bare
    ```
    
2. 进入文件夹中的 hooks 文件夹创建`post-receive`文件，设置`hook`，这样每当仓库收到新的代码推送时，就可以运行一些动作
    
    ```bash
    cd test-git.git/hooks/
    vim post-receive
    ```
    
3. 修改`post-receive`文件并保存
    
    ```bash
    # 首先定义这个文档使用 bash 执行
    # !/bin/bash
    # 定义终端输出文案（随便定义）echo 'server: received code push...'
    # 切换到服务目录cd /www/wwwroot
    # 定义终端输出文案（随便定义）# 准备从 Bare 仓库 将代码 checkout 到服务目录echo 'server: checkout latest code from git...'
    # 执行从 Bare 仓库 将代码 checkout master 分支到服务目录
    # checkout 的分支可以自行定义# GIT_DIR 是 .git 目录的位置。 如果这个没有设置， Git 会按照目录树逐层向上查找 .git 目录，直到到达 ~ 或 /。
    # GIT_WORK_TREE 是非空版本库的工作目录的根路径。 如果指定了 --git-dir 或 GIT_DIR 但未指定 --work-tree、GIT_WORK_TREE 或 core.worktree，那么当前工作目录就会视作工作树的顶级目录。git --git-dir=/opt/test-git.git --work-tree=/www/wwwroot/test checkout master -f
    # 如果是 Node 项目，可以在后续添加以下命令（或其他项目需要的编译命令）
    # 定义终端输出文案（随便定义）echo 'server: running npm install...'
    # 执行 npm install, npm run build# 当中的 && 符号代表紧接着要执行的命令# 句末的反斜杠是连接下一行的意思npm install \&& echo 'server: building...' \&& npm run build \&& echo 'server: done.'
    ```
    
4. 赋予`post-receive`执行权限
    
    ```bash
    chmod +x post-receive
    ```

## 本地部分

服务器部分处理完就可以回到本地代码操作了

1. 使用`vscode`等编辑器打开带有`git`仓库的代码文件夹
2. 运行 `git remote add prod`
    
    ```bash
    # prod 是 production 的缩写
    # root 是服务器的登录名称，后期可以创建一个低权限的账号进行操作
    # 然后是服务器的IP，以及Git Bare 仓库的路径git remote add prod ssh://root@IP地址/opt/test-git.git
    # 执行 git remote -v 查看新增的 prod 是否正确# remote 是远端代码托管平台的地址git remote -v# 执行 git push prod master 将代码推送到服务器# 就可以看到在 hooks/post-receive 中以'server: ' 开头的讯息，就是定义的post-receive hook 所执行的指令git push prod master
    ```

## 补充部分

- 新建用户
    
    ```
    sudo adduser newuser
    ```
    
- 赋予用户权限
    
    > 以当前文档为例
    
    ```
    sudo chown -R newuser /opt/test-git.git
    sudo chown -R newuser /www/wwwroot/test
    ```

完。
