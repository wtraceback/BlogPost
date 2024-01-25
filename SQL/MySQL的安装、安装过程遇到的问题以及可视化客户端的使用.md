### 1. 下载安装 MySQL
**从 MySQL 官网下载最新的 MySQL Community Server 版本：**
https://dev.mysql.com/downloads/mysql/


### 2. 安装操作
我下载的是 ZIP 包的文件，因此需要下列的安装操作
```
a. 解压
b. 配置环境变量
c. 自定义一个 my.ini 文件
d. 以管理员的身份运行 cmd，然后切换到对应的目录下，执行安装 MySQL 执行
```

#### （1） 将下载好的 MySQL ZIP 包解压到自己想要放置的目录中

#### （2） 配置环境变量
我的电脑 --> 属性 --> 高级系统设置 --> 环境变量 --> 系统变量 --> Path --> 将解压后的 MySQL 里的 bin 目录的绝对路径添加到环境变量中

#### （3） 创建一个名为 my.ini 的配置文件，存放在 MySQL 安装的根目录（跟 bin 目录同级），文件格式如下：
- my.ini 配置文件里面的内容如下：
```
[client]
# 设置 MySQL 客户端默认字符集
default-character-set=utf8mb4
[mysqld]
# 设置 3306 端口
port = 3306
# 设置 mysql 的安装目录
basedir=C:\\web\\mysql-8.0.20
# 设置 mysql数据库的数据的存放目录，MySQL 8+ 不需要以下配置，系统自己生成即可，否则有可能报错
# datadir=C:\\web\\sqldata
# 允许最大连接数
max_connections=200
# 服务端使用的字符集默认为 utf8mb4
character-set-server=utf8mb4
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
```
- 需要修改配置文件内容里面的两个目录的路径：
a. *一个是解压后的 MySQL 的路径（basedir 设置为解压后的 MySQL 在自己电脑上的目录）*
b. *一个是需要放置 MySQL 数据相关的路径（MySQL 8+ 版本不需要设置）*

#### （4） 以管理员的身份运行 cmd 命令行工具，然后通过命令行切换到 MySQL 解压后的 bin 目录里面
```
# 1. 执行安装命令：
mysqld install

# 2. 初始化数据库：（执行完成后，会输出 root 用户的初始默认密码）
mysqld --initialize --console

# 3. 启动 MySQL 服务器命令：（如果不初始化，有可能在启动时存在启动不了的问题）
net start mysql

# 3.1 如果运行 net start mysql 提示“发生系统错误 2。找不到指定文件”，则依次运行下面的命令：
    mysqld --remove
    mysqld --install
    net start mysql

# 4. 连接到本机的 MySQL 服务器
mysql -u root -p
输入上面初始化过程中生成的临时密码（第二步），如果一切正确，就会连接到 MySQL 服务器，同时提示符变为 mysql>

# 5. 退出 MySQL 命令行
mysql> exit
或
mysql> quit

# 6. 修改密码
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
# mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';

# 7. 停止 MySQL 服务器命令
net stop mysql
```

### 3. 安装 MySQL 过程中可能遇到的问题
**在安装 MySQL 8 的时候，报了一个错误：**
    无法启动此程序，因为计算机中丢失 VCRUNTIME140_1.dll。尝试重新安装改程序以解决问题。

- 解决方法：
a. 去微软官网下载最新的2015-2019vc++支持包:
https://support.microsoft.com/zh-cn/help/2977003/the-latest-supported-visual-c-downloads
b. 下载安装 VC_redist.x64.exe

- 问题所在：
a. 在 C:\Windows\System32 目录中不存在 VCRUNTIME140_1.dll 文件
b. 安装完成后，就可以在 C:\Windows\System32 中找到文件 VCRUNTIME140_1.dll

### 4. MySQL 客户端 图形化管理工具（开源免费）
**HeidiSQL    （有 安装版 和 免安装版）**
https://www.heidisql.com/download.php
