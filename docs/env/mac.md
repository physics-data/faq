# macOS 环境配置

macOS 提供了较为标准的 POSIX 环境，但属于 BSD 流派，与 GNU 有微妙不好查觉的偏差，而且没有内置包管理器。在 macOS 上建立 GNU 环境，分为虚拟机、包管理器与双系统启动三种，虚拟机适合初学者，包管理器适合对系统软件研发感兴趣的同学，双系统启动适合不喜欢 macOS 但要用 mac 硬件的同学。

## 虚拟机

macOS 上的虚拟机软件中，同时满足开源自由、性能好、易用三点的工作为[UTM](https://mac.getutm.app/)。

### UTM

[UTM](https://mac.getutm.app/) 基于 [QEMU](https://www.qemu.org/) 开发，支持 amd64 和 arm64 指令集的硬件加速和图形加速，性能较好。

下载[UTM.dmg](https://github.com/utmapp/UTM/releases/latest/download/UTM.dmg)并安装，启动程序后，创建一台虚拟机。从 [TUNA 清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/) 下载 [amd64](https://mirrors.tuna.tsinghua.edu.cn/debian-cd/current/amd64/iso-cd/debian-12.6.0-amd64-netinst.iso) 或 [arm64](https://mirrors.tuna.tsinghua.edu.cn/debian-cd/current/arm64/iso-cd/debian-12.6.0-arm64-netinst.iso) 的安装光盘文件，挂载到新创建的虚拟机中。

启动虚拟机，按提示安装 Debian GNU/Linux 系统。

## 第三方包管理器

第三方的包管理器基于 macOS 的原生环境构建出一套 GNU 的子环境，与系统的集成好。但由于 Apple 公司经常改动系统级别的 API ，这些方案都不太稳定，建议对 macOS 有深入理解并且决心在 macOS 上进行研发的同学尝试。以下推荐程度从高到低：

- Gentoo Prefix
- Homebrew
- Nix
- MacPorts

### Gentoo Prefix

[Gentoo Prefix](https://wiki.gentoo.org/wiki/Project:Prefix) 是当前由续本达领导开发的用户态基于 Gentoo Linux 的通用 GNU 环境。它的安装需要编译 Python 和 Portage（Gentoo 的包管理器）和 GCC，需要的时间较长，因此请保证计算机的供电。

如果从前没有在 macOS 上编译过程序，则需要安装苹果的 XCode 编译器。
```shell
xcode-select --install
xcode-select --switch /Library/Developer/CommandLineTools
```

接下来到[General Boostrap Process](https://wiki.gentoo.org/wiki/Project:Prefix/Bootstrap) 页面的 "Bootstrapping Prefix" 一节，安提示下载 `bootstrap-prefix.sh`。

打开 macOS 的终端命令行环境，执行下面的命令：
```shell
curl -O https://gitweb.gentoo.org/repo/proj/prefix.git/plain/scripts/bootstrap-prefix.sh
chmod +x bootstrap-prefix.sh
```

在运行安装之前，指定清华 TUNA 的镜像可以加速网络连接。
```
export GENTOO_MIRRORS=http://mirrors.tuna.tsinghua.edu.cn/gentoo
export GNU_URL=http://mirrors.tuna.tsinghua.edu.cn/gnu
./bootstrap-prefix.sh
```

询问的问题一路采用默认值即可。如果一切顺利，编译完成后，会出现如图所示的信息

![bootstrap 成功](prefix-success.png)

系统在 `~/Gentoo` 目录，它称为 `EPREFIX` 目录，意为 `portage` 的 prefix。在之下启动 Gentoo Prefix：

```shell
~/Gentoo/startprefix
```

这样就配好了 Gentoo Prefix 环境，之后可以用 `emerge` 命令来安装软件。例如

```shell
emerge -avt git
```

#### 可能遇到的问题及解决方案

##### DSO support routines 报错

运行 stage3 安装时，多位同学遇到因网络问题造成的 `Failed to emerge sys-apps/some_package` 错误。具体错误信息如下：

从 gitweb.gentoo.org/artfiles.org/mirrorservice.org 等下载渠道报错为：

```shell
8678637888:error:25FFF06C:DSO support routines:CRYPTO_internal:functionality not supported:dso/dso_lib.c:226:
8678637888:error:0EFFF06E:configuration file routines:CRYPTO_internal:error loading dso:conf/conf_mod.c:273:module=providers, path=providers
8678637888:error:0EFFF071:configuration file routines:CRYPTO_internal:unknown module name:conf/conf_mod.c:214:module=providers
```

请先确认自己的网络稳定。可以重新运行 `./bootstrap-prefix.sh` 来确认问题是否能复现。

如果确实不是网络问题，首先查看 portage 下载用的命令是什么：

```shell
$ ~/Gentoo/usr/bin/portageq envvar FETCHCOMMAND
wget -t 3 -T 60 --passive-ftp -O "${DISTDIR}/${FILE}" "${URI}"
```

显示它使用的是 wget，但是如果系统里没有自带 wget，就会使用到刚编译的不带 HTTPS 支持的 wget。

接下来参考 [emerge via curl or wget?](https://forums.gentoo.org/viewtopic-t-306884.html) 做以下的尝试：

首先，确认 curl 命令的位置，这里 `-a` 是 all 的意思：

```shell
which -a curl
```

如果返回路径中存在 `/usr/bin/curl`，则在 `/Users/username/Gentoo/etc/portage/make.conf` 中（注意把 username 替换成自己的账户名称）添加下面两行：

```shell
# The necessary code to make portage use curl instead of wget
FETCHCOMMAND="/usr/bin/curl --connect-timeout 15 -# -o \${DISTDIR}/\${FILE} \${URI}"
RESUMECOMMAND="/usr/bin/curl --connect-timeout 15 -# -C - -o \${DISTDIR}/\${FILE}\${URI}" 
```

之后运行命令：

```shell
~/Gentoo/usr/bin/portageq envvar FETCHCOMMAND
```

确认配置是否生效。如果返回值中 `wget` 命令替换为 `curl`，则说明配置生效。重新运行 `./bootstrap-prefix.sh`，确认问题是否解决。

##### 进入 Gentoo 环境后仍然调用非 Gentoo 环境中的 Python

首先，如果在 VSCode 等编辑器中出现这种情况，可以通过设置默认 python 默认解释器的方法来解决。而对于在终端中调用 python 的场景（例如通过 make 执行 python 脚本），解决的方式会相对复杂一些。

这种问题出现的原因往往是之前安装过 Homebrew 或 Conda，导致`.zshrc`, `.bashrc`等涉及默认路径的配置文件被修改，系统优先调用该环境中的 python。

进入 Gentoo 环境后，`grep -nH PATH .bash_profile .bashrc .zshrc .zprofile` 可以大致确定哪些文件中增加了额外的 PATH，例如可能出现

```
.zprofile:3:# Set PATH, MANPATH, etc., for Homebrew.
```

这意味着之前安装 Homebrew 时`.zprofile`被修改，之后`code .zprofile`打开之，将涉及 Homebrew 的部分删除即可。

需要注意的是，这可能会导致之后在 Homebrew 或 Conda 环境调用 python 时出现一些问题，请谨慎操作。

此外，还有一种可能的解决方案，是将

```
alias python="/Users/ere.lihy/Gentoo/bin/python3"
alias python3="/Users/ere.lihy/Gentoo/bin/python3"
```

加入到`.zprofile`，`bash_profile`等文件中，但是该方案无法改变 make 语句的调用目标。

具体的讨论参见：
- https://git.tsinghua.edu.cn/physics-data/faq/-/issues/97
- https://git.tsinghua.edu.cn/physics-data/faq/-/issues/132 
- https://git.tsinghua.edu.cn/physics-data/faq/-/issues/275 

### 配置 Homebrew

Homebrew 是 macOS 上的包管理器，可以方便地安装各类工具。

#### 安装 Homebrew

详细安装方式，请参考 https://mirrors.tuna.tsinghua.edu.cn/help/homebrew/。

简单来说，首先打开终端，然后按顺序执行下列这些命令：

```shell
# 设置 Homebrew 镜像
export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git"
export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git"
export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles"

# 从本镜像下载安装脚本并安装 Homebrew
git clone --depth=1 https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/install.git brew-install
/bin/bash brew-install/install.sh

# 更新 homebrew
brew update

# 替换仓库上游
for tap in core cask{,-fonts,-drivers,-versions} command-not-found; do
    brew tap --custom-remote --force-auto-update "homebrew/${tap}" "https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-${tap}.git"
done
brew update
```

中途可能会询问你的密码以使用 root 权限，还有回车以确认进行安装操作。

#### 使用 Homebrew

安装后，就可以用 `brew` 命令安装包了：

```shell
brew install xxx
```

由于 `brew` 运行的时候会从 GitHub 上更新仓库，所以这一步可能会卡住，建议使用一些方法来加速 GitHub 访问。

#### 配置 python 环境

安装 brew 以后，可以用 brew 来安装 python 环境：

```shell
brew install python3
python3 --version
```

如果安装成功了，应该可以看到 Python 3.9 或者更加新的版本。

接下来，就可以用 pip 来安装包了：

```shell
pip3 install matplotlib numpy
python3 -c 'import matplotlib;import numpy'
```

如果第二条命令导入的时候没有出错，就说明安装成功了。

### 配置 Nix

首先按照 <https://nixos.org/download.html> 上的方法，进行 `Multi-user installation`。执行成功以后，就可以在命令行中执行 `nix-env` 命令了。

要安装一个包，可以用命令 `nix-env`：

```shell
nix-env -i xxx
```

可以在 <https://search.nixos.org> 上搜索想要安装的包的名字。

### 配置 MacPorts

助教不使用这个环境，如果有同学要使用的话，可以自己摸索。

# 双系统启动
安装新系统可能因误操作损失数据，请注意备份已有数据。具体作法在此暂不讨论，遇到困难请联系助教。
