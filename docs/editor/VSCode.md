## VSCode

使用 Visual Studio Code 进行代码编写和工作。它具有大量的插件，也内建了 Git 的支持。

### 环境准备

如果你使用 macOS 或者 Linux，正常情况下无需特别配置。

如果你使用 macOS 发现无法通过 `code` 指令运行 VSCode，你可以在 VSCode 中通过 `Cmd+Shift+P` 快捷键打开命令面板（Command Palette），然后输入 `shell command` 找到 `Shell Command: Install 'code' command in PATH` 并运行。其他问题可以参考[官方教程](https://code.visualstudio.com/docs/setup/mac)。

如果你使用 Linux 系统，各种问题也可以参考[官方教程](https://code.visualstudio.com/docs/setup/linux)。

如果你使用 Windows，请参照 [官方教程](https://code.visualstudio.com/docs/remote/wsl-tutorial) 安装 WSL 支持。

如果你在 Windows 上使用 Linux 虚拟机，请参照 [官方教程](https://code.visualstudio.com/docs/remote/ssh-tutorial) 安装 SSH 支持，并连接到你的虚拟机。

### 插件安装

请在从你的 **工作环境**（如 WSL / Remote SSH）启动的 VSCode 中，安装 Python 插件。注意安装在 Windows 中的插件不起作用。

打开对应的文件夹开始使用。

### 插件安装 FAQ
在 VSCode 中搜索插件时，出现错误提示“Error while fetching extentions. XHR failed”：

* 可能你正在科学上网。这种情况下可以关闭你的梯子，并刷新网络设置。参考：https://zhuanlan.zhihu.com/p/614288766
* 可能你的 VSCode 版本太老了。从左下角的设置按钮点击“检查更新”，并按照提示安装新版本。

### Python 交互窗口

[Jupyter](https://jupyter-notebook.readthedocs.io/en/latest/)（以前称为 IPython Notebook）是一个开源项目，可将 Markdown 文本和可执行 Python 源代码组合在一个称为笔记本的画布上。在安装 Jupyter 包后，在工作环境中的VSCode中安装 Python 和 Jupyter 插件，可以使用类似于 Jupyter Notebook 的功能。 

在 Debian 系的系统中，推荐使用以下命令安装 Jupyter：
```bash
sudo apt install python3-ipykernel
```

#### Jupyter 代码单元

使用 `# %%` 注释在 Python 代码(`.py`)中定义类似 Jupyter 的代码单元：
```python
# %%
msg = "Hello World"
print(msg)

# %%
msg = "Hello again"
print(msg)
```
当 Python 扩展检测到代码单元时，它会添加 `Run Cell`(`运行单元格`) 和 `Debug Cell`(`调试单元格`) 的 Code Lens 装饰。第一个单元格还包括 `Run Below`(`运行以下`) ，所有后续单元格都包括 `Run Above`（`运行以上`） 。

点击 `Run Cell`(`运行单元格`) 按钮或使用快捷键 `Ctrl+Enter`(运行后停留在本单元格) 或者 `Shift+Enter`(运行后随即进入下一个单元格) 来运行代码单元。初次选择Kernel之后，就可以在右侧的交互窗口中运行相应的单元格。

![Python 交互窗口](./python_interactive.png)

#### 使用 Python 交互窗口

在 VSCode 中，按下 `Ctrl+Shift+P` ，输入 `Jupyter：Create Interactive Window`（`Jupyter：创建交互式窗口`） 。这样可以直接启动一个 Python 交互窗口，在最下方输入 Python 代码并查看输出。

相关内容可以参考：https://code.visualstudio.com/docs/datascience/python-interactive

#### 在 VSCode 中使用 Jupyter Notebook

安装 Jupyter 包后，可以在VSCode中打开`.ipynb`文件，使用 Jupyter Notebook 的功能。

参考：https://code.visualstudio.com/docs/datascience/jupyter-notebooks
