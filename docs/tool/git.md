## Git 安装与配置

如果是 Debian/Ubuntu 系列的发行版：

```shell
sudo apt update
sudo apt install -y git
```

如果是 macOS：

```shell
brew install git
```

也可以用 macOS 自带的 git 版本。

安装完，可以用 `git --version` 确认安装成功：

```shell
$ git --version
git version 2.x.x
```

此后，请使用下面的命令配置 git 身份：

```shell
git config --global user.name "Your Name" # 通常可以是自己名字，或者 ID
git config --global user.email "name@example.com" # 请使用注册 GitHub 的邮箱，以便 GitHub 识别你的身份
```

## 生成 SSH Key

运行 `ssh-keygen`，一路按回车，会生成密钥：

- `~/.ssh/id_rsa`: 私钥，需要保密
- `~/.ssh/id_rsa.pub`: 公钥，可以公开

## 配置 GitHub 的 SSH Key

1. 打开 GitHub 帐号
2. 点击右上角的头像，选择 `Settings`
3. 点击 `SSH and GPG keys`
4. 点击 `New SSH Key` 绿色按钮
5. 在 Linux 系统中运行 `cat ~/.ssh/id_rsa.pub`
6. 把输出的内容里，以 `ssh-rsa` 打头的一行，完整复制
7. 粘贴到第 4 步的浏览器页面中，点击 `Add SSH Key` 按钮
8. 在 Linux 系统中运行 `ssh -T git@github.com`
9. 如果出现了 `yes/no` 的提示，输入 `yes` 并回车
10. 如果配置成功了，它会显示形如 `Hi xxx! You've successfully authenticated, but GitHub does not provide shell access.` 的提示信息

## 克隆 GitHub 仓库到本地

1. 浏览 GitHub 仓库
2. 点击右侧的 `Code` 按钮，点一下 `Use SSH`
3. 复制下面的文本框的内容，它应该是 `git@github.com` 开头的一串内容
4. 回到 Linux，输入 `git clone 剪贴板内容`，回车执行

如果提示文件夹已存在，可能你已经克隆过了，直接 `cd 目录名` 就可以进入仓库的目录。

## 可以修改 `git push` 之后的 commit 吗

- 不可以。即使有可能，也非常不推荐。
- 如果这样做，会给队友造成很多麻烦。
- 可以加一个新的 commit，说明前一个 commit 哪里错了，是如何修改的。

## Push 的时候出现 Permission denied 错误

Push 的时候，显示 `ERROR: Permission to physics-data/xxx.git denied to xxx` 错误的适合，可能是以下这些情况：

1. 可能是没有配置 SSH Key，请按照 SSH Key 的配置方法进行排查
2. 可能是用错了仓库，一般来说，如果上面显示 `Permission to physics-data/tpl_xxx.git denied`，一般是你在 `git clone` 的时候目标是错误的，应该是 `git clone git@github.com:physics-data/xxx-xxx.git`，会出现你的 GitHub 用户名
3. 可能是出现了拼写错误，请仔细和网页版比对

## Git Clone 时遇到 `destination path already exists and is not an empty directory`

例子：

```bash
$ git clone git@github.com:physics-data/xxx-xx.git
fatal: destination path 'xxx-xx' already exists and is not an empty directory.
```

这意味着 `xxx-xx` 目录已经存在，可能你之前已经运行过 `git clone`，不需要再 clone。直接 `cd xxx-xx` 进入目录即可。

## Git Push 遭遇 Support for password authentication was removed 错误

Push 的时候，显示：
```
remote: Support for password authentication was removed on August 13, 2021. Please use a personal access token instead.
remote: Please see https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/ for more information.
```

输入`git remote -v`命令查看，如果输出结果为：
```
origin  https://github.com/username/repository.git (fetch)
origin  https://github.com/username/repository.git (push)
```

则需执行`git remote set-url origin git@github.com:username/repository.git`，然后再次尝试进行`git push`，注意`username`为 Github 用户名，`repository`为仓库名。