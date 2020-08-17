# 配置 TUNA 镜像源

对于 Debian，编辑 `/etc/apt/sources.list`：

1. 运行 `sudo nano /etc/apt/sources.list`
2. 按 `Ctrl+\`，输入 `deb.debian.org`，回车
3. 输入 `mirrors.tuna.tsinghua.edu.cn`，回车
4. 输入 `A`，回车，表示替换所有
5. 按 `Ctrl-O`，回车，表示把修改后的更新到文件里
6. 按 `Ctrl-X` 退出

然后 `sudo apt update` ，如果没有失败，则配置成功。

如果是 Ubuntu，则用类似的方法把 `archive.ubuntu.com` 替换为 `mirrors.tuna.tsinghua.edu.cn` 。