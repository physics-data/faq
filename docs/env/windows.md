为了在本机上配置一个 POSIX 环境，使用 Windows 系统的同学可以在以下几种方法中选择一种方案配置，推荐程度由高到低：

1. WSL（Windows Subsystem for Linux）
2. 虚拟机（Hyper-V/VirtualBox/VMware）
3. MinGW/Cygwin

## WSL

WSL (Windows Subsystem for Linux) 是微软开发的在 Windows 上提供 Linux 支持的功能，能够提供一个几乎与原生 Linux 相同的环境，较好地满足 Windows 用户的课程需要。我们推荐所有 Windows 用户安装并
使用 WSL。

### 版本

WSL 推出以来，存在 WSL1 与 WSL2 两个版本。如果你已经安装了其中的任何一个，它都可以符合我们的需要；如果没有，则默认将会使用 WSL2。

**可以通过在命令提示符中运行 `wsl -l -v` 命令来判断目前的 WSL 版本。**

WSL2 要求 Windows 版本如下：

- For x64 systems: Version 1903 or higher, with Build 18362 or higher.
- For ARM64 systems: Version 2004 or higher, with Build 19041 or higher.

如果 Windows 版本低于所要求的版本，建议升级到更新的 Windows 10 版本并安装 WSL2；如果无法升级，再尝试使用 WSL1。

### 安装 WSL

