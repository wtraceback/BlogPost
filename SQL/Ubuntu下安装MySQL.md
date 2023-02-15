### 1. 安装 MySQL 服务器
```
$ sudo apt-get install mysql-server
```

### 2. 查看 MySQL 的版本
```
$ mysql --version
```

### 3. 启动 MySQL 数据库服务
```
$ sudo service mysql start
或
$ sudo systemctl start mysql.service
```

### 4. 停止 MySQL 数据库服务
```
$ sudo service mysql stop
或
$ sudo systemctl stop mysql.service
```

### 5. 查看 MySQL 运行状态
```
$ sudo service mysql status
或
$ sudo systemctl status mysql.service
```

### 6. MySQL 的初始密码
由于我使用 ```sudo apt-get install mysql-server``` 命令安装的，自动安装了 MySQL 8.0 版本
```
# 第一次进入 MySQL
$ sudo mysql -uroot


# 修改 root 的密码
# 示例：mysql> alter user 'root'@'localhost' identified with mysql_native_password by '123456';
mysql> alter user 'root'@'localhost' identified with mysql_native_password by '新密码';


# 退出 MySQL
mysql> exit
或
mysql> quit


# 修改密码之后再次进入 MySQL
$ sudo mysql -uroot -p123456
```
