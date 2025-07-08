## 运行 `~/.bashrc` 显示 `Permission denied`

这个文件里面写的是 bash 启动的时候自动运行的命令，它本身不是直接执行的。如果要让 .bashrc 的内容立即生效，应该用 `source ~/.bashrc`。

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

## 我使用kill命令终结任务失败怎么办？

`kill`与`fg`、`bg`等命令不同，前者是系统级的命令，不仅可以处理shell内的任务，也可以直接处理系统中的进程PID，而后者只能处理shell内的任务，所以使用`kill`命令时需要注明需要终结的是shell中的任务序号（`jobs`命令显示的序号如`[1]`）还是系统中的进程PID（`ps`命令或`jobs -p`显示的PID）。对象为shell中的任务时，使用`kill %1`（其中`%1`为任务序号），对象为系统中的进程时，使用`kill 1234`（其中`1234`为进程PID）。在处理进程PID时，可能需要使用`sudo`权限，若进程依然无法终结，可以尝试使用 `kill -9`命令强制终结。