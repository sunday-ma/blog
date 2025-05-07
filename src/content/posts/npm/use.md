---
title: 使用 nvm 管理不同版本的 node 与 npm
published: 2025-04-21
tags:
  - nvm
  - npm
  - node
toc: false
lang: zh
---

在我们的日常开发中经常会遇到这种情况：手上有好几个项目，每个项目的需求不同，进而不同项目必须依赖不同版的 NodeJS 运行环境。如果没有一个合适的工具，这个问题将非常棘手。

[nvm](https://github.com/creationix/nvm) 应运而生，nvm 是 Mac 下的 node 管理工具，有点类似管理 Ruby 的 rvm，如果需要管理 Windows 下的 node，官方推荐使用 [nvmw](https://github.com/hakobera/nvmw) 或 [nvm-windows](https://github.com/coreybutler/nvm-windows)。不过，nvm-windows 并不是 nvm 的简单移植，他们也没有任何关系。但下面介绍的所有命令，都可以在 nvm-windows 中运行。

---

### nvm 与 n 的区别

node 版本管理工具还有一个是 TJ大神的 [n](https://github.com/tj/n) 命令，n 命令是作为一个 node 的模块而存在，而 nvm 是一个独立于 node/npm 的外部 shell 脚本，因此 n 命令相比 nvm 更加局限。

由于 npm 安装的模块路径均为 **`/usr/local/lib/node_modules`**，当使用 n 切换不同的 node 版本时，实际上会共用全局的 node/npm 目录。 因此不能很好的满足『按不同 node 版本使用不同全局 node 模块』的需求。

---

### 使用 nvm 之前的工作

> 如果是新电脑，忽略；
> 
1. 完全卸载已经安装的 NodeJS，否则会发生冲突；
2. 删除系统环境变量中有关 NodeJs 的路径配置；

---

### Window 安装

下载 [nvm-windows](https://github.com/coreybutler/nvm-windows/releases) 最新安装包，直接安装即可

---

### 安装多版本 node/npm

1. 首先，运行 `nvm --help`，查看支持的命令
    
    ```bash
    PS C:\Users\username> nvm --helpRunning version 1.1.11.
    Usage:  nvm arch                     : Show if node is running in 32 or 64 bit mode.
      nvm current                  : Display active version.
      nvm debug                    : Check the NVM4W process for known problems (troubleshooter).  nvm install <version> [arch] : The version can be a specific version, "latest" for the latest current version, or "lts" for the
                                     most recent LTS version. Optionally specify whether to install the 32 or 64 bit version (defaults                                 to system arch). Set [arch] to "all" to install 32 AND 64 bit versions.
                                     Add --insecure to the end of this command to bypass SSL validation of the remote download server.
      nvm list [available]         : List the node.js installations. Type "available" at the end to see what can be installed. Aliased as ls.
      nvm on                       : Enable node.js version management.
      nvm off                      : Disable node.js version management.
      nvm proxy [url]              : Set a proxy to use for downloads. Leave [url] blank to see the current proxy.
                                     Set [url] to "none" to remove the proxy.
      nvm node_mirror [url]        : Set the node mirror. Defaults to https://nodejs.org/dist/. Leave [url] blank to use default url.
      nvm npm_mirror [url]         : Set the npm mirror. Defaults to https://github.com/npm/cli/archive/. Leave [url] blank to default url.
      nvm uninstall <version>      : The version must be a specific version.
      nvm use [version] [arch]     : Switch to use the specified version. Optionally use "latest", "lts", or "newest".
                                     "newest" is the latest installed version. Optionally specify 32/64bit architecture.
                                     nvm use <arch> will continue using the selected version, but switch to 32/64 bit mode.
      nvm root [path]              : Set the directory where nvm should store different versions of node.js.
                                     If <path> is not set, the current root will be displayed.
      nvm [--]version              : Displays the current running version of nvm for Windows. Aliased as v.
    ```
    
2. 安装16.6.0版本，可以使用以下命令
    
    ```bash
    nvm install 16.6.0
    ```
    
    nvm 遵守[语义化版本](http://semver.org/lang/zh-CN/)命名规则。例如，你想安装最新的 **`16.6`** 系列的最新的一个版本的话，可以运行：
    
    ```bash
    nvm install 16.2
    ```
    
    nvm 会寻找 `16.6.x` 中最高的版本来安装。
    
    你可以通过以下命令来列出远程服务器上所有的可用版本：
    
    ```bash
    nvm ls available
    ```
    

### 在不同版本间切换

每当我们安装了一个新版本 Node 后，全局环境会自动把这个新版本设置为默认。

nvm 提供了 `nvm use` 命令。这个命令的使用方法和 `install` 命令类似。

例如，切换到 `16.6.2`：

```bash
nvm use 16.6.2
```

切换到最新的 `16.6.x`：

```bash
nvm use 16.6
```

### 列出已安装实例

```bash
nvm ls
```

当前正在使用的版本会被标注成 `Currently using 64-bit executable`

### 在项目中使用不同版本的 Node

我们可以通过创建项目目录中的 `.nvmrc` 文件来指定要使用的 Node 版本。之后在项目目录中执行 `nvm use` 即可。`.nvmrc` 文件内容只需要遵守上文提到的语义化版本规则即可。

`.nvmrc` 文件内容如下：

```bash
v14.21.3
```

完。
