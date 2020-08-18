## 启动 WSL 功能

1. 打开控制面板
2. 所有控制面板项
3. 程序和功能
4. 启用或关闭 Windows 功能
5. 开启 “适用于 Linux 的 Windows 子系统”

或者，你也可以用管理员权限打开 Powershell，然后运行：

```shell
$ dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

参考文档：https://docs.microsoft.com/en-us/windows/wsl/install-win10

之后，你就可以到 Microsoft Store 中按照你喜欢的发行版。
