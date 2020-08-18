## nano 安装与使用

如果是 Debian/Ubuntu 系的系统，可以安装：

```bash
$ sudo apt install -y nano
```

如果要编辑文件，运行：

```bash
$ nano file_to_edit
```

比如，如果要编辑 apt 的源，可以：

```bash
$ sudo nano /etc/apt/sources.list
```

进入 nano 之后，在屏幕中部应该可以看到文件内容，下面看到若干快捷键的提示，常用的如下：

- ^X (Ctrl-X)：退出 nano
- ^O (Ctrl-O)：保存文件
- ^\ (Ctrl-\)：进行替换

在执行命令途中，下面的菜单可能会变化，则按照相应的按键执行相应的操作即可。细心的读者可能发现，`^` 在这里表示的就是 Ctrl 键。
