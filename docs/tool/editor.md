## nano 安装与使用

如果是 Debian/Ubuntu 系的系统，可以安装：

```bash
sudo apt install -y nano
```

如果要编辑文件，运行：

```bash
nano file_to_edit
```

比如，如果要编辑 apt 的源，可以：

```bash
sudo nano /etc/apt/sources.list
```

这里用 sudo 的原因是，root 用户才有权限修改这个文件。

进入 nano 之后，在屏幕中部应该可以看到文件内容，下面看到若干快捷键的提示，常用的如下：

- ^X (Ctrl-X)：退出 nano
- ^O (Ctrl-O)：保存文件，按回车确认
- ^\\ (Ctrl-\\)：进行替换

在执行命令途中，下面的菜单可能会变化，则按照相应的按键执行相应的操作即可。细心的读者可能发现，`^` 在这里表示的就是 Ctrl 键。

## Emacs 安装与使用

在 Debian/Ubuntu 中，可以安装 `emacs` 软件包，或者安装 `emacs-gtk` 以获得更多功能。

- 保存：C-x C-s
- 退出：C-x C-c

## Vim 安装与使用

在 Debian/Ubuntu 中，可以安装 `vim` 软件包，或者安装 `vim-gtk3`（可能会使 vim 启动变慢，因为它会寻找 X11 服务器打开窗口） 以获得更多功能。

如果需要保存并退出，请按 `Esc :wq`，可将 `wq` 替换为 `q!`，此时不会保存文件。

更详细的介绍，可见 OI Wiki 的 [Vim](https://oi-wiki.org/tools/editor/vim/) 页面。

## VSCode 安装与使用

我们推荐使用 Visual Studio Code 进行代码编写和工作。它具有大量的插件，也内建了 Git 的支持。

### 环境准备

如果你使用 macOS 或者 Linux，则无需特别配置。

如果你使用 Windows，请参照 [官方教程](https://code.visualstudio.com/docs/remote/wsl-tutorial) 安装 WSL 支持。

如果你在 Windows 上使用 Linux 虚拟机，请参照 [官方教程](https://code.visualstudio.com/docs/remote/ssh-tutorial) 安装 SSH 支持，并连接到你的虚拟机。

### 插件安装

请在从你的 **工作环境**（如 WSL / Remote SSH）启动的 VSCode 中，安装 Python 插件。注意安装在 Windows 中的插件不起作用。

然后，打开对应的文件夹，愉快地使用吧！
