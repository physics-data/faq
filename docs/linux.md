Linux 是最符合课程要求的环境，我们推荐使用 Debian GNU/Linux，Ubuntu 亦是 Debian 的衍生版。

## 配置 TUNA 镜像源

在安装系统后，建议使用 TUNA 镜像源以提高安装软件的速度。

### Debian

对于 Debian，编辑 `/etc/apt/sources.list`：

1. 运行 `sudo nano /etc/apt/sources.list`
2. 按 `Ctrl+\`，输入 `deb.debian.org`，回车；如果搜不到，并且文件中已经包括 `mirrors.tuna.tsinghua.edu.cn`，则不需要进行下面的操作
3. 输入 `mirrors.tuna.tsinghua.edu.cn`，回车
4. 输入 `A`，回车，表示替换所有
5. 按 `Ctrl-O`，回车，表示把修改后的更新到文件里
6. 按 `Ctrl-X` 退出

然后 `sudo apt update` ，如果没有失败，则配置成功。

### Ubuntu

如果是 Ubuntu，则用类似于 Debian 的方法把 `archive.ubuntu.com` 替换为 `mirrors.tuna.tsinghua.edu.cn` ，然后 `sudo apt update` 即可。
