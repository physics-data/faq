## cut 在 macOS 上行为与 Linux 上不同的问题

`cut` 有不同的版本，在 Linux 上常用的是 GNU cut，来自 coreutils，而 macOS 自带的 cut（位于 `/usr/bin/cut`） 则是 BSD cut。它们之间可以接受的参数有相同的也有不同的。

为了解决这个问题，可以在 macOS 上也安装 coreutils：

```bash
$ brew install coreutils
...
==> Caveats
Commands also provided by macOS have been installed with the prefix "g".
If you need to use these commands with their normal names, you
can add a "gnubin" directory to your PATH from your bashrc like:
  PATH="/usr/local/opt/coreutils/libexec/gnubin:$PATH"
```

这时候，它为了不和自带的 `cut` 发生冲突，把 GNU cut 命名称为了 `gcut`，你可以选择：

1. 把所有 `cut` 出现的地方改成 `gcut`
2. 按照上面的描述，通过修改 `PATH` 环境变量，把 `cut` 从原来的 `/usr/bin/cut` 换成 `/usr/local/opt/coreutils/libexec/gnubin/cut` 。但要注意，这种做法可能会让一些依赖于 BSD cut 的程序运行失败
