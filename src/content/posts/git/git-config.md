---
title: Git 配置文件
published: 2025-04-21
tags:
  - git
toc: false
lang: zh
---

- `.gitignore`: 忽略文件
- `.gitattributes`: 用于指定特定文件或路径的属性和行为
    - 设置换行符：
        
        ```
        *.txt eol=lf     # 将所有 .txt 文件的换行符设置为 LF (Unix 风格)
        *.md eol=crlf    # 将所有 .md 文件的换行符设置为 CRLF (Windows 风格)
        ```
        
    - 设置文件类型：
        
        ```
        *.png binary    # 将所有 .png 文件标记为二进制文件
        *.txt text      # 将所有 .txt 文件标记为文本文件
        ```
        
    - 设置合并策略：
        
        ```
        *.txt merge=union    # 使用 union 合并策略合并 .txt 文件
        *.md merge=ours      # 使用 ours 合并策略合并 .md 文件
        ```
        
    - 设置文件编码：
        
        ```
        *.html charset=utf-8    # 将所有 .html 文件的编码设置为 UTF-8
        *.css charset=utf-8     # 将所有 .css 文件的编码设置为 UTF-8
        ```
        
- `.gitkeep`：这是一个约定的文件名，用于在空文件夹中保留该文件夹的版本控制。Git 默认不会跟踪空文件夹，但如果您想在版本库中保留空文件夹，可以在其中添加一个名为 `.gitkeep` 的空文件。

完。
