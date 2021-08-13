WSL (Windows Subsystem for Linux) 是微软开发的在 Windows 上提供 Linux 支持的功能，能够提供一个几乎与原生 Linux 相同的环境，较好地满足 Windows 用户的课程需要。我们推荐所有 Windows 用户安装并使用 WSL。


!!! warning "注意环境"
    
    如果你有任何已经配置好的 POSIX 环境（如 Cygwin、MinGW），同样可以使用。但我们不保证它能够完整地满足课程需求，也无法提供支持。

## 版本

WSL 推出以来，存在 WSL1 与 WSL2 两个版本。如果你已经安装了其中的任何一个，它都可以符合我们的需要；如果没有，则默认将会使用 WSL2。

## 安装 WSL
### 推荐：脚本安装

为了方便同学们的安装和使用，今年助教团队基于最新的 Debian 制作了 WSL 镜像（其中包含了课程需要的绝大多数常用软件），并编写脚本支持一键安装、导入、卸载。我们推荐所有新用户使用这一方法安装。

镜像下载地址为：<https://physics-data.meow.plus/wsl/>。请按照页面上的支持执行即可完成安装。安装后，可直接查阅参见 Terminal 一节。

### 手工安装

详细的介绍，可见 OI Wiki 的 [WSL (Windows 10)](https://oi-wiki.org/tools/wsl/) 页面。

1. 打开控制面板
2. 所有控制面板项
3. 程序和功能
4. 启用或关闭 Windows 功能
5. 开启 “适用于 Linux 的 Windows 子系统”

或者，你也可以用管理员权限打开 Powershell 或者 cmd，然后运行：

```cmd
> dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
> dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

参考文档：https://docs.microsoft.com/en-us/windows/wsl/install-win10

之后，你就可以到 Microsoft Store 中安装你喜欢的发行版（推荐使用 Debian 或者 Ubuntu，否则可能无法按照老师的教程操作）。

## 终端（Terminal）

为了与 WSL 交换，我们需要使用终端模拟器（terminal emulator），通常简称为终端（terminal）。

WSL 安装后会自带一个终端（在运行中输入 `wsl` 即可启动），但其并不美观，也缺失必要的功能。我们推荐从 Microsoft Store 安装 [Windows Terminal](https://www.microsoft.com/zh-cn/p/windows-terminal/9n0dx20hk701)，这是微软开发的一款现代终端，功能比较强大。

Windows Terminal 能够自动检测本机所有的 WSL 发行版。如果在使用时导入了新的 WSL 发行版，在设置中随意进行一点改动并保存，就能触发自动检测机制。

## WSL 下 X11 环境的配置

如果需要在 WSL 中使用 GUI 程序，最简单的办法是在 Windows 上安装 [vcXsrv](https://sourceforge.net/projects/vcxsrv/files/) ，然后运行 `XLaunch` ，然后应该可以在右下角的状态栏中找到它。

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

如果上述方法不工作，可以参考 [@Light1110 同学提供的解决方案](https://github.com/physics-data/faq/issues/6#issuecomment-680972955)：

解决方案：
1. 如下配置环境变量 `DISPLAY`：

```
export DISPLAY=$(awk '/nameserver / {print $2; exit}' /etc/resolv.conf 2>/dev/null):0
```

2. 配置 X11 server 使其允许远程接入。
如：使用 VcXsrv 时，勾选 `Disable access control`   

参考：https://stackoverflow.com/questions/61110603/how-to-set-up-working-x11-forwarding-on-wsl2

如果你是 Windows 高级用户，可以体验 Windows 新的 [WSLg](https://github.com/microsoft/wslg) 功能，可能获得更好的图形性能和体验。
