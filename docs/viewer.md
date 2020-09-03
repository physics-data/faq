## Vitables: HDF5 文件查看器

### Debian Buster Vitables 无效

在 Debian Buster 出现以下错误：

```
vitables hz.h5                
Creating the Query results file...

Traceback (most recent call last):
  File "/usr/share/vitables/vitables/h5db/dbDoc.py", line 115, in openH5File
    h5file = tables.openFile(self.filepath, self.mode)
AttributeError: 'module' object has no attribute 'openFile'


Please, if you think it is a bug, report it to developers.

File creation failed due to unknown reasons!
Please, have a look to the last error displayed in the logger. If you think it's a bug, please report it to developers.
```

这是因为 Debian Buster 中的 vitables 版本过低，维护失误。解决方案是将其升级。

1. 创建 `/etc/apt/apt.conf.d/07repo`，内容为
```
APT {
        Default-Release "buster";
}
```

2. 在 `/etc/apt/sources.list` 里加入 `Debian Sid` 源
```
...
deb http://mirrors.tuna.tsinghua.edu.cn/debian/ sid main non-free contrib
...
```

3. `apt install vitables/sid`

注意，此时安装的 vitables 是来自 Debian Sid 的 3.0 版本，并非 Debian Buster 中的 2.1 版本。会有如下输出：

```
...
Selected version '3.0.0-1.1' (Debian:unstable [all]) for 'vitables'
```

4. 确认 vitables 的版本

```
$ vitables --version
vitables 3.0.0
```

### Vitables 无法调用 PyQt5

在某些系统比如 Ubuntu 20.04 LTS 中，执行 vitables 会出现 `PySide2` 无法找到的错误。但经过分析，其根本原因为 PyQt5 无法被正常调用，因为 `/usr/lib/x86_64-linux-gnu/libQt5Core.so.5` 的 `ABI-tag` 与 WSL 的内核不兼容。

解决方法是使用 `strip` 把不兼容的部分去掉。以 `root` 用户执行，

```
strip --remove-section=.note.ABI-tag /usr/lib/x86_64-linux-gnu/libQt5Core.so.5
```

参考：https://github.com/microsoft/WSL/issues/3023
