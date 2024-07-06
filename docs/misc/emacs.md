**关于emacs界面视觉优化及中文字体、输入法安装**

**一、emacs界面视觉优化**

原装emacs字体小，颜色单一，使用起来不方便。为解决这一问题，下面整理了一些可操作的方法：

**1. 关于更改字体类型和大小：**
查找与缓存字体类型：输入`apt-cache search fonts`回车，它会给你列很多字体类型。

在终端输入`nano .Xresources`，回车后输入你想要的字体类型和大小，如：`emacs.Font: Inconsolata` 保存后退出即可。（字体资源下一步才会加载进来，这里先写着）【注：.Xresources是一个用于配置Linux系统中一些应用程序的文件，主要用于设置它们的外观（显示设置），通常位于用户的主目录中。】
安装一个配置的工具：`sudo apt install x11-xserver-utils`【注：这是一个包含多种X服务器工具的软件包。（我们需要其中的命令行工具xrdb，它允许用户从指定的文件中读取资源设置，并将它们加载到 X 服务器中。）】从.Xresources中加载资源设置：输入`xrdb -load .Xresources`

然后就可以安装想要的那种字体，如安装的是Inconsolata：输入`sudo apt install fonts-inconsolata`

设置字体：直接在emacs的配置文件.emacs（怎么打开见后文）中写入'(set-face-attribute 'default nil :height 200'，height后面的那个数字是字体大小，可以自行调整。
【注：.emacs的打开方法————输入`nano .emacs`（或者在emacs里打开.emacs也行：输入Crtl+x,Ctrl+f,.emacs）】

**2. 关于字体颜色的选择：**
查找与缓存emacs上的颜色主题：输入`apt-cache search color theme emacs`【注：查找与缓存颜色主题：输入`apt-cache search color theme`】

选一个你喜欢的颜色配置，安装一下，如输入`sudo apt install elpa-solarized-theme`

**3. emacs的其他外观配置（语法高亮的颜色、背景色、前景色）**
打开emacs的配置文件.emacs，如写入`(load-theme 'solarized-light t)`【注：load-theme 是 Emacs Lisp 中的一个函数，用于加载和激活主题；solarized-light是主题的名称；其他的符号是函数语法，如'是表示符号，最后的t代表布尔值为真】，就可以添加想要的颜色主题。

安装主题的选择：可以在 apt 中找。例如，在命令行输入`apt install elpa-color-theme-modern`，里面包含了多个颜色的主题。

**4. emacs上方的任务栏设置：**
前面所说的一系列操作并不会改变任务栏的字体、背景，所以这个部分还要另外操作————在 `.bashrc` 中写入`export GDK_SCALE=2`即可（数字表示字体大小，可以自己调整）。
【注：也可以直接关掉这部分，其实用途不大】

**二、Linux系统中文字体安装：**
Linus系统刚开始使用的时候没有中文字体，直接用emacs打开含有中文的文件会导致出现乱码。直接使用vim（或nano）不会出现这样的问题，是因为vim（或nano）直接在wsl终端运行，而windows上是有中文字体的。此时需要在Linux系统上安装中文字体：`sudo apt install fonts-noto-cjk`【注：noto是一个包含简体中文、繁体中文、日文、日语、韩语等的字体族。】

**三、emacs中的中文输入法安装——pyim**
可以先搜一下有哪些种类：输入`apt-cache search pyim`后，会跳出一些，可以仔细阅读一下。
安装：输入`sudo apt install elpa-pyim`

在emacs内部安装pyim：输入M-x package-install RET pyim RET，可能会出现找不到的情况(no match)，更新一下package-install的库（输入M-x package-refresh-contents），然后用M-x package-list-packages查看可用包列表，搜索一下pyim有关的包名，这个时候应该就可以搜到了，安装完就可以了。

同样要在emacs配置文件中声明：在.emacs文件中写入`(require 'pyim)(require 'pyim-basedict)(pyim-basedict-enable)(setq default-input-method "pyim")`【注：更详细的配置信息可以参见https://github.com/tumashu/pyim】

一个补充：
* 从零开始写配置文件：https://nyk.ma/posts/emacs-write-your-own
* 两个emacs发行版（如果你懒得做配置文件，可以先试一试别人的，当然可能用到最后还是自己写配置文件）：[doom](https://github.com/doomemacs/)和[spacemacs](https://www.spacemacs.org/)