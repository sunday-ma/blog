---
title: 使用Scoop管理windows软件
published: 2025-05-07
tags:
  - windows
  - scoop
toc: false
lang: zh
---

## 🔧 准备工作

  * **操作系统**：Windows 10 或 Windows 11（Windows 7 SP1+ 理论上也支持，但更推荐新版本）。

  * **PowerShell**：版本 5.1 或更高版本。Windows 10/11 通常自带。

    + **检查方法**：打开 PowerShell（按 Win + X，选择“Windows PowerShell”或“终端”），输入 $PSVersionTable.PSVersion 并回车，查看 Major 版本号是否 >= 5。
  
  * **.NET Framework**：4.5 或更高版本 (通常新版 Windows 已满足)。

  * **网络连接**：稳定且能访问 GitHub (Scoop 的核心依赖)。

  * **管理员权限**：虽然 Scoop 的核心理念是非管理员权限安装，但在首次设置 PowerShell 执行策略或进行全局安装时可能需要。建议首次操作时，以管理员身份运行 PowerShell。

  * **基本命令行知识**：别担心！如果你是新手，只需跟着步骤复制粘贴命令即可。我会解释每一步的作用。

  * **网络环境**：全程代理网络。如果你在国内网络环境下遇到困难（如下载慢、连接失败），可能需要配置代理或使用其他网络优化方法。文末会提供一些解决方案思路。

## 🚀 核心步骤

