## 安装 SSH Server

如果你使用的是 WSL，一般不需要 SSH Server；如果你使用的是虚拟机，并且想用 VSCode 通过 SSH 访问虚拟机，则需要安装和配置 SSH Server。

如果使用的是 Debian/Ubuntu，可以按照下面的命令安装 openssh server：

```shell
sudo apt install openssh-server
```

## 启动 SSH Server

接着，按照下面的命令启动 SSH Server，并设置每次启动的时候都会启动：

```shell
sudo systemctl start ssh
sudo systemctl enable ssh
```

## 如何检查 SSH Server 正在运行

第一种方法：寻找正在运行的 sshd 进程，运行下面的命令并且观察是否有输出：

```shell
ps aux | grep sshd
```


第二种方法：查看 sshd 是否监听了 TCP 22 端口，观察有没有 sshd：

```shell
sudo lsof -i tcp -nP | grep "22 (LISTEN)"
```

第三种方法：用 systemctl 命令查看服务状态，是否是 running 状态：

```shell
sudo systemctl status ssh
```
