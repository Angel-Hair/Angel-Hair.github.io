---
title: Debian下安装JDK+Eclipse和Gnome图标的配置
categories:
 - Java
tags:
 - Eclipse
 - Gnome
 - Debian
 - JDK
 - 操作记录
---

# 前言

最近学校开始java的课程了，虽然要求是JavaSE-1.6的老版本，但是装了最新版后发现自带1.6版本，顿时爽到(=ﾟωﾟ)=

因为是个方便记录，写得粗糙请见谅。

# 安装 JDK

首先`apt-get update`一波，然后执行：

> apt-get install default-jdk

再执行：

> java

查看是否有用法提示，如果有就说明安装成功了。

另外不同发行版的JDK的版本也是不同的（比如我的stretch是openjdk-8-jdk），详情可以参见WIKI：[https://wiki.debian.org/Java](https://wiki.debian.org/Java)

# 安装 Eclipse

到官网([https://www.eclipse.org/downloads/packages/](https://www.eclipse.org/downloads/packages/))找到你需要的Packages，我这里下载的是“Eclipse IDE for Java Developers”。

再将下载的包解压到 /usr 目录下：

> tar -xzvf eclipse-java-photon-R-linux-gtk-x86_64.tar.gz -C /usr

到目录下运行试试

> cd /usr/eclipse

> ./eclipse

如果安装成功，软件就已经打开了。

如果需要汉化的话，到[http://www.eclipse.org/babel/downloads.php](http://www.eclipse.org/babel/downloads.php)下载对应版本的汉化包，解压到软件目录下面就行了。

# 在Gnome下配置Eclipse图标

我现在用的是Gnome桌面（~~曾经我也是个KDE派，直到我遇到了Flat主题(;´Д`)~~），图标配置起来比较麻烦，KDE要好些...

如果只想当前用户可见的话，在 .local/share/applications/ 路径下创建 "eclipse-photon.desktop" 文件。如果想让所有用户都能看见的话在 /usr/share/applications/ 路径下创建。

> su

> cd /usr/share/applications

> nano eclipse-photon.desktop

输入以下内容：
```
[Desktop Entry]
Type=Application
Name=eclipse
GenericName=Integrated development environment
Comment=Configurable and extensible IDE
Exec=/usr/eclipse/eclipse
Icon=eclipse.png
Terminal=false
Categories=Development;IDE;
StartupNotify=true
```
含义如下：
* Name: 程序快捷方式的名称
* Comment: 程序快捷方式的描述
* Exec: 程序可执行文件的路径
* Terminal: 程序执行的方式，true为执行在命令行中，falase则相反
* Type:  程序类型，默认为Application
* Categories: 程序在Application面板中所属的分类，
* StartupNotify: 设置是否现实程序启动和关闭的提示，默认为true
* Icon: 程序图标的路径，如果只填写名字，那么gnome会在 /usr/share/icons 里面寻找这个图片
* GenericName：该数值指定了相关应用程序的属名
* 想了解更多可以参考这篇文章：[https://blog.csdn.net/qq_31811537/article/details/79325123](https://blog.csdn.net/qq_31811537/article/details/79325123)

再将软件目录下自带的那个 "icon.xpm" 文件转换成png图片，并重命名为 "eclipse" 拷贝到 /usr/share/icons 里就可以了。（不知道为什么，我这里只有png格式才能正常显示，大家可以试试别的格式）

如果没有错误的话，此时图标已经显示在列表里了，点开来测试下吧。

至此所有配置已经完毕。