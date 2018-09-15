---
title: BANDWAGONHOST VPS搭建SHADOWSOCKS SERVER及BBR算法优化加速的记录
categories:
 - 服务器
tags:
 - 科学上网
 - shadowsocks
 - bandwagonhost
 - 操作记录
---

# 前言

这篇文章是我在刚上大学时因为要查找各种资料，不得不搭建ss服务器时的方便记录，写得十分粗糙请见谅。
**另外，请不要将文章中的知识用于越界法律的研究！**

<!-- more -->

# 第一步：买好你的VPS

参考 https://zhuanlan.zhihu.com/p/33887156 选择了Bandwagonhost主机商，购买参考：http://www.cnhawkit.com/328.html。
参考 https://zhuanlan.zhihu.com/p/36121716，决定领取github student pack，用digitalocean主机商，尚未找到配置参考。

~~发现领取github student pack太麻烦，Bandwagonhost刚好在打折，还是用第一种吧（逃）。~~

# 第二步：查找ss服务器配置的相关文章

相关文章列表（部分内容需要翻墙！）：
* https://zhuanlan.zhihu.com/p/31513383
* https://zhuanlan.zhihu.com/p/36121716
* https://www.diycode.cc/topics/738
* https://banwagong.cn/ovz-kvm.html
* http://banwagong.cn/kvm-install-bbr.html
* https://www.bandwagonhost.net/
* https://github.com/easonhuang123/blog/issues/1
* https://moshuqi.github.io/2017/07/20/%E8%87%AA%E5%B7%B1%E6%90%AD%E5%BB%BAVPN%E6%9C%8D%E5%8A%A1%E5%99%A8/
* https://blog.csdn.net/allfun/article/details/53350214

# 第三步：简单配置

在买好主机后（这里我买的是KVM架构，数据中心选的是洛杉矶QNET，因为采用了亚洲优化路线），登录到KiwiVM管理面板，准备把系统改成Ubuntu（~~没错，朱军，我喜欢debian系！⊂彡☆))∀`)~ 因为我最近一直在用Xbuntu，比较熟悉嘛。）

![菜单栏](..\img\2018\1.png)
![管理面板](..\img\2018\2.png)

首先在主管理界面点击“STOP”关机，然后通过左侧“Install new OS”进入系统选择页面，选择你想要改成的系统，并勾选“I agree that…”

![系统选择](..\img\2018\3.png)

成功后会转到提示，并会给出系统自动生成的root密码和SSH端口（同时还会发送相关邮件给你）。

![邮件](..\img\2018\4.png)

KiwiVM虽然自带shell网页端，但用起来不怎么方便，为了方便操作，换用Xshell连接服务器。安装好Xshell后，点击左上角“新建”，在弹出的对话框依次填入VPS商在邮件里给出信息（值得一提的是Bandwagonhost指定了SSH端口，请填写正确；如果vps商没有指定，默认的22端口就行了）。

![Xshell](..\img\2018\5.png)
![Xshell](..\img\2018\6.png)

当你填好后，进入模拟终端界面了，输入指令`apt-get update`，我这里遇到了`dpkg was interrupted, you must manually run 'dpkg --configure -a' to correct the problem.`的报错，删掉/var/lib/dpkg/lock以及/var/lib/dpkg/updates/*，再重新update就好了。

![Shell](..\img\2018\7.png)

# 第四步：配置SHADOWSOCKS SERVER

先执行：
> apt-get install build-essential  
发现依赖关系报错。。。执行`apt-get -f install`后再重新执行命名就好了。
然后执行：
> apt-get install python-pip  
再装上ss：
> pip install shadowsocks  
接下来配置文件执行：
> nano /etc/shadowsocks.json  
编辑文件输入以下内容：
```
{
"server":"my_server_ip",
"server_port": 1234,
"local_address": "127.0.0.1",
"local_port": 1080,
"timeout":300,
"password": "yourpassword",
"method":"aes-256-cfb",
"fast_open":true,
"workers":1
}
```
含义如下：

* server：vps的IP地址 
* server_port：服务器监听的端口，一般设为80，443等，注意不要设为使用中的端口 
* password：设置密码，自定义 
* timeout：超时时间（秒） 
* method：加密方法，可选择 “aes-256-cfb”, “rc4-md5”等等。“rc4-md5” 速度快些，但“aes-256-cfb”密度大。
* fast_open：true 或 false。如果你的服务器 Linux 内核在3.7+，可以开启 fast_open 以降低延迟。 
* workers：workers数量，默认为 1。

另外，若是多用户模式，将server_port和password合并为port_password，如下：
```
"port_password": {
         "443": " mypassword 1”, 
         "8888": " mypassword 2”
},
```
填完保存后，接下来设置ss开机自启，执行：
> nano /etc/rc.local  
在文档最后加入以下代码：
> ssserver -c /etc/shadowsocks.json -d start  
然后可以reboot试一下了。

另外前端启动执行：
>ssserver -c /etc/shadowsocks.json  
后端启动，执行：
启动：
>ssserver -c /etc/shadowsocks.json -d start  
关闭：
>ssserver -c /etc/shadowsocks.json -d stop  

# 第五步：BBR算法优化

因为在Google在Kernel 4.9内核上加入BBR算法，所以需要将服务器内核更新为4.9以上，输入：`uname –r`查看当前系统内核。我这里的内核是4.4的版本，所以需要更新更新方法如下：
到Ubuntu网站 http://kernel.ubuntu.com/~kernel-ppa/mainline/ 查找所需Ubuntu内核版本目录，如：http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.16/linux-image-4.16.0-041600-generic_4.16.0-041600.201804012230_amd64.deb ，然后输入：
> wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.16/linux-image-4.16.0-041600-generic_4.16.0-041600.201804012230_amd64.deb  
安装内核执行：
> dpkg -i linux-image-4.*.deb  
删除旧内核（可选）：
> dpkg -l | grep linux-image  
> apt-get purge 旧的内核  
更新grub系统引导文件并重启：
>update-grub  
>reboot  
开机后再次确认版本。
执行`lsmod | grep bbr`，如果结果中没有“tcp_bbr”的话就先执行：
>modprobe tcp_bbr  
>echo "tcp_bbr" >> /etc/modules-load.d/modules.conf  
>echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf  
>echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf  
>sysctl -p  
>sysctl net.ipv4.tcp_available_congestion_control  
>sysctl net.ipv4.tcp_congestion_control  
如果这两个输出都有“bbr”，则你的内核已经开启了bbr，执行`lsmod | grep bbr`看到有“tcp_bbr”模块即说明 bbr 已启动。

至此所有的配置已完毕。

# 第六步：客户端测试

各类客户端下载地址：https://shadowsocks.org/en/download/clients.html