可以观看[助教录制的 WSL 安装视频](https://hep.tsinghua.edu.cn/~orv/teaching/physics-data/WSL2Tutorial.mp4)。

详细的介绍，可见 OI Wiki 的 [WSL (Windows 10)](https://oi-wiki.org/tools/wsl/) 页面。简单来说，有如下几步：

1. 打开控制面板
2. 所有控制面板项
3. 程序和功能
4. 启用或关闭 Windows 功能
5. 开启 “适用于 Linux 的 Windows 子系统”

或者，你也可以用管理员权限打开 Powershell 或者 cmd，然后运行：

```cmd
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

参考文档：https://docs.microsoft.com/en-us/windows/wsl/install-win10

如果之前使用过旧版 Windows 或者 WSL1，可以运行下面命令设置默认版本为 WSL2:

```cmd
wsl --set-default-version 2 
```

### 安装 Linux 发行版

之后，你就可以到 Microsoft Store 中安装你喜欢的发行版（推荐使用 Debian，否则可能无法按照老师的教程操作）：

- Debian：使用 12 (bookworm) 或更新的版本
- Gentoo：使用 stable 版本
- Ubuntu：不建议使用，带有内嵌广告
- CentOS：建议不使用，已经停止开发。(`CentOS Stream` 不是 `CentOS`。)

### 终端（Terminal）

为了与 WSL 交换，我们需要使用终端模拟器（terminal emulator），通常简称为终端（terminal）。

WSL 安装后会自带一个终端（在运行中输入 `wsl` 即可启动），但其并不美观，也缺失必要的功能。我们推荐从 Microsoft Store 安装 [Windows Terminal](https://www.microsoft.com/zh-cn/p/windows-terminal/9n0dx20hk701)，这是微软开发的一款现代终端，功能比较强大。

Windows Terminal 能够自动检测本机所有的 WSL 发行版。如果在使用时导入了新的 WSL 发行版，在设置中随意进行一点改动并保存，就能触发自动检测机制。

### 配置 Linux 系统

安装好 WSL 以后，就可以在终端中配置 Linux 系统了。

接下来，请参考 [Linux 环境配置](../linux) 文档。

### WSL 下 X11 环境的配置

如果需要在 WSL 中使用 GUI 程序，需要安装 X Server。

- **如果你正在使用 Windows 11**: WSL2 可能已经带有了一个 X Server 实现 ([WSLg](https://github.com/microsoft/wslg))。通过以下方法检查：
    - 启动终端进入 WSL2 发行版，执行 `echo $DISPLAY`，如果显示不为空，说明存在 WSLg。
- **如果你没有在使用 Windows 11，或者上述测试输出为空**: 需要自行安装 X Server，并手动设置 DISPLAY 环境变量。
    - 推荐安装 [VcXsrv](https://sourceforge.net/projects/VcXsrv/files/)。然后运行 `XLaunch`，会弹出一个配置窗口，一路前进，勾选 `Disable access control`，最后选择 `Finish`，然后应该可以在右下角的状态栏中找到它。这时 X Server 已经启动。第一次启动的时候可能会弹出窗口询问是否打开防火墙权限，此时要勾选所有的网络并同意。
    - 接着需要在 WSL 里面配置 `DISPLAY` 环境变量。这一步在不同的 WSL 版本中要用不同的办法。

#### WSL2

WSL2 可以参考 [@Light1110 同学提供的解决方案](https://github.com/physics-data/faq/issues/6#issuecomment-680972955)：

解决方案：

启动 X11 server，同时，配置 X11 server 使其允许远程接入：如：启动 VcXsrv 时，**勾选 `Disable access control`**。

在 WSL 内按如下命令配置环境变量 `DISPLAY`：

```bash
export DISPLAY=$(ip route show default | cut -d' ' -f3):0
```

如果上述命令还是不能工作，可以尝试下面的命令：

```bash
export DISPLAY=$(awk '/nameserver / {print $2; exit}' /etc/resolv.conf 2>/dev/null):0
```

如果 **类似** 下面的效果（实际 IP 可能不同）：

```bash
$ echo $DISPLAY
172.22.192.1:0
```

这样就算设置成功了。可以把这个设置写入到 `~/.bashrc` 中，这样下次启动 wsl bash 的时候也会生效。

如果还是连不上，可以尝试用管理员权限启动 VcXsrv。

参考：https://stackoverflow.com/questions/61110603/how-to-set-up-working-x11-forwarding-on-wsl2

#### WSL1

如果是临时使用：

```bash
export DISPLAY=:0
```

这样做的话，每次在用之前都需要重新运行这条命令。如果想一劳永逸，那么，运行下面的命令一次：

```bash
echo 'export DISPLAY=:0' >> ~/.bashrc
```

它会往 `~/.bashrc` 文件中追加一个命令，而 `bash` 在每次启动的时候都会执行 `~/.bashrc` 里面的每条指令。完成后，重新开一个命令行窗口，如果出现下面的效果：

```bash
$ echo $DISPLAY
:0
```

那么，设置就成功了。注意，这样设置只对新开的窗口有效果。

#### 测试 X Server

可以在 Linux 中安装 `x11-apps` 软件包并执行 xclock，如果 X Server 运转正常，可以显示出一个钟表。

```bash
sudo apt install x11-apps
xclock
```

注意，上面是 x 十一，不是 x 两个 L。

注：如果你在使用 WSLg，初次启动图形程序会有一定延迟，大约 10-20 秒，是正常现象。

#### 问题排查

如果还是连不上，按照以下的方式检查：

1. 检查 DISPLAY 环境变量是否配置了：`echo $DISPLAY`
2. 确认你用的是 WSL1 还是 WSL2：在 **命令提示符** 里运行 `wsl -l -v` 可以看到版本
3. 确认你在上面运行的命令和你的 WSL 版本是一致的
4. 检查 VcXsrv 是否启动：桌面右下角应该可以找到它的图标
5. 确认 VcXsrv 启动的时候勾选了 **Disable access control**：如果不记得了，退出 VcXsrv 再重新开一次
6. 在桌面右下角图标里右键，选择 Applications -> xcalc，确认 VcXsrv 本身可以显示窗口
7. 修改防火墙规则：设置，找到防火墙，选择高级设置，左侧找到入站规则，在中间找到所有 VcXsrv 相关的条目，如果左边显示的不是绿色的勾，则双击对应的属性，选择允许连接。保证所有 VcXsrv 相关的规则都是绿色的
8. 如果还是不行，尝试关闭杀毒软件
9. 如果还是不行，尝试用管理员权限启动 VcXsrv
10. 如果还是不行，尝试禁用整个防火墙

## 虚拟机

我们可以使用虚拟机软件，在虚拟机内运行一个完整的 Linux 系统，安装好 Linux 系统以后，其余的配置内容见 [Linux 环境配置](../linux)。

常用的虚拟机软件有：

- Hyper-V：微软官方的虚拟机
- VirtualBox：开源的虚拟机
- VMware：商业软件

实际使用的时候，选择其中一个即可，使用多个可能在某些情况下会有冲突的问题。

## MinGW/Cygwin

助教不使用这个环境，如果有同学要使用的话，可以自己摸索。
