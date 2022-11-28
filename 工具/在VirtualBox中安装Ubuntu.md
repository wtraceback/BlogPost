每次更换电脑，需要安装虚拟机的时候，都要去搜别人的教程，干脆自己把安装过程记录下来，方便后面的参考。

## （一） 下载 Ubuntu
我使用的是 ubuntu 16
- ubuntu 官网下载地址：
https://releases.ubuntu.com/
- ubuntu-16.04.7-desktop-amd64 官网下载地址：
https://releases.ubuntu.com/16.04.7/
- 下载示意图
![](在VirtualBox中安装Ubuntu.assets/1下载Ubuntu.png)

## （二） 安装前的设置
1. 点击【新建】
![](在VirtualBox中安装Ubuntu.assets/2新建.png)
2. 设置【名称】、【安装的目录】、【类型】、【版本】
![](在VirtualBox中安装Ubuntu.assets/3设置安装目录.png)
3. 分配内存：1GB 或 2GB
![](在VirtualBox中安装Ubuntu.assets/4分配内存.png)
4. 选择【现在创建虚拟磁盘】
![](在VirtualBox中安装Ubuntu.assets/5创建虚拟硬盘.png)
5. 创建虚拟硬盘 【VDI（VirtualBox磁盘映像）】
![](在VirtualBox中安装Ubuntu.assets/6VirtualBox磁盘映像.png)
6. 选择【动态分配】
![](在VirtualBox中安装Ubuntu.assets/7虚拟硬盘的动态分配.png)
7. 创建虚拟磁盘的大小：10GB 或 20GB
![](在VirtualBox中安装Ubuntu.assets/8创建虚拟硬盘的大小.png)
8. 选择虚拟盘 --> ubuntu的镜像文件
![](在VirtualBox中安装Ubuntu.assets/9选择ubuntu镜像.png)
![](在VirtualBox中安装Ubuntu.assets/10选择镜像后的截图.png)

## （三） 在 VirtualBox 中启动并安装 ubuntu
1. 启动
![](在VirtualBox中安装Ubuntu.assets/11启动.png)
2. 选择语言，点击【安装 Ubuntu】
![](在VirtualBox中安装Ubuntu.assets/12选择中文并安装ubuntu.png)
3. 勾选【安装 Ubuntu 时下载更新】
![](在VirtualBox中安装Ubuntu.assets/13安装ubuntu时下载更新.png)
4. 勾选【清除整个磁盘并安装 Ubuntu】
清除的是之前创建的虚拟磁盘里面的数据，而不是安装目录所在的磁盘
![](在VirtualBox中安装Ubuntu.assets/14清除虚拟磁盘.png)
5. 继续
![](在VirtualBox中安装Ubuntu.assets/15写入虚拟磁盘的确认.png)
![](在VirtualBox中安装Ubuntu.assets/16选择所在地.png)
![](在VirtualBox中安装Ubuntu.assets/17键盘布局选择汉语.png)
6. 账户设置
![](在VirtualBox中安装Ubuntu.assets/18设置账户信息.png)
7. 等待安装完成即可
![](在VirtualBox中安装Ubuntu.assets/19安装中1.png)
![](在VirtualBox中安装Ubuntu.assets/20安装中2.png)
。。。

## （四） 增强功能：VirtualBox-Ubuntu-主机和虚拟机实现互相复制粘贴
[VirtualBox-Ubuntu-主机和虚拟机实现互相复制粘贴](https://www.cnblogs.com/wanghuizhao/p/16923829.html)