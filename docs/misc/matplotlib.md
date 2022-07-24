## matplotlib 图片中中文无法显示，变成了方块

由于 matplotlib 对中文字体支持的缺陷，我们**不推荐在 matplotlib 中直接使用中文**。具体来说：

- 如果指定一个字体列表，则 matplotlib 只会应用其中第一个可用的字体。[#18883](https://github.com/matplotlib/matplotlib/issues/18883)
- 对于 `.ttc` 字体集，如果包含多种字体，则也将只应用其中的第一种字体。[#3135](https://github.com/matplotlib/matplotlib/issues/3135)

如果要使用中文，可以用如下方式临时解决：

1. 查看系统中的所有中文字体：

    ``` bash
    $ fc-list :lang=zh
    /usr/share/fonts/truetype/droid/DroidSansFallbackFull.ttf: Droid Sans Fallback:style=Regular
    /usr/share/fonts/truetype/SourceHanSerifSC-VF.ttf: Source Han Serif SC VF:style=Regular
    ```

    如果运行结果类似上图，则说明系统中存在中文字体。每一行的格式均为 `字体路径: 字体名称: 字体样式`。

    如果没有得到任何输出，则需要安装中文字体。以“文泉驿正黑”字体为例。执行
    
    ``` bash
    sudo apt install fonts-wqy-zenhei
    ```

    再执行 `fc-list :lang=zh`，即可查看新安装的字体。然后执行如下命令，删除原有的字体缓存目录：

    ``` bash
    rm -r ~/.cache/matplotlib
    ```

    重启 python 解释器，再调用 matplotlib 时会自动载入新的字体。

2. 将中文字体应用到 `matplotlib`。以下均以“文泉驿正黑”举例，**需要将其替换为要设置的字体**。

    - 如果在个别位置使用中文，请查阅函数的文档，并将中文字体名称填入相应参数。通常设置关键字参数 `fontdict={'family': 'WenQuanYi Zen Hei'}` 即可生效。

    - 如果需要将中文字体添加到默认设置，则可以执行如下命令：

        ``` bash
        echo "font.family: WenQuanYi Zen Hei" >> ~/.config/matplotlib/matplotlibrc
        ```

        然后重启 python 解释器，即可将字体应用于 matplotlib 全局。
        
        **此外，由于西文字符也将使用该字体，请在使用该方法时，尽量选用大字符集的字体**，如[思源宋体](https://github.com/adobe-fonts/source-han-serif/blob/master/README-CN.md)。

注意，由于 matplotlib 只支持 `.ttc` 字体集中的第一个字体，因此如果设置不生效，则可以换用同一文件下其他字体再次尝试。