## `apt install` 的时候遇到 Unable to fetch some archives

安装时遇到 `Unable to fetch some archives, maybe run apt-get update or try with --fix-missing` 解决方法：

先尝试 `sudo apt update`，再重新执行之前报错的命令。

## 执行 `apt update` 或 `git push` 等操作遇到 `unresolved hostname` 或 `Temporary failure in name resolution`

一般是 DNS 问题，可以参考 [WSL2 DNS issues](https://github.com/microsoft/WSL/issues/5256)，执行命令 `sudo vim /etc/resolv.conf`，在文件最后添加：

```
nameserver 114.114.114.114
nameserver 180.76.76.76
```