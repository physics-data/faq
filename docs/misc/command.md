## 运行 `~/.bashrc` 显示 `Permission denied`

这个文件里面写的是 bash 启动的时候自动运行的命令，它本身不是直接执行的。如果要让 .bashrc 的内容立即生效，应该用 `source ~/.bashrc`。