macOS 提供了标准的 POSIX 环境，但没有内置包管理器。因此我们一般会使用第三方的包管理器，推荐程度从高到低：

- Homebrew
- Nix

## 配置 Homebrew

Homebrew 是 macOS 上的包管理器，可以方便地安装各类工具。

详细安装方式，请参考 https://mirrors.tuna.tsinghua.edu.cn/help/homebrew/。

安装后，就可以用 `brew` 命令安装包了：

```shell
brew install xxx
```

由于 `brew` 运行的时候会从 GitHub 上更新仓库，所以这一步可能会卡住，建议使用一些方法来加速 GitHub 访问。

## 配置 Nix

首先按照 <https://nixos.org/download.html> 上的方法，进行 `Multi-user installation`。执行成功以后，就可以在命令行中执行 `nix-env` 命令了。

要安装一个包，可以用命令 `nix-env`：

```shell
nix-env -i xxx
```

可以在 <search.nixos.org> 来搜索想要安装的包的名字。
