由于在ubuntu（ubuntu 16.04）下写博客时需要录制 gif 格式的图片与截图，因此需要录屏工具，录屏以及截图一些操作如下：

## 1. 命令行安装软件：
```
sudo apt-get install kazam

sudo apt-get install  mplayer

sudo apt-get install imagemagick
```


## 2. 使用 kazam 来录制视频：
按一下键盘的 win 键（也就是一个 windows 的图标的键），然后输入 kazam，点击该软件，就可以通过它来录制视频了。该软件可以截图也可以录制 mp4 格式的视频。


## 3. 将 mp4 格式转化为 gif：
这个过程分为两步：
####（1）将 mp4 格式转化为 jpg 格式的图片;
在终端下切换到刚刚存放的 mp4 格式的地方，然后在该文件夹下创建一个 tabs 文件夹用以存放等下生成的 jpg 图片。

将刚刚生成的 tabs.mp4 在 tabs文件夹中生成 jpg 图片：
> 输入命令：
> mplayer -ao null tabs.mp4 -vo jpeg:outdir=./tabs

####（2）将生成的 jpg 格式的图片转化为 gif 格式的图片。
将 tabs 文件夹中的 jpg 格式的图片转化为 gif 格式的图片：
> convert ./tabs/*.jpg view.gif

在当前目录下生成 view.gif 。这时就已经是 gif 格式的图片了。

## 4. 效果图：
![效果图](./Ubuntu%E4%B8%8B%E6%88%AA%E5%9B%BE%E4%B8%8E%E5%BD%95%E5%88%B6%E8%A7%86%E9%A2%91%E5%B9%B6%E4%BD%9C%E6%88%90gif%E5%9B%BE%E7%89%87.assets/25194027_iDoi.gif)