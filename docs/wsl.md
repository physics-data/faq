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
