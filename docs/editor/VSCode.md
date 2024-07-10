## VSCode

使用 Visual Studio Code 进行代码编写和工作。它具有大量的插件，也内建了 Git 的支持。

### 环境准备

如果你使用 macOS 或者 Linux，正常情况下无需特别配置。

如果你使用 macOS 发现无法通过 `code` 指令运行 VSCode，你可以在 VSCode 中通过 `Cmd+Shift+P` 快捷键打开命令面板（Command Palette），然后输入 `shell command` 找到 `Shell Command: Install 'code' command in PATH` 并运行。其他问题可以参考[官方教程](https://code.visualstudio.com/docs/setup/mac)。

如果你使用 Linux 系统，各种问题也可以参考[官方教程](https://code.visualstudio.com/docs/setup/linux)。

如果你使用 Windows，请参照 [官方教程](https://code.visualstudio.com/docs/remote/wsl-tutorial) 安装 WSL 支持。

如果你在 Windows 上使用 Linux 虚拟机，请参照 [官方教程](https://code.visualstudio.com/docs/remote/ssh-tutorial) 安装 SSH 支持，并连接到你的虚拟机。

### 插件安装

请在从你的 **工作环境**（如 WSL / Remote SSH）启动的 VSCode 中，安装 Python 插件。注意安装在 Windows 中的插件不起作用。

打开对应的文件夹开始使用。

### 插件安装 FAQ
在 VSCode 中搜索插件时，出现错误提示“Error while fetching extentions. XHR failed”：

* 可能你正在科学上网。这种情况下可以关闭你的梯子，并刷新网络设置。参考：https://zhuanlan.zhihu.com/p/614288766
* 可能你的 VSCode 版本太老了。从左下角的设置按钮点击“检查更新”，并按照提示安装新版本。
