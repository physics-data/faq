## Git Clone 时遇到 destination path already exists and is not an empty directory

例子：

```shell
$ git clone git@github.com:physics-data/xxx-xx.git
fatal: destination path 'xxx-xx' already exists and is not an empty directory.
```

这意味着 `xxx-xx` 目录已经存在，可能你之前已经运行过 `git clone`，不需要再 clone。直接 `cd xxx-xx` 进入目录即可。

## 访问 GitHub 时遇到 Permission denied (publickey)

请生成 SSH Key（`ssh-keygen`，注意，只要 `~/.ssh/id_rsa` 文件存在就不用再次生成），然后把 `~/.ssh.id_rsa.pub` 的内容添加到 GitHub 的 SSH Keys 设置中。设置好以后，再 `ssh -T git@github.com` 进行测试，如果输出：

```shell
$ ssh -T git@github.com
Hi xxx! You've successfully authenticated, but GitHub does not provide shell access.
```

那说明配置成功了。

需要注意的是，SSH Key 只需要配置一次即可，它相当于你的 GitHub 帐号的登录凭证。

## `apt install` 的时候遇到 Unable to fetch some archives

安装时遇到 `Unable to fetch some archives, maybe run apt-get update or try with --fix-missing` 解决方法：

先尝试 `sudo apt update`，再重新执行之前报错的命令。

## 执行 `apt update` 或 `git push` 等操作遇到 `unresolved hostname` 或 `Temporary failure in name resolution`

一般是 DNS 问题，可以参考 [WSL2 DNS issues](https://github.com/microsoft/WSL/issues/5256)，执行命令 `sudo vim /etc/resolv.conf`，在文件最后添加：

```
nameserver 114.114.114.114
nameserver 180.76.76.76
```

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