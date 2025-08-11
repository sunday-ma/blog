---
title: 输入法推荐：RIME + 雾凇拼音
published: 2025-05-13
tags:
  - 小狼毫
  - RIME
toc: false
lang: zh
---

## 介绍

  1. [RIME／中州韵输入法引擎](https://rime.im)，是一个跨平台的输入法算法框架。

      基于这一框架，Rime 开发者与其他开源社区的参与者在 Windows、macOS、Linux、Android 等平台上创造了不同的输入法前端实现。

  2. [雾凇拼音](https://github.com/iDvel/rime-ice)则是RIME的一个长期维护的简体开源词库。亮点丰富，功能强大，维护在线。

## 安装

  1. Windows 平台下的安装、更新

      前往[RIME官方网站](https://rime.im/download/)，下载小狼毫（Weasel）安装包；

      下载后，运行安装程序，按提示安装即可。期间，安装程序会要求你指定用户文件夹，该文件夹用于放置 RIME 的用户配置文件，通常使用默认设置即可，当然你也可以指定其他的位置。

  2. 安装雾凇拼音

      Windows 平台安装雾凇拼音非常方便，可以直接使用小狼毫输入法自带的配置工具。

      **更新步骤与安装步骤完全相同**。

      + 第一步，右键点击任务栏上的 RIME 图标，选择「输入法设定」，打开配置工具。

      + 第二步，在配置工具中，点击左下角的「获取更多输入方案」按钮。

      + 第三步，随后会出现一个命令行窗口，这就是小狼毫自带的配置文件安装工具。在提示符「`Enter package name...`」后，输入雾凇拼音的包名（其中，full表示安装所有的组件）：

          ```powershell
          iDvel/rime-ice:others/recipes/full
          ```

      + 第四步，回车确认，随即 RIME 会自动下载、安装雾凇拼音输入方案，如下图所示。

      + 注意：配置文件安装工具需要用到 Git。

      + 第五步，稍等片刻，命令提示符出现「`Updated xxx files...`」的提示（黄色字样），表示安装完成。此时可以直接关掉该窗口。

      + 第六步，回到小狼毫配置工具，将列表往下拉，你就会看到雾凇拼音的选项。勾选它，然后单击「中」按钮3，确认。

          接下来配置工具还会要求你选择一款皮肤，选择`纯粹的形式`。直接点击「中」按钮确认，即可完成全部设置。

  3. 激活雾凇拼音输入方案

      现在雾凇拼音输入方案已经准备就绪，但还没有激活。此时输入文字，仍然还在使用原有的拼音方案。

      接下来，只需要按 `「Ctrl+~」` 快捷键（其中，`「~」` 键位于 `Tab` 键的正上方），打开 RIME 的输入方案选择菜单，选择 雾凇拼音 即可。

## 一些配置

  1. 字体

      推荐[霞鹜文楷](https://github.com/lxgw/LxgwWenKai)

      前往用户文件夹，修改`weasel.custom.yaml`文件

          ```yaml
          patch:
            "style/color_scheme": purity_of_form_custom # 选择的皮肤
            "style/font_face": "霞鹜文楷, Segoe UI Emoji, Microsoft YaHei, SF Pro, Noto Color Emoji" # 字体
            "style/font_point": 12 # 字体大小
            "style/label_font_face": "霞鹜文楷" # 序号字体
            "style/comment_font_face": "霞鹜文楷" # 注字体
            "show_notifications": false # 不显示状态变化的通知
          ```

  2. 候选词个数

      前往用户文件夹，修改`default.custom.yaml`文件

          ```yaml
            patch:
              schema_list:
                - {schema: rime_ice} # 选定的雾凇拼音输入方案
              menu:
                page_size: 7 # 修改候选词数量为7
          ```

  3. 使用 Ctrl + space 切换中英文输入法

      1. 禁用系统 Ctrl + space 切换中英文

          ```
          1. 打开注册表，跳转到HKEY_CURRENT_USER/Control Panel/Input Method/Hot Keys目录下面
          2. 选择00000070（中文繁体）或者00000010（中文简体）
          3. 将Key Modifiers的第一个字节设置为00（02c00000->00c00000）
          4. 将Virtual Key的第一个字节设置为ff（20000000->ff000000）
          5. 注销用户然后重新登录，搞定

          另外
          HKEY_CURRENT_USER/Control Panel/Input Method/Hot Keys，保存的是当前用户的快捷键配置；
          HKEY_USERS.DEFAULT\Control Panel\Input Method\Hot Keys，保存的是默认的快捷键配置；
          若修改上一个注册表不好使，那就把下面的默认的也修改了
          ```

      2. 配置 `default.custom.yaml`

          ```
          patch:
            # 禁用默认中西文切换
            "ascii_composer/switch_key/Shift_L": noop
            "ascii_composer/switch_key/Shift_R": noop

            # 新建 中西文切换 快捷键绑定
            "key_binder/bindings":
              - { when: always, accept: "Control+space", toggle: ascii_mode }
          ```

  4. 解决 方案选单快捷键 在 VSCode 中与打开终端快捷键冲突

      禁用 `default.yaml` 中的 `方案选单相关` 下 `hotkeys` 下的 `Control+grave` 选项，并重新部署

## 相关文档

  + [RIME官方网站](https://rime.im/download/)

  + [雾凇拼音](https://github.com/iDvel/rime-ice)

  + [Rime 配置：雾凇拼音](https://dvel.me/posts/rime-ice)

  + [RIME + 雾凇拼音，打造绝佳的开源文字输入体验](https://sspai.com/post/89281)

完。
