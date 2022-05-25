我安装的Linux版本是ubuntu 16.04的英文版的（至于为什么安装英文版的，我最初的想法是平常查询的官方文档都是英文的，使用谷歌搜索出来的东西也基本都是英文的居多，还有许多平常使用的网站都是全英文的，英语水平很重要，所以尽量使自己处于全英文的状态下使得英语水平慢慢提升）。当进入ubuntu中，发现什么都是最初始的，什么都时未设置的，因此平常中需要的很多软件都得需要自己安装。


## 1. 更新软件源：
首先我们需要先更新ubuntu中的软件源，使用以下命令：

```
sudo apt-get install update
```


## 2. System Settings显示：
1) 一开始点击右上角的System Settings是不能够显示出系统设置界面的，因此运行通过以下命令来使其显示：

```
sudo apt-get install unity-control-center
```

2) 如果执行了上面一条命令后点击System Setting还是不能够显示系统设置界面的话请使用一下命令：

```
sudo apt-get install ubuntu-desktop
```

3) 如果上面两条命令都执行了，点击后还是不能显示的话，则需要重启一下，重启后就可以显示了。


## 3. 安装 fcitx：
由于我安装的ubuntu版本一开始是没有中文输入法的，而且我使用的是双拼输入法，因此需要安装fcitx，安装fcitx通过以下命令：

```
sudo apt-get install fcitx-table-wbpy
```

1) 安装完成之后点击右上角的System Setting，在弹出来的系统设置界面中点击Language Support，然后在弹出来的小方框中点击Keyboard input-method system：选择fcitx。操作如下图：

![](./Ubuntu%E4%BB%8E0%E5%BC%80%E5%A7%8B%E5%AE%89%E8%A3%85%E5%BF%85%E9%9C%80%E7%9A%84%E8%BD%AF%E4%BB%B6.assets%5C124242_4cSu_2363215.png)

![](./Ubuntu%E4%BB%8E0%E5%BC%80%E5%A7%8B%E5%AE%89%E8%A3%85%E5%BF%85%E9%9C%80%E7%9A%84%E8%BD%AF%E4%BB%B6.assets%5C124320_KMmZ_2363215.png)

2) 然后为了让右上角的小键盘出现，我们需要关机重启一下ubuntu。

3) 重启之后点击右上角的小键盘  --》 选择configure(配置)  --》点击左下角的 + 号  --》取消Only Show Current Language的选中（也就是未选中状态）  --》  在搜索框中输入 shu  --》  在上面会显示 shuangpin，然后选中它  --》  点击右下角的OK按钮  --》  添加完成后双击shuangpin  --》  然后就可以选择适合自己的双拼输入法的类型了。操作如下：

![](./Ubuntu%E4%BB%8E0%E5%BC%80%E5%A7%8B%E5%AE%89%E8%A3%85%E5%BF%85%E9%9C%80%E7%9A%84%E8%BD%AF%E4%BB%B6.assets%5C124510_KlOb_2363215.png)

![](./Ubuntu%E4%BB%8E0%E5%BC%80%E5%A7%8B%E5%AE%89%E8%A3%85%E5%BF%85%E9%9C%80%E7%9A%84%E8%BD%AF%E4%BB%B6.assets%5C124607_nBzI_2363215.png)

![](./Ubuntu%E4%BB%8E0%E5%BC%80%E5%A7%8B%E5%AE%89%E8%A3%85%E5%BF%85%E9%9C%80%E7%9A%84%E8%BD%AF%E4%BB%B6.assets%5C124800_mUFU_2363215.png)

4) 至于切换输入法的快捷键，在点击System Setting，在弹出的窗口中选择Keyboard。

![](./Ubuntu%E4%BB%8E0%E5%BC%80%E5%A7%8B%E5%AE%89%E8%A3%85%E5%BF%85%E9%9C%80%E7%9A%84%E8%BD%AF%E4%BB%B6.assets%5C124830_U1mT_2363215.png)

![](./Ubuntu%E4%BB%8E0%E5%BC%80%E5%A7%8B%E5%AE%89%E8%A3%85%E5%BF%85%E9%9C%80%E7%9A%84%E8%BD%AF%E4%BB%B6.assets%5C124857_mUd4_2363215.png)

点击了Text Entry之后就可以自己设置切换输入法的快捷键了。



## 4. 安装搜狗输入法：
如果需要安装搜狗输入法，则打开浏览器去搜狗拼音输入法的官网中下载linux版的输入法，至于搜狗输入法官网中的安装指南，可以不需要按照其来。

但我们把搜狗输入法的 deb 软件包下载下来后，双击搜狗输入法的deb软件包进行安装时，可能会报错（“This software comes from a 3rd party and may contain non-free components”），当出现报错时，则终端运行以下命令：

```
sudo apt install gdebi
```


> 安装完成后，找到其软件包右键 --》点击properties --》选择open with  --》 选中GDebi Package Installer --》选择GDebi Package installer --》Set as default

（上面的设置是为了以后安装其他deb软件包是都可以直接双击打开。）然后双击搜狗输入法的安装包即可，安装之后还得自己去添加搜狗输入法，跟上面选中的 shuangpin 一样的操作，在此就不重复说明了。



## 5. 安装flash：
由于在ubuntu下有时候需要看动漫，然后打开默认的火狐浏览器时，一开始时不能看视频的，需要安装flash，运行一下命令即可：

```
sudo apt-get install flashplugin-nonfree
```


运行该命令时，需要下载安装包。如果没有设置代理的话，下载速度会很慢，先做其他事，等过一段时间后它会自动安装完成。



## 6. 同步win7和ubuntu两个系统的时间：
当你安装了双系统
>1）当你在 win7 下显示的是正常时间
>2）关机进入 ubuntu 之后，ubuntu 会自动更换时间，这时 ubuntu 显示的也会是正常时间（一开始可能时间不对，不过等级分钟会自动更新为正常时间）
>3）这时当你关闭 ubuntu 进入 win7 时，win7下显示的时间会比正常时间慢8个小时，然而我们总不能每次去手动更改 win7 下的时间，这样太麻烦了。因此我们需要统一时间，在ubuntu的终端运行以下命令：

```
sudo timedatectl set-local-rtc 1
```


这是为了让Ubuntu不使用UTC时间，从而与Windows保持一致。