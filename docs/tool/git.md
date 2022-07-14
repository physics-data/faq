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

首先安装 SSH 客户端：

```shell
sudo apt install openssh-client
```

接着，运行 `ssh-keygen`，一路按回车，会生成密钥：

- `~/.ssh/id_rsa`: 私钥，需要保密
- `~/.ssh/id_rsa.pub`: 公钥，可以公开

## Git.Tsinghua

### 配置 Git.Tsinghua 的 SSH Key

1. 登录 <https://git.tsinghua.edu.cn/>
2. 点击右上角的头像，选择 `Preferences`
3. 在左边栏中，点击 `SSH Keys`
4. 在 Linux 终端中运行 `cat ~/.ssh/id_rsa.pub`
5. 把输出的内容里，以 `ssh-rsa` 打头的一行，完整复制
6. 粘贴到第 4 步的浏览器页面中
7. 向 Title 的编辑框内随便写入一个名字
8. 点击 `Add Key` 按钮
9. 可以看到下面的 `Your SSH keys` 多了一项
10. 在 Linux 终端中运行 `ssh -T git@git.tsinghua.edu.cn`
11. 如果出现了 `yes/no` 的提示，输入 `yes` 并回车
12. 如果配置成功了，它会显示形如 `Welcome to GitLab, @xxx!` 的提示信息

### 克隆 Git.Tsinghua 仓库到本地

1. 浏览 Git.Tsinghua 仓库
2. 点击右侧的 `Clone` 按钮
3. 点击 `Clone with SSH` 下面文本框右边的复制按钮
4. 此时剪贴板应该是 `git@git.tsinghua.edu.cn` 开头的一串内容
4. 回到 Linux 终端，输入 `git clone 剪贴板内容`，回车执行

如果提示文件夹已存在，可能你已经克隆过了，直接 `cd 目录名` 就可以进入仓库的目录。

## GitHub

### 配置 GitHub 的 SSH Key

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

### 克隆 GitHub 仓库到本地

1. 浏览 GitHub 仓库
2. 点击右侧的 `Code` 按钮，点一下 `Use SSH`
3. 复制下面的文本框的内容，它应该是 `git@github.com` 开头的一串内容
4. 回到 Linux，输入 `git clone 剪贴板内容`，回车执行

如果提示文件夹已存在，可能你已经克隆过了，直接 `cd 目录名` 就可以进入仓库的目录。