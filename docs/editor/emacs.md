## Emacs
在 Debian 中，可以安装 `emacs` 软件包，或者安装 `emacs-gtk` 以获得更多功能。

- 保存：C-x C-s
- 退出：C-x C-c

### 界面优化

在 WSL 安装 Emacs 字体小，颜色单一，使用起来不方便。为解决这一问题，下面整理了一些可操作的方法：

#### 更改字体类型和大小
查找与缓存字体类型：输入`apt-cache search fonts`回车，它会给你列很多字体类型。

在终端输入`nano .Xresources`，回车后输入你想要的字体类型和大小，如：`emacs.Font: Inconsolata` 保存后退出即可。（字体资源下一步才会加载进来，这里先写着）

`.Xresources` 是一个用于配置Linux系统中一些应用程序的文件，主要用于设置它们的外观（显示设置），通常位于用户的主目录中。

安装一个配置的工具：`sudo apt install x11-xserver-utils` 这是一个包含多种 X11 服务器工具的软件包。我们需要其中的命令行工具xrdb，它允许用户从指定的文件中读取资源设置，并将它们加载到 X 服务器中。从 `.Xresources` 中加载资源设置：`xrdb -load .Xresources`。

安装想要的那种字体，如安装的是 `Inconsolata`：输入`sudo apt install fonts-inconsolata`。

设置字体：直接在 emacs 的配置文件 `.emacs` 中写入 `(set-face-attribute 'default nil :height 200)`，height后面的那个数字是字体大小，可以自行调整。

`.emacs` 的打开方法————输入`nano .emacs`或者在 emacs 里打开 `.emacs` 输入 Crtl+x, Ctrl+f, .emacs 。

#### 选择字体颜色
查找与缓存 emacs 上的颜色主题：输入`apt-cache search color theme emacs`。

选一个你喜欢的颜色配置并安装，输入`sudo apt install elpa-solarized-theme`。

#### 其他外观配置（语法高亮的颜色、背景色、前景色）
打开 emacs 的配置文件.emacs，如写入`(load-theme 'solarized-light t)`，就可以添加想要的颜色主题。

`load-theme` 是 Emacs Lisp 中的一个函数，用于加载和激活主题；solarized-light是主题的名称；其他的符号是函数语法，如'是表示符号，最后的t代表布尔值为真。

安装主题的选择：可以在 apt 中找。例如，在命令行输入`apt install elpa-color-theme-modern`，里面包含了多个颜色的主题。

#### 上方的任务栏设置
前面所说的一系列操作并不会改变任务栏的字体、背景，所以这个部分还要另外操作————在 `.bashrc` 中写入`export GDK_SCALE=2`即可。数字表示字体大小，可以自己调整。也可以直接关掉任务任，其实用途不大。

### Linux系统中文字体安装
Linux 系统刚开始使用的时候没有中文字体，直接用 emacs 打开含有中文的文件会导致出现乱码。直接使用vim（或nano）不会出现这样的问题，是因为vim（或nano）直接在wsl终端运行，而windows上是有中文字体的。此时需要在Linux系统上安装中文字体：`sudo apt install fonts-noto-cjk`。noto是一个包含简体中文、繁体中文、日文、日语、韩语等的字体族。

### 中文输入法安装——pyim
可以先搜一下有哪些种类：输入`apt-cache search pyim`后，会跳出一些，可以仔细阅读一下。安装，`sudo apt install elpa-pyim`。

如果 Debian 自带的 `pyim` 不够新，可在 emacs 内部安装 pyim，但这样可能会造成与系统软件的冲突，长久稳定性低，不建议。输入M-x package-install RET pyim RET，可能会出现找不到的情况(no match)，更新一下package-install的库 输入 `M-x package-refresh-contents`，然后用 `M-x package-list-packages` 查看可用包列表，搜索一下pyim有关的包名，这个时候应该就可以搜到了。

同样要在emacs配置文件中声明：在.emacs文件中写入`(require 'pyim)(require 'pyim-basedict)(pyim-basedict-enable)(setq default-input-method "pyim")`。更详细的配置信息可以参见 https://github.com/tumashu/pyim 。

### 参考资料
* 从零开始写配置文件：https://nyk.ma/posts/emacs-write-your-own
* 如果你懒得做配置文件，可以先试一试别人的，当然可能用到最后还是自己写配置文件。两个emacs发行版为：[doom](https://github.com/doomemacs/)和[spacemacs](https://www.spacemacs.org/)。
