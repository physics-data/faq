下面是一些课程会用到的软件的安装方式。如果你使用了我们预先构建的 WSL 镜像，则这些软件都已经安装在系统中。

## 常用 Python 包安装方式

通常来说，你可以使用系统的包管理器（`apt`）或者 Python 自带的 `pip` 工具来安装常见的 Python 包，后者通常更新更及时，但需要手动执行更新操作。

无论使用哪种方式安装，请不要同时使用两种方式安装同一个包，否则可能带来意想不到的麻烦。

| 软件包           | Ubuntu/Debian                           | macOS(Homebrew)                                              |
| ---------------- | --------------------------------------- | ------------------------------------------------------------ |
| hdf5             | sudo apt install hdf5-tools             | brew install hdf5                                            |
| matplotlib       | sudo apt install python3-matplotlib     | pip3 install matplotlib                                      |
| vitables         | sudo apt install vitables python3-pyqt5 | 不支持，建议使用 [HDFView](https://www.hdfgroup.org/downloads/hdfview/#download) |
| ipython          | sudo apt install python3-ipython        | pip3 install ipython                                         |
| h5py             | sudo apt install python3-h5py           | pip3 install h5py                                            |
| numpy            | sudo apt install python3-numpy          | pip3 install numpy                                           |
| jupyter notebook | sudo apt install jupyter                | pip3 install notebook                                        |

## 常用软件安装方式

| 软件包  | Ubuntu/Debian            | macOS(Homebrew)      |
| ------- | ------------------------ | -------------------- |
| python3 | sudo apt install python3 | brew install python3 |
| git     | sudo apt install git     | brew install git     |

