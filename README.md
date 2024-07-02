 FAQ (答疑)

本仓库通过 issue 的形式，用于回答课程相关的问题、分享心得。

Issue 列表: https://git.tsinghua.edu.cn/physics-data/faq/-/issues

## 使用方法

以下操作方案基于 Gitlab 的浏览器客户端。使用命令行客户端 `glab` 的同学可以使用 `glab --help` 等资料自行探索使用方案。
Debian 上 `glab` 包可由 `sudo apt install glab` 安装。

### 提出新问题：开 issue
0. 在提问前做充足调研。看看 [docs](docs) 下的文档、已有的 issues 是否已经解决你的问题。
1. 进入 https://git.tsinghua.edu.cn/physics-data/faq/-/issues/new
2. 为你的问题起合适的标题，填入标题栏，并在描述框中详细阐述问题
3. 如果你希望 issue 的任何进展能邮件通知 (Cc) 某位用户，可以在 issue 描述/回复中 @用户名 。特别的，可以使用 `/assign @用户名` 指定负责人
4. 你可以使用 `/due` 命令来设置 issue 的 deadline
5. 不妨考虑为 issue 添加 label 进行分类

不止是问题可以提 issue. 事实上这就是一个公告板，你可以把任何觉得需要告诉给大家的内容张贴。

### 讨论问题
- 你可以直接在网页的 issue 页面讨论问题。回复别人的话语时记得引用，例如
```
Alice: 

xxxxxx

Bob:

> xxxxxxx

yyyyy
```
- 如果你有其他渠道的讨论，请尽可能同步到 issue 里。

### 解决问题：关闭 issue
在解决问题后，请及时使用 `/close [描述]` 关闭 issue. 如果你的某个 commit 解决了某个 issue, 请直接在 commit message 末添加。

```
Closes: https://git.tsinghua.edu.cn/physics-data/faq/-/issues/<id>
```

这样 push 后该 issue 会被自动关闭。当然，一个 commit 可能部分解决，或者与某个 issue 有关，你可以使用

```
Bug: https://git.tsinghua.edu.cn/physics-data/faq/-/issues/<id>
```
来引用。
