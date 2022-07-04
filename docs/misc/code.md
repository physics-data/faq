## WSL 中无法使用`code .`

### 添加环境变量

在 WSL 环境中的 PATH 中添加：`/mnt/c/Users/Username/AppData/Local/Programs/Microsoft VS Code/bin/code`。

### 完整路径

在 WSL 里写完整路径运行：`"/mnt/c/Users/Username/AppData/Local/Programs/Microsoft VS Code/bin/code" .`（注意双引号是半角，也需要输入到终端）

注意，以上路径中`Username`为 Windows 环境下的用户名。