### 步骤一：安装 Scoop 本体

  Scoop 本身就是一个需要被“安装”的工具。

  1. **打开 PowerShell:**

    * 按下 Win + X 快捷键，在菜单中选择 “Windows PowerShell (管理员)” 或 “终端 (管理员)”。**建议首次使用管理员权限，**以确保后续步骤顺利。

  2. **设置执行策略:**

      * 为了允许 PowerShell 运行像 Scoop 安装脚本这样的本地或远程签名的脚本，需要调整执行策略。输入以下命令：

          ``` powershell
          Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
          ```

      * 按下回车。如果系统询问是否更改执行策略，输入 `Y` 并回车确认。

      * 💡小科普:

          - `Set-ExecutionPolicy`: 这是设置 PowerShell 执行策略的命令。

          - `RemoteSigned`: 这是一种安全策略，允许运行本地创建的脚本，对于从网络下载的脚本，则要求它们具有可信发布者的数字签名。这比 `Unrestricted`（允许所有脚本）更安全。
          
          - `-Scope CurrentUser`: 表示这个设置仅对当前登录的用户生效，通常不需要管理员权限（但首次设置可能需要确认），也更安全。

  3. **执行安装命令:**

      * 接下来，粘贴并运行以下命令来下载并执行 Scoop 的官方安装脚本：

        ```powershell
        Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
        # 或者使用别名更简洁的版本 (iwr = Invoke-RestMethod, iex = Invoke-Expression)
        # iwr -useb get.scoop.sh | iex
        ```

      * 💡小科普:

          - `Invoke-RestMethod` (或 `iwr`): 从指定的 [URL](https://get.scoop.sh) 下载内容（这里是安装脚本）。

          - `|` (管道符): 将前一个命令的输出（下载的脚本内容）传递给后一个命令。
          
          - `Invoke-Expression` (或 `iex`): 执行接收到的字符串内容（也就是运行安装脚本）。

  4. **预期结果:**

      * 等待脚本执行完毕。如果一切顺利，你会在 `PowerShell` 窗口看到类似 `“Scoop was installed successfully!”` 的成功信息。

      * 默认情况下，`Scoop` 会安装在你的用户目录下：`C:\Users\你的用户名\scoop`。


  5. **(可选) 修改默认安装路径:**

      * 如果你不想把 `Scoop` 安装在 `C` 盘，或者想统一管理软件，可以在执行安装命令之前，先运行以下两条命令来指定路径（例如，安装到 `D:\Apps\Scoop`）：

          ```powershell
          # 1. 设置 Scoop 的安装目录环境变量
          $env:SCOOP='D:\Apps\Scoop'
          # 2. 将这个环境变量永久写入用户配置 (下次打开 PowerShell 依然有效)
          [Environment]::SetEnvironmentVariable('SCOOP', $env:SCOOP, 'User')
          # 3. (可选) 设置全局安装路径 (如果需要全局安装软件)
          # $env:SCOOP_GLOBAL='D:\GlobalApps'
          # [Environment]::SetEnvironmentVariable('SCOOP_GLOBAL', $env:SCOOP_GLOBAL, 'Machine') # Machine 级别需要管理员权限
          ```

      * 设置完路径后，`再执行上面的第 3 步安装命令`。

### 步骤二：验证安装与添加软件仓库 (Bucket)

    Scoop 使用 “Bucket”（桶）来管理软件包列表，每个 Bucket 就是一个 Git 仓库，里面包含了软件的安装信息（称为 Manifest，清单文件）。默认只有 main Bucket，主要包含常用的命令行工具。我们需要添加更多 Buckets 来发现和安装更多软件。


  1. **验证 Scoop:**

      + 在 `PowerShell` 中输入 `scoop help` 并回车。如果看到 Scoop 的帮助信息和可用命令列表，说明 Scoop 已成功安装并可以工作了。

  2. **添加常用 Bucket:**

      + `extras` Bucket 包含了大量流行的 GUI 软件和非 main Bucket 的常用工具。强烈建议添加：

          ```powershell
          scoop bucket add extras
          ```

      + 💡**提示**: 添加 Bucket 需要从 GitHub 克隆仓库，如果网络慢请耐心等待。如果失败，检查网络或代理设置。

      + **重要**: Scoop 依赖 `git` 来管理 Buckets。如果你的系统没有安装 `git`，Scoop 在添加第一个 Bucket 时通常会提示并自动尝试安装。你也可以手动安装：`scoop install git`。

  3. **添加其他常用 Buckets (可选):**

      + 根据你的需要，可以添加更多社区维护的 Buckets。例如：

          - 安装各种 Java 版本：`scoop bucket add java`

          - 安装 Nerd Fonts (美化终端字体)：`scoop bucket add nerd-fonts`

          - 安装特定旧版本的软件：`scoop bucket add versions`

          - 安装非便携应用 (需要特殊处理)：`scoop bucket add nonportable` (安装里面的软件可能需要管理员权限)

      + **去哪里找更多 Buckets？**

          - 官方维护的 [Bucket](https://github.com/ScoopInstaller) 列表

          - 社区维护的已知 Buckets 列表：可以在网上搜索 “known scoop buckets” 或 查看 [这个非官方索引](https://rasa.github.io/scoop-directory/) (但请注意，首选官方或广泛使用的 Bucket)。

  4. **注意事项:**

      + 添加 Bucket 时需要良好的网络连接。

      + 如果命令出错，请检查 Bucket 名称是否拼写正确。

      + 有时网络波动会导致添加失败，重试一次可能就好了。

### 步骤三：用 Scoop 安装你的常用软件

    激动人心的时刻到了！现在你可以像逛超市一样，用简单的命令安装软件了。

  1. **搜索软件:**

      + 不确定软件是否在已添加的 Buckets 里？用 `search` 命令！

          ```powershell
          scoop search <你想搜索的软件名或关键词>
          # 例如: 搜索 pnpm
          scoop search pnpm
          ```

      + Scoop 会列出匹配的软件包及其所在的 Bucket。

      + 更方便的搜索: 你也可以直接访问 [Scoop 的官方网站](https://scoop.sh/)，它提供了一个图形化的搜索界面。注意取消勾选页面上的 “main” 筛选器可以搜索到 `extras` 等其他 Bucket 中的包。

  2. **安装单个软件:**

      + 找到你需要的软件名后，使用 `install` 命令安装。

          ```powershell
          # 安装 Git (来自 main bucket)
          scoop install git
          # 安装 7zip (来自 main bucket)
          scoop install 7zip
          # 安装 VS Code (来自 extras bucket)
          scoop install vscode
          ```

  3. **一次安装多个软件:**

      + 提高效率，一次性安装多个！用空格隔开软件名即可。

        ```powershell
        scoop install nodejs python mysql nginx putty everything powertoys
        ```

  4. **(可选) 全局安装:**

      + 默认情况下，Scoop 安装的软件只对当前用户可用，并且安装在用户目录下的 `scoop` 文件夹中（如 `C:\Users\你的用户名\scoop\apps`）。这通常不需要管理员权限，也更干净、便携。

      + 如果你希望软件对系统上的所有用户都可用，可以使用 `-g` 或 `--global` 参数。这**需要管理员权限**运行 PowerShell，并且软件会被安装到全局路径（默认为 `C:\ProgramData\scoop`，或通过 `$env:SCOOP_GLOBAL` 指定的路径）。

          ```powershell
          # 全局安装 OpenJDK (需要管理员权限运行 PowerShell)
          scoop install openjdk --global
          ```

      + 💡**建议**: 除非你明确知道需要全局安装，否则优先使用默认的用户模式安装，这更能体现 Scoop 的优势。

  5. **预期结果:**

      + Scoop 会自动下载软件包、解压、处理依赖，并将可执行文件通过 “`shim`” 机制添加到你的 `PATH` 中。安装完成后，你通常可以直接在新的 `PowerShell` 或 `CMD` 窗口中使用该软件的命令。

      + 例如，安装 `openjdk` 后，打开一个新的 `PowerShell` 窗口，输入 `java -version`，应该能看到 `Java` 的版本信息。

  6. 💡**Scoop 的魔法：Shim 是什么？**

      + Scoop 的一个核心优势是**不污染**系统的 `PATH` 环境变量。它怎么做到的？答案是 “`Shim`” (垫片)。

      + 当你安装一个软件（比如 `git`）后，`Scoop` 不会把 `git.exe` 所在的整个目录加到系统 `PATH`。它只做一件事：在 `~/scoop/shims` 目录下（这个目录在安装 `Scoop` 时会被自动添加到用户 `PATH` 中）创建一个名为 `git.exe` 的极小的可执行文件 (`shim`)。

      + 当你运行 `git` 命令时，系统在 `PATH` 里找到了 `~/scoop/shims/git.exe`。这个 `shim` 文件知道真正 `git.exe` 的位置（比如在 `~/scoop/apps/git/current/bin/git.exe`），然后它会启动真正的 `git.exe`。

      + 这样做的好处是：你的 `PATH` 变量非常干净，只增加了一个 `shims` 目录。卸载软件时，只需删除对应的 `shim` 和软件目录，对系统几乎没有影响。对于 GUI 程序，`Scoop` 会在开始菜单创建一个 “`Scoop Apps`” 文件夹存放快捷方式。

### 步骤四：软件的更新与管理

    安装只是开始，Scoop 还能帮你轻松管理已安装的软件。

  1. **检查更新:**

      + 想知道哪些软件有新版本了？

          ```powershell
          # 检查 Scoop 自身和所有 Buckets 的更新，并列出可更新的软件
          scoop status
          # 或者直接更新 Scoop 自身和 Buckets 的信息
          scoop update
          ```

      + `scoop update` 首先会更新 `Scoop` 自身和所有已添加的 `Bucket`（相当于对每个 `Bucket` 的 `Git` 仓库执行 `git pull`），获取最新的软件包清单。

  2. **更新所有软件:**

      + 一键更新所有已安装且有新版本的软件：

          ```powershell
          scoop update *
          ```

  3. **更新指定软件:**

      + 只想更新某个特定的软件？

          ```powershell
          scoop update <软件名>
          # 例如: 更新 nodejs
          scoop update nodejs
          ```

  4. **查看已安装列表:**

      + 想看看自己都装了些什么？

          ```powershell
          scoop list
          ```

  5. **卸载软件:**

      + 不再需要某个软件了？干干净净地移除它！

          ```powershell
          scoop uninstall <软件名>
          # 例如: 卸载 mysql
          scoop uninstall mysql
          ```

      + Scoop 会删除软件文件和对应的 shim/快捷方式，非常彻底。

  6. **查看软件信息:**

      + 想了解某个已安装或可用软件的详细信息（版本、来源、依赖等）？

          ```powershell
          scoop info <软件名>
          # 例如: 查看 git 的信息
          scoop info git
          ```

  7. **回退到旧版本:**

      + 更新后发现新版本有 Bug？可以尝试回退。

          ```powershell
          # 先查看可用版本
          scoop info <软件名>
          # 回退到指定版本 (例如回退 git 到 2.30.0.windows.1)
          scoop reset <软件名>@<版本号>
          scoop reset git@2.30.0.windows.1
          ```

  8. **其他常用命令:**

      + `scoop cleanup *` : 清理所有软件的旧版本，释放磁盘空间。

      + `scoop cache rm *` : 清除下载缓存。
      
      + `scoop home <软件名>` : 在浏览器中打开软件的官方主页。

      + `scoop which <命令名>` : 显示某个命令对应的可执行文件的实际路径 (类似 Linux 的 which)。

### 引用自：[重装电脑用Scoop管理软件,清爽多了!](https://linux.do/t/topic/566873)

完。