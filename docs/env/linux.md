Linux 是最符合课程要求的环境，我们推荐使用 Debian GNU/Linux，Ubuntu 亦是 Debian 的衍生版。

## Linux 发行版安装

如果你是 WSL 用户，已经安装好了 Linux 发行版，可以跳过这一步。

如果要安装一个新的 Linux 发行版，那么：

- Ubuntu：使用 20.04 或更新的版本
- Debian：使用 bullseye 或更新的版本
- CentOS：不建议使用

在虚拟机中或者物理机中安装一个 Linux 发行版的流程大致是：

1. 下载一个安装镜像 .iso 文件
2. 如果是虚拟机，则挂载到虚拟机的虚拟光盘；如果是物理机，可以先烧录到 U 盘上，再从 U 盘启动
3. 按照安装向导一步步进行，如果是虚拟机，直接用整个盘进行分区即可；如果是物理机，并且要保留双系统，要注意不要把已有的 Windows 覆盖掉

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

## 常用 apt 命令

在学习课程的过程中，通常会使用 apt 来安装一些包。如果已经知道了包的名字，我们一般采用如下的方法：

1. 运行 `sudo apt update`，保证目前采用的是最新的软件包
2. 运行 `sudo apt install xxxx`，安装指定的包

如果不知道包的名字，可以尝试在本地寻找：`sudo apt search xxx`；也可以在网页上搜索，例如 <https://packages.ubuntu.com/> 和 <https://www.debian.org/distrib/packages> 。

## 用 apt 安装 python

安装好 Ubuntu/Debian 系统以后，我们可以安装 python3:

```shell
sudo apt install python3 python3-pip
python3 --version
```

安装好后，就可以看到 python3 的版本了。你应该可以看到 Python 3.8 或者更高的版本，否则就说明你的系统太老了。

接下来，我们就可以用 `pip3` 来安装一些常用的 python 包：

```shell
pip3 install matplotlib numpy
python3 -c 'import matplotlib;import numpy'
```

如果第二条命令导入的时候没有出错，就说明安装成功了。

如果 `pip3 install` 的时候显示网络错误，可以用 TUNA 镜像安装：

```shell
pip3 config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
pip3 install matplotlib numpy
```

测试 matplotlib 能否显示图像：

```shell
python3 -c 'import matplotlib.pyplot;matplotlib.pyplot.plot([3,2,1,2,3]);matplotlib.pyplot.show()'
```

如果弹出了窗口，并且窗口中出现了一个 V 字型，就说明绘图功能正常。