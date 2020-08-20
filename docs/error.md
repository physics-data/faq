## Git Clone 时遇到 `destination path alread exists and is not an empty directory`

例子：

```
$ git clone git@github.com:physics-data/xxx-xx.git
fatal: destination path 'xxx-xx' already exists and is not an empty directory.
```

这意味着 `xxx-xx` 目录已经存在，可能你之前已经运行过 `git clone`，不需要再 clone。直接 `cd xxx-xx` 进入目录即可。

## 访问 GitHub 时遇到 `Permission denied (publickey)`

请生成 SSH Key（`ssh-keygen`，注意，只要 `~/.ssh/id_rsa` 文件存在就不用再次生成），然后把 `~/.ssh.id_rsa.pub` 的内容添加到 GitHub 的 SSH Keys 设置中。设置好以后，再 `ssh -T git@github.com` 进行测试，如果输出：

```
Hi xxx! You've successfully authenticated, but GitHub does not provide shell access.
```

那说明配置成功了。

需要注意的是，SSH Key 只需要配置一次即可，它相当于你的 GitHub 帐号的登录凭证。