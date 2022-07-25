## Linux 下的路径

Linux 下的路径从根目录开始（`/`），而没有盘符。路径的每一段由 `/` 分隔，和 Windows 不一样。

你的 Home 目录在一些地方（比如 Shell），可以用 `~` 代替，它实际上是一个环境变量 `$HOME`，你可以用 `echo $HOME` 看到它的内容，一般是 `/home` 下的目录。

在命令行中，一般用 `cd` 切换当前路径，用 `pwd` 显示当前路径。特别地，`.` 表示当前目录，`..` 表示上一级目录。

## WSL 下的路径

### WSL 访问 Windows 路径

你可以在 WSL 下访问 Windows 路径，对应关系例子：Windows 路径 C:\a\b\c\d 对应 WSL 里面的路径 /mnt/c/a/b/c/d。

但需要注意，访问 Windows 路径可能会有一些问题，一些软件可能不会在 Windows 路径下正常工作。这个时候，建议复制到 WSL 里面的 HOME 目录再使用。

### Windows 显示 WSL 路径

你也可以在 Windows 的文件资源管理器中显示 WSL 的文件。在文件资源管理器的地址栏中输入 `\\wsl$\`， 即可找到一个显示 Linux 系统名称的文件夹，打开即可进入 Linux 的根目录。

此外，在某个 linux 目录下输入

```bash
explorer.exe .
```

即可在 Windows 的文件资源管理器中打开当前目录。当然，这个方法在 Windows 自己的命令行中也是生效的。
