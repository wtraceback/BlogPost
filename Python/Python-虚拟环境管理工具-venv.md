### 1. 概述
Python 应用程序通常会使用不在标准库内的软件包和模块。应用程序有时需要特定版本的库，因为应用程序可能需要修复特定的错误，或者可以使用库的过时版本的接口编写应用程序。

这意味着一个 Python 安装可能无法满足每个应用程序的要求。如果应用程序 A 需要特定模块的 1.0 版本但应用程序 B 需要 2.0 版本，则需求存在冲突，安装版本 1.0 或 2.0 将导致某一个应用程序无法运行。

这个问题的解决方案是创建一个 virtual environment，一个目录树，其中安装有特定 Python 版本，以及许多其他包。

然后，不同的应用将可以使用不同的虚拟环境。 要解决先前需求相冲突的例子，应用程序 A 可以拥有自己的 安装了 1.0 版本的虚拟环境，而应用程序 B 则拥有安装了 2.0 版本的另一个虚拟环境。 如果应用程序 B 要求将某个库升级到 3.0 版本，也不会影响应用程序 A 的环境。
### 2. 虚拟环境 venv 模块
一般来说，Python3 是内置了 venv 模块的，如果没有默认安装 venv 模块，则需要通过操作系统的包管理工具手动安装：
```
# 在 ubuntu 下
$ sudo apt install python3-venv
```
### 3. 创建虚拟环境
```
$ python -m venv env  # Windows
```
或
```
$ python3 -m venv env  # Linux 和 macOS
```

说明：
上述命令中，最后一个参数是虚拟环境名称，你可以自由定义，比如 venv、env，或是“项目名-venv”，这里使用了 env。这会在当前目录创建一个包含 Python 解释器环境的虚拟环境文件夹，名称为 env。
### 4. 激活虚拟环境
Windows 下激活虚拟环境：
```bash
$ env\Scripts\activate  # Windows

# 在 Windows 中使用 Git Bash，则需要使用 . env/Scripts/activate 命令
```


Linux 或 MacOS 下激活虚拟环境：
```bash
$ . env/bin/activate  # Linux 或 macOS
```

激活虚拟环境后，可以使用 ```pip``` 命令来管理包
### 5. 退出虚拟环境
```
(env) $ deactivate
```