---
title: Git 多公钥配置
published: 2025-04-21
tags:
  - git
toc: false
lang: zh
---

> 解决一个公钥无法共用问题

1. 生成第一个密钥
    
    ```bash
    ssh-keygen -t rsa -C "emailname@email.com"
    ```
    
2. 生成第二个公钥
    
    `two_git` 是第二个公钥的名称
    
    ```bash
    ssh-keygen -t rsa -C  "emailname@email.com" -f two_git
    ```
    
3. 在 `.ssh` 目录新建 `config` 文件
    
    `windows`下的 `.ssh` 目录 在 `C:\Users\username`
    
4. `config` 文件配置
    
    ```bash
    # 账号一Host gitee.com
    HostName gitee.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa
    HostkeyAlgorithms +ssh-rsa
    PubkeyAcceptedAlgorithms +ssh-rsa
    # 账号二# giteeHost two.gitee.com
    HostName gitee.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/two_git
    HostkeyAlgorithms +ssh-rsa
    PubkeyAcceptedAlgorithms +ssh-rsa
    ```
    
5. 使用 `ssh` 的方式克隆仓库
    1. 使用账号一克隆代码
        
        ```bash
        git clone git@gitee.com:xxxxxxx
        ```
        
    2. 使用账号二克隆代码
        
        ```bash
        git clone git@two.gitee.com:xxxxxxx
        ```

完。
