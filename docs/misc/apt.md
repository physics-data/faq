## `apt install` 的时候遇到 Unable to fetch some archives

安装时遇到 `Unable to fetch some archives, maybe run apt-get update or try with --fix-missing` 解决方法：

先尝试 `sudo apt update`，再重新执行之前报错的命令。

如果仍然不行，再检查一下自己是否已经使用TUNA清华源，配置方式请参考[配置TUNA清华源](https://physics-data.meow.plus/faq/env/linux/#tuna).

## 执行 `apt update` 或 `git push` 等操作遇到 `unresolved hostname` 或 `Temporary failure in name resolution`

这个报错的意思是 DNS 询问一个域名对应的 IP 地址失败。软件通过 DNS 协议查询域名的原理是：

1. 根据 `/etc/resolv.conf` 文件的内容，找到 DNS 服务器的 IP 地址
2. 向 DNS 服务器的 IP 地址发送 DNS 请求
3. DNS 服务器返回查询结果

那么，如果发生错误，也是按照这个顺序来排查：

1. 查看 `/etc/resolv.conf` 中是否有 DNS 服务器的 IP 地址
2. 如果没有的话，添加一些常见的 DNS 服务器的 IP 地址，如 `114.114.114.114` 或 `180.76.76.76`
3. 如果有的话，如果是形如 172.x.x.x 的 IP 地址，那么这是 Windows 自动为 WSL2 设置的 DNS 服务器，一般不需要修改；如果是其他 IP 地址，可以用 ping 命令来判断联通性，如 `ping 114.114.114.114`
4. 检查一下是否可以访问网络，比如浏览器中打开 `www.tsinghua.edu.cn`，在 WSL 中尝试 `ping 101.6.6.6`

如果要修改 `/etc/resolv.conf` 的话，可以参考 [WSL2 DNS issues](https://github.com/microsoft/WSL/issues/5256)，执行命令 `sudo vim /etc/resolv.conf`，在文件最后添加：

```config
nameserver 114.114.114.114
nameserver 180.76.76.76
```

需要注意，修改 `/etc/resolv.conf` 的方法可能会与之前 WSL2 配置 DISPLAY 的方法冲突，因为之前使用的命令：

```shell
export DISPLAY=$(awk '/nameserver / {print $2; exit}' /etc/resolv.conf 2>/dev/null):0
```

实际上就是利用了 WSL2 的特性，Windows 会自动把自己的 IP 地址设为 WSL2 的默认 DNS 服务器。这里取了 `/etc/resolv.conf` 的第一行的 IP 地址作为 DISPLAY 环境变量中 IP 地址的设置。
