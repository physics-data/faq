## `apt install` 的时候遇到 Unable to fetch some archives

安装时遇到 `Unable to fetch some archives, maybe run apt-get update or try with --fix-missing` 解决方法：

先尝试 `sudo apt update`，再重新执行之前报错的命令。

## 执行 `apt update` 或 `git push` 等操作遇到 `unresolved hostname` 或 `Temporary failure in name resolution`

一般是 DNS 问题，可以参考 [WSL2 DNS issues](https://github.com/microsoft/WSL/issues/5256)，执行命令 `sudo vim /etc/resolv.conf`，在文件最后添加：

```config
nameserver 114.114.114.114
nameserver 180.76.76.76
```

需要注意，修改 `/etc/resolv.conf` 的方法可能会导致之前 WSL 配置 DISPLAY 的方法冲突。