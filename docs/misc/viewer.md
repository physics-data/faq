## Debian Buster 中 Vitables 无法运行

Debian 为了配合 2021 年 8 月 16 日开课，在 14 日发布了 Debian 11，代号为 `bullseye`。将 Debian 10 升级至 11 后即可使用 ViTables 3。

### 查看 Debian 的版本

安装 `lsb-release`：

```
apt install lsb-release
```

执行`lsb_release -a`，例如:
```

$ lsb_release -a
No LSB modules are available.
Distributor ID: Debian
Description:    Debian GNU/Linux 11 (bullseye)
Release:        11
Codename:       bullseye
```

如果"Codename"显示是 `bullseye`，表明是 Debian 11 系统。如果是 `buster`，表明是 Debian 10，需要升级。

### 升级 Debian

1. 使用 root 权限修改 `/etc/apt/sources.list`。Debian 系统应选择 `bullseye`，可参考 <https://mirrors.tuna.tsinghua.edu.cn/help/debian/>。
2. 使用 root 权限执行以下命令：
   ```
   apt update  # 更新索引
   apt full-upgrade # 升级操作系统
   ```
   过程会遇到很提示，默认选择 "Yes"。

参考：<https://github.com/physics-data/faq/discussions/67>。

## Vitables 无法调用 PyQt5

在 WSL 1 上的 Debian 11 和 Ubuntu 20.04 LTS 中，执行 vitables 会出现 `PySide2` 无法找到的错误。其根本原因是 `/usr/lib/x86_64-linux-gnu/libQt5Core.so.5` 的 `ABI-tag` 与 WSL 1 的内核不兼容，导致 PyQt5 无法被正常调用。

### 确认 WSL 版本

在 Windows Power Shell 中执行

```
wsl --list -v
```

查看 `VERSION` 列的输出，如
```
  NAME   STATE   VERSION
* Debian Running 1
```

表明 Debian 运行的环境为 WSL 1。或者如果 `wsl` 提示不支持 `-v` 选项，则代表它的版本是 1。

### 解决方法

使用 `strip` 把不兼容的部分去掉。以 `root` 用户执行，

```bash
strip --remove-section=.note.ABI-tag /usr/lib/x86_64-linux-gnu/libQt5Core.so.5
```

参考：<https://github.com/microsoft/WSL/issues/3023>。
