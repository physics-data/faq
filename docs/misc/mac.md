## cut 在 macOS 上行为与 Linux 上不同的问题

`cut` 有不同的版本，在 Linux 上常用的是 GNU cut，来自 coreutils，而 macOS 自带的 cut（位于 `/usr/bin/cut`）则是 BSD cut。它们之间可以接受的参数有相同的也有不同的。

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

这时候，它为了不和自带的 `cut` 发生冲突，把 GNU cut 命名称为了 `gcut`。这时候要按照上面的描述，通过修改 `PATH` 环境变量，把 `cut` 从原来的 `/usr/bin/cut` 换成 `/usr/local/opt/coreutils/libexec/gnubin/cut` 。但要注意，这种做法可能会让一些依赖于 BSD cut 的程序运行失败

类似的还有：

1. 安装 GNU awk：`brew install gawk` 并添加相应的 PATH 环境变量
2. 安装 GNU sed：`brew install gnu-sed` 并添加相应的 PATH 环境变量
3. 安装 GNU find：`brew install findutils` 并添加相应的 PATH 环境变量
4. 安装 GNU make：`brew install make` 并添加相应的 PATH 环境变量
5. 安装 GNU grep：`brew install grep` 并添加相应的 PATH 环境变量

总结一下，可以用下面的命令来配置 PATH 环境变量：

```shell
export PATH="$(brew --prefix)/opt/coreutils/libexec/gnubin:$PATH"
export PATH="$(brew --prefix)/opt/gawk/libexec/gnubin:$PATH"
export PATH="$(brew --prefix)/opt/gnu-sed/libexec/gnubin:$PATH"
export PATH="$(brew --prefix)/opt/findutils/libexec/gnubin:$PATH"
export PATH="$(brew --prefix)/opt/make/libexec/gnubin:$PATH"
export PATH="$(brew --prefix)/opt/grep/libexec/gnubin:$PATH"
```

确认相关的命令行工具确实是 GNU 版本：

```shell
wc --version | grep GNU
cut --version | grep GNU
awk --version | grep GNU
sed --version | grep GNU
find --version | grep GNU
make --version | grep GNU
grep --version | grep GNU
```

如果每个命令的版本信息都有 `GNU`，就说明配置正确了。

## 怎么安装带有 guile 支持的 make？

```shell
brew tap jiegec/formulas
brew install make-guile
export PATH="$(brew --prefix)/opt/make-guile/libexec/gnubin:$PATH"
```

## Arm Debian 包管理

参考 `gcc(1)`，Arm 平台上的 GOT 大小为 28k，超过后需要使用 `-fPIC`。在下载编译 R 包时可能会遇到此问题。

```
-fpic
  Generate position-independent code (PIC) suitable for use in a shared library,  if
  supported  for  the  target  machine.   Such  code accesses all constant addresses
  through a global offset table (GOT).  The dynamic loader resolves the GOT  entries
  when  the program starts (the dynamic loader is not part of GCC; it is part of the
  operating system).  If the GOT size for the linked executable exceeds  a  machine-
  specific  maximum  size,  you get an error message from the linker indicating that
  -fpic does not work; in that case, recompile with -fPIC instead.  (These  maximums
  are  8k on the SPARC, 28k on AArch64 and 32k on the m68k and RS/6000.  The x86 has
  no such limit.)
```

解决办法：

修改下列选项为 `-fPIC`：

```
$ cat /etc/R/Makeconf | grep fpic
CPICFLAGS = -fpic
CXXPICFLAGS = -fpic
CXX11PICFLAGS = -fpic
CXX14PICFLAGS = -fpic
CXX17PICFLAGS = -fpic
CXX20PICFLAGS = -fpic
FPICFLAGS = -fpic
```
