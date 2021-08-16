## 安装 SSH Server

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