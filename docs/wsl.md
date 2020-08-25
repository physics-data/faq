## 启动 WSL 功能

更详细的介绍，可见 OI Wiki 的 [WSL (Windows 10)](https://oi-wiki.org/tools/wsl/) 页面。

1. 打开控制面板
2. 所有控制面板项
3. 程序和功能
4. 启用或关闭 Windows 功能
5. 开启 “适用于 Linux 的 Windows 子系统”

或者，你也可以用管理员权限打开 Powershell 或者 cmd，然后运行：

```cmd
> dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

参考文档：https://docs.microsoft.com/en-us/windows/wsl/install-win10

之后，你就可以到 Microsoft Store 中安装你喜欢的发行版（推荐使用 Debian 或者 Ubuntu，否则可能无法按照老师的教程操作）。

## Terminal

WSL 自带的终端比较难用，我们推荐从 Microsoft Store 安装 [Windows Terminal](https://www.microsoft.com/zh-cn/p/windows-terminal/9n0dx20hk701)。

## WSL 下 X11 环境的配置

需要在 Windows 上安装 [vcXsrv](https://sourceforge.net/projects/vcxsrv/files/) ，然后运行 `XLaunch` ，然后应该可以在右下角的状态栏中找到它。

接着，在 WSL 里面配置 `DISPLAY` 环境变量。如果是临时使用：

```bash
> export DISPLAY=:0
```

这样做的话，每次在用之前都需要重新运行这条命令。如果想一劳永逸，那么，运行下面的命令一次：

```bash
> echo 'export DISPLAY=:0' >> ~/.bashrc
```

它会往 `~/.bashrc` 文件中追加一个命令，而 `bash` 在每次启动的时候都会执行 `~/.bashrc` 里面的每条指令。完成后，重新开一个命令行窗口，如果出现下面的效果：

```bash
$ echo $DISPLAY
:0
```

那么，设置就成功了。注意，这样设置只对新开的窗口有效果。

