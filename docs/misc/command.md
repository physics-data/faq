## 运行 `~/.bashrc` 显示 `Permission denied`

这个文件里面写的是 bash 启动的时候自动运行的命令，它本身不是直接执行的。如果要让 .bashrc 的内容立即生效，应该用 `source ~/.bashrc`。

## matplotlib 显示 `UserWarning: Matplotlib is currently using agg, which is a non-GUI backend` 然后不显示窗口

需要安装 `python3-tk`：

```shell
sudo apt install python3-tk
```

这是因为如果没有安装 `python3-tk`，matplotlib 找不到图形界面库，就无法显示图形界面。

## matplotlib 显示 "Couldn't find foreign struct converter for 'cairo.Context'"

运行如下命令：

```shell
sudo apt update
sudo apt install python-gi-cairo
```

参考文档：<https://github.com/rbgirshick/py-faster-rcnn/issues/221>

## 运行 `ssh-keygen` 以后找不到 `~/.ssh/id_rsa.pub`

运行 `ssh-keygen` 的时候，它会问你生成的的路径等问题，这时候不要输入内容（姓名、学号等等），直接一路回车即可。


## 运行 `ssh-keygen` 显示找不到命令 `command not found`

请安装 `openssh-client`：

```shell
sudo apt install openssh-client
```

## 命令行下面中文显示不正常，变成了豆腐块

请修改一下字体。例如，对于命令提示符，可以在标题栏右键，找到字体设置，然后设置为中文字体。