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