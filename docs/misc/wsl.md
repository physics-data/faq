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