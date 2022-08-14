## WSL 中无法使用 `code .`

### 添加环境变量

在 WSL 环境中的 PATH 中添加：`/mnt/c/Users/Username/AppData/Local/Programs/Microsoft VS Code/bin/code`。

### 完整路径

在 WSL 里写完整路径运行：`"/mnt/c/Users/Username/AppData/Local/Programs/Microsoft VS Code/bin/code" .`（注意双引号是半角，也需要输入到终端）

注意，以上路径中`Username`为 Windows 环境下的用户名。

## 升级 WSL 的时候遇到错误 "This update only applies to machines with the Windows Subsystem for Linux"

可以根据 <https://github.com/microsoft/WSL/issues/5035> 中描述的方法尝试解决：

第一种：

```
I have just got it working by running wsl_update_x64 from powershell with admin rights as opposed to double clicking.
```

第二种：

```
Procedure to fix the issue :-

1. Open "turn windows features on or off"
2. Uncheck both "Virtual Machine Platform" and "Windows subsystem for Linux"
3. Press OK
4. Reboot your system
5. Open "turn windows features on or off" again
6. Now check both "Virtual Machine Platform" and "Windows subsystem for Linux"
7. Press OK
8. Reboot your system again
9. Now run the wsl_update_x64.msi again, it should install successfully now
10. Go to powershell and run wsl command

Demo Video for graphical representation
https://youtu.be/4czn-4uu65c
```

## WSL2 中程序运行时出现 "Read-only filesystem"

这一般是因为文件系统损坏（或者 Windows 盘满了），系统自动设置文件系统为只读模式。

如果是盘满了，先清理一下，保证至少有几个 G 的空闲空间。

然后，为了解决文件系统的损坏问题，在 WSL2 中运行：

```shell
sudo e2fsck $(mount | grep ext4 | awk '{print $1}') -y
sudo e2fsck $(mount | grep ext4 | awk '{print $1}') -p
```

如果成功修复，则在命令提示符中运行 `wsl --shutdown`，然后再启动 wsl。

参考文档：

- https://github.com/microsoft/WSL/issues/8340
- https://github.com/microsoft/WSL/issues/4833

## WSL 忘记密码怎么办？

@liuyiqi20 同学提供了回答：

WSL 系统中可以使用 root 用户身份重置普通用户的密码。

启用命令行或 PowerShell，如果需要重置的用户是 **默认WSL分发版** 的用户，使用以下命令登录 root 用户：

```cmd
wsl -u root
```

如果在 Windows 中只安装了一种版本的 WSL，就只需要按照上述操作登入 root 用户。

如果安装了多种版本，可以在命令行或 PowerShell 中使用 ```wsl -l``` 查看 WSL 分发版名称以及默认情况。倘若要重置的用户是非默认 WSL 分发版的用户，则需要使用以下命令登录 root 用户：

```cmd
wsl -d <Linux_name> -u root
```

其中 ```<Linux_name>``` 为对应分发版的名称，如 Debian 或 Ubuntu 等。

登入 root 用户后，可以使用 ```passwd <username>``` 来更新密码，其中 ```<username>``` 是需要重置的用户名。

输入并确认密码后，最好通过 ```exit``` 关闭 WSL。

参考：[设置 Linux 用户名和密码-Microsoft Docs](https://docs.microsoft.com/zh-cn/windows/wsl/setup/environment#set-up-your-linux-username-and-password)

## WSL 出现报错 "参考的对象类型不支持尝试的操作"

用管理员权限运行：

```cmd
netsh winsock reset
```

参考：https://github.com/microsoft/WSL/issues/4194

## 如何判断当前命令行环境

对于 Windows 的同学来说，命令行环境可能有三种：

1. Windows 的命令行提示符：显示当前路径+`>`，如：`C:\Users\xxxx>`
2. Windows 的 PowerShell：显示 PS+当前路径+`>`，如：`PS C:\Users\xxxx>`
3. WSL 的命令行：显示用户名+`@`+机器名+`:`+当前路径+`$`，如：`user@machine:~$`

本课程的大部分命令都是在第三种命令行环境下运行。`wsl` 命令在第一种或者第二种命令行环境下运行。

## WSL 的可执行权限

同学们在用 WSL 开发的时候，如果仓库克隆的时候在 `/mnt` 下面，push 上去的时候可能会出现文件没有可执行权限的问题。

这是因为，WSL 中的 `/mnt` 目录是将 Windows 的文件系统作为网络共享挂载到本地。但是，Linux 下面的文件权限和 Windows 不同，所以 Windows 的文件在 WSL 中用 `ls -alh` 看到的权限都是 `rwxrwxrwx`。

但是，我们知道，Git 会跟踪文件的权限，这就出现了矛盾：比如原来的权限是 `rw-r--r-`，然后在 Windows 路径下克隆以后，由于上述限制，会变成 `rwxrwxrwx`。但是 Git 又无法把文件权限设置为正确的，所以 Git 会在仓库中自动设置 `core.filemode=false`：

```shell
$ git config --list
core.filemode=false
```

于是，Git 会无视文件的权限，而是直接设置一个默认值。这可能导致在 CI 和助教的评测环境中出现不可执行的情况，导致黑盒零分。

根据上面的分析，解决方案也很明确了：

1. 在 HOME 目录下克隆仓库，这一步的目的是得到正确的文件权限，并且 Git 也会自动设置 `core.filemode=true`，这样它才会跟踪文件权限的变化
2. 进入仓库，向需要调整权限的脚本添加执行权限，直到本地评分通过
3. 此时运行 `git status`，会提示有文件权限的变更
4. 提交修改

## 已知并且没有被修复的 WSL bug

WSL1:

1. [WSL1 下 Ubuntu 22.04 的 gzip 无法执行](https://github.com/microsoft/WSL/issues/8219)：Ubuntu 22.04 可以复现：`/usr/bin/gzip: cannot execute binary file: Exec format error`，Debian Bullseye 和 Ubuntu 20.04 正常
2. [WSL1 下 Ubuntu 22.04 的 gdb 无法打断点](https://github.com/microsoft/WSL/issues/8356)：GDB 11 开始不兼容，报错 `Cannot insert breakpoint 1`，Debian Bullseye 和 Ubuntu 20.04 的 GDB 正常
3. [WSL1 下部分动态库（如 libQt5Core.so）不工作](https://github.com/microsoft/WSL/issues/3023)：Debian Bullseye 可以复现：`wireshark: error while loading shared libraries: libQt5Core.so.5: cannot open shared object file: No such file or directory`
