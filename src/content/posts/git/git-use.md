---
title: Git 使用
published: 2025-04-21
tags:
  - git
toc: false
lang: zh
---

## 设置项目内的user.name和user.email

```powershell
git config --local user.name "xxxxxx"git config --local user.email "xxx@xxxx.xxx"
```


## .gitignore

`.gitignore` 是一个用于指定Git版本控制系统忽略哪些文件和目录的配置文件。当你在项目中创建了.gitignore文件并定义了忽略规则后，Git会自动忽略这些文件和目录的变更。

`.gitignore` 文件的语法很简单，每一行表示一个忽略规则。你可以使用以下几种模式来指定要忽略的文件或目录：

1. 文件名匹配：
    - filename：忽略特定文件或目录，如myfile.txt；
    - .txt：通配符模式，忽略所有以”.txt”结尾的文件；
    - folder/：忽略特定目录，如myfolder/；
2. 路径匹配：
    - /path/to/file：从根目录开始的完整路径，忽略特定文件或目录；
    - path/：相对于当前位置的路径，忽略特定目录及其内容；
3. 注释：
    - 使用#作为注释标识，可以在文件中添加注释说明。


## .gitignore规则不生效

`.gitignore` 只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改 `.gitignore` 是无效的。

解决方法就是先把本地缓存删除（改变成未 `track` 状态），然后再提交

```powershell
git rm -r --cached .git add .git commit -m 'chore: update .gitignore'
```

可以使用 `git` 提供的 `Git Bash` 键入一下命令创建 `.gitignore` 文件

```bash
touch .gitignore 创建忽略文件
open .gitignore 打开忽略文件
```

## 克隆仓库又创建了一个新的文件夹怎么办？

有些时候在克隆代码仓库之前会创建一个新的空文件夹以便于管理项目，但克隆代码仓库会新创建一个文件夹。

```bash
new-product
└── code-repository-name
```

这样的话，可以将 `code-repository-name` 文件夹内所有文件都向上拷贝到 `new-product` 文件夹下，删除 `code-repository-name` 即可。

## 仓库迁移

下面的指令可以把已经在的团队的代码仓库提交的代码连着提交历史一起提交到新的代码仓库中，备份好代码后可以使用这个命令把项目从别的团队转到新的团队 ，但是项目协同相关的信息不会一起转移

```bash
git push --mirror 仓库地址
```

## 强制拉取线上代码覆盖本地文件

1. 需要将这些更新取回本地，这时就要用到git fetch命令
    
    ```bash
    git fetch --all
    ```
    
2. 撤销本地、暂存区、版本库（用远程服务器的origin/master替换本地）
    
    ```bash
    git reset --hard origin/master
    ```
    
3. git pull 来从远程仓库拉取同步代码
    
    ```bash
    git pull origin master
    ```

完。
