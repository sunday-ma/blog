---
title: npm,yarn,pnpm
published: 2025-04-21
tags:
  - npm
  - yarn
  - pnpm
toc: false
lang: zh
---

### 命令对比

| **npm** | **yarn** | **pnpm** |
| --- | --- | --- |
| npm install | yarn | pnpm install / pnpm i |
| npm install react –save | yarn add react | pnpm add react |
| npm uninstall react –save | yarn remove react | pnpm remove react / pnpm rm react |
| npm install react –save-dev | yarn add react –dev | pnpm add react -D |
| npm update –save | yarn upgrade | pnpm update |

### 查看全局安装过的包

```bash
npm list -g --depth 0
pnpm list -g --depth 0
```

### 查看正在使用的软件源

```bash
npm config get registry
npm config list
pnpm config get registry
pnpm config list
```

### 设置软件源

```bash
# 官方源npm config set registry https://registry.npmjs.org/
yarn config set registry https://registry.npmjs.org/
pnpm config set registry https://registry.npmjs.org/
# 阿里镜像源pnpm config set registry https://registry.npmmirror.com/
```

在项目中创建`.npmrc`文件配置软件源

### .npmrc的作用

`.npmrc`，可以理解成`npm running cnfiguration`, 即`npm`运行时配置文件。

> npm按照如下顺序读取这些配置文件：
> 
1. 项目配置文件：你可以在项目的根目录下创建一个`.npmrc`文件，只用于管理这个项目的`npm`安装。
2. 用户配置文件：在你使用一个账号登陆的电脑的时候，可以为当前用户创建一个`.npmrc`文件，之后用该用户登录电脑，就可以使用该配置文件。可以通过 `npm config get userconfig` 来获取该文件的位置。
3. 全局配置文件： 一台电脑可能有多个用户，在这些用户之上，你可以设置一个公共的`.npmrc`文件，供所有用户使用。该文件的路径为：`$PREFIX/etc/npmrc`，使用 `npm config get prefix` 获取`$PREFIX`。如果你不曾配置过全局文件，该文件不存在。
4. npm内嵌配置文件：最后还有npm内置配置文件，基本上用不到，不用过度关注。

> 如何设置.npmrc
> 
1. 设置项目配置文件
    
    在项目的根目录下新建 `.npmrc` 文件，在里面以 `key=value` 的格式进行配置。比如要把npm的源配置为淘宝源，可以参考以下代码：
    
    ```bash
    registry=https://registry.npmmirror.com/
    ```
    
2. 设置用户配置文件
    
    你可以直接通过 `npm config get userconfig` 命令找到该文件的路径，然后直接仿照上述方法该文件，也可以通过 `npm config set` 命令继续设置，命令如下：
    
    ```bash
    npm config set registry https://registry.npmmirror.com/
    ```
    
3. 设置全局配置文件
    
    方法和设置用户配置文件如出一辙，只不过在使用命令行时需要加上 `-g` 参数。
    
    ```bash
    npm config set registry https://registry.npmmirror.com/ -g
    ```
    

### 解决因为node或者npm版本过高导致依赖安装不上的问题

> 建议使用 nvm 切换 nodejs 版本
> 

```bash
npx -p npm@6 npm i --legacy-peer-depsnpx -p npm@7 npm i --legacy-peer-deps
```

### npm和pnpm执行npm包中的可执行文件工具

```tsx
# npm
npx eslint --init
# pnpm
pnpm dlx eslint --init
```

完。
