## 运行 `~/.bashrc` 显示 `Permission denied`

这个文件里面写的是 bash 启动的时候自动运行的命令，它本身不是直接执行的。如果要让 .bashrc 的内容立即生效，应该用 `source ~/.bashrc`。

## matplotlib 显示 `UserWarning: Matplotlib is currently using agg, whcih is a non-GUI backend` 然后不显示窗口

需要安装 `python3-tk`：

```shell
sudo apt install python3-tk
```

这是因为如果没有安装 `python3-tk`，matplotlib 找不到图形界面库，就无法显示图形界面。