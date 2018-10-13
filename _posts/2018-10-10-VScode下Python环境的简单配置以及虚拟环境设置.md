---
title: VScode下Python环境的简单配置以及虚拟环境设置
categories:
 - Python
tags:
 - Visual Studio Code
 - Python
 - 虚拟环境
 - 操作记录
---

# 前言

因为项目组的需要要学python了，虽然老师强烈推荐pycharm，但是现在电脑上已经有许多IDE了，真的不想装再多了_(:зゝ∠)_ 我怕装多了电脑环境出问题，于是就想着还是用vscode吧。虽然后来要求要搭虚拟环境，又吹了一波pycharm，但是我回头一想，vscode好像也可以来着...就立马回去试了下，还真就成了(=ﾟωﾟ)= 感觉这波操作可以有(~~我可能会忘记(´∀((☆ミつ~~)，于是就想写下来了。

方便记录罢了，如有纰漏见谅。

~~顺便说下vscode今天更新了下，我TM吹爆(´∀((☆ミつ~~

# VScode下Python环境的简单配置

## 简单配置

其实主要还是安装插件啦_(:зゝ∠)_

首先把下面的几个插件装好(第一个是必须的，其他两个是我推荐的(=ﾟωﾟ)=)：
1、Python

![Python](\img\2018\8.PNG)

2、AREPL for python

![AREPL for python](\img\2018\9.PNG)

3、Python Extension Pack

![Python Extension Pack](\img\2018\10.PNG)

前两个还好，一般来讲直接就可以装上，但最后一个Python Extension Pack因为微软引入了刚发布的发布 Python 语言服务器([Python Language Server](https://www.oschina.net/news/98235/introducing-the-python-language-server))，可能会报错，所以我们需要在用户设置里添加以下代码：

> "python.jediEnabled": false,  //指示是使用Jedi作为IntelliSense引擎（true）还是Microsoft Python语言服务器（false）

如果需要单独安装Python Language Server，直接`pip install python-language-server`就行了。

## 额外的配置

另外如果需要更加个性化的配置，"settings.json"文件里的"Python Configuration"项里列出了所有的自定义项及其简单介绍，可以按需把其中的配置项移到用户设置里修改(这样可以在不改变全局配置的情况下覆盖原有设置)，更加详细的说明可以参考官方的文档:[https://code.visualstudio.com/docs/python/editing](https://code.visualstudio.com/docs/python/editing)

![Python Configuration](\img\2018\11.PNG)

# 虚拟环境的设置

首先，进入vscode设置里面，在搜索栏搜索"python.venv"，在出现的代码里选中"python.venvPath"和"python.venvFolders"这两个设置项，并拖到用户设置里进行配置。

![python venv](\img\2018\12.PNG)

这两个设置项的意义如下：

* "python.venvPath"：放置python虚拟环境的根目录
* "python.venvFolders"：根目录下的虚拟环境的文件夹名

例如我需要在"D:\workpace\python\virtual_environment"目录下配置一个名为"test"虚拟环境的文件夹可以参考下面的配置：

```json
{
    //以下是python虚拟环境的设置
    "python.venvPath": "D:\\workpace\\python\\virtual_environment", //这里是windows下的目录设置，linux用户按照常规设置。
    "python.venvFolders": [
        "test",
    ],
}
```

接下来我们需要到根目录下去搭建一个虚拟环境，python3及其以上版本可以用venv，python3以下的可以用virtualenv。

接上一个例子：

1、venv(python3自带)

执行以下命令：

> cd /d D:\workpace\python\virtual_environment\test  
> python -m venv .

Linux下的命令和上面windows的差不多，读者可以自行判别。

2、virtualenv

virtualenv可以参考这篇文章：[https://www.cnblogs.com/yixuetang/p/8359856.html](https://www.cnblogs.com/yixuetang/p/8359856.html) 这里就不再详细描写了(~~突然变懒，逃|∀ﾟ~~)。

搭建好了以后就可以重启vscode测试了。

比如需要在项目中切换到虚拟环境，只需要点击左下角的图标：
![left](\img\2018\13.PNG)
然后再弹出的上方窗口选择要切换的虚拟环境：
![up](\img\2018\14.PNG)
如果切换成功的话，这时左下角的图标就已经改变了：
![left-change](\img\2018\15.PNG)

如果没有图标也可以使用命命令行 ctrl +p 然后输入 python:select interpreter

至此，所有的配置已完毕。