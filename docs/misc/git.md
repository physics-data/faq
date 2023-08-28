## 可以修改 `git push` 之后的 commit 吗

- 不可以。即使有可能，也非常不推荐。
- 如果这样做，会给队友造成很多麻烦。
- 可以加一个新的 commit，说明前一个 commit 哪里错了，是如何修改的。

## Git Clone 时遇到 destination path already exists and is not an empty directory

例子：

```shell
$ git clone git@github.com:physics-data/xxx-xx.git
fatal: destination path 'xxx-xx' already exists and is not an empty directory.
```

这意味着 `xxx-xx` 目录已经存在，可能你之前已经运行过 `git clone`，不需要再 clone。直接 `cd xxx-xx` 进入目录即可。

## Push 的时候出现 Permission denied 错误

Push 的时候，显示 `ERROR: Permission to physics-data/xxx.git denied to xxx` 错误的适合，可能是以下这些情况：

1. 可能是没有配置 SSH Key，请按照 SSH Key 的配置方法进行排查
2. 可能是用错了仓库，一般来说，如果上面显示 `Permission to physics-data/tpl_xxx.git denied`，一般是你在 `git clone` 的时候目标是错误的，应该是 `git clone git@github.com:physics-data/xxx-xxx.git`，会出现你的 GitHub 用户名
3. 可能是出现了拼写错误，请仔细和网页版比对

验证 SSH Key 是否正确：

```shell
$ ssh -T git@github.com
Hi xxx! You've successfully authenticated, but GitHub does not provide shell access.
```

出现上面的消息，就说明 SSH Key 配置成功了。否则，按照下面的步骤配置 SSH Key：

生成 SSH Key（运行 `ssh-keygen`，一路回车，注意，只要 `~/.ssh/id_rsa` 文件存在就不用再次生成），然后把 `~/.ssh.id_rsa.pub` 的内容添加到 GitHub 的 SSH Keys 设置中。

需要注意的是，SSH Key 只需要配置一次即可，它相当于你的 GitHub 帐号的登录凭证。

## Git Clone 时遇到 `destination path already exists and is not an empty directory`

例子：

```shell
$ git clone git@github.com:physics-data/xxx-xx.git
fatal: destination path 'xxx-xx' already exists and is not an empty directory.
```

这意味着 `xxx-xx` 目录已经存在，可能你之前已经运行过 `git clone`，不需要再 clone。直接 `cd xxx-xx` 进入目录即可。

## Git Push 遭遇 Support for password authentication was removed 错误

Push 的时候，显示：
```shell
remote: Support for password authentication was removed on August 13, 2021. Please use a personal access token instead.
remote: Please see https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/ for more information.
```

输入`git remote -v`命令查看，如果输出结果为：
```shell
origin  https://github.com/username/repository.git (fetch)
origin  https://github.com/username/repository.git (push)
```

则需执行 `git remote set-url origin git@github.com:username/repository.git`，然后再次尝试进行 `git push`，注意 `username` 为 Github 用户名，`repository` 为仓库名。

## Push 的时候出现 "port 22: Network is unreachable"

1. 首先检查网络，尝试访问其他网站，例如用浏览器访问 `https://git.tsinghua.edu.cn`，在 WSL 中运行 `ping 101.6.6.6`
2. 如果网络正常，请配置清华的 [SSLVPN](https://deny.tsinghua.edu.cn)

## Push 的时候出现 "setsockopt IPV6_TCLASS 8: Operation not permitted"

这是因为 WSL1 实现不完整的问题。可以忽略，不影响使用。

参考：<https://github.com/microsoft/WSL/issues/1869>

## Push 的时候报错 "failed to push some refs to ...(A default branch (e.g. master) does not yet exist)"

这是git的一些历史遗留问题，可能在挂梯子且设置全局模式后出现。

参考：<https://www.theserverside.com/feature/Why-GitHub-renamed-its-master-branch-to-main>
     <https://sfconservancy.org/news/2020/jun/23/gitbranchname>
     <https://git.tsinghua.edu.cn/physics-data/faq/-/issues/300>

解决方案：把分支名取为‘master’

```
git checkout -b master
git push origin master

```
