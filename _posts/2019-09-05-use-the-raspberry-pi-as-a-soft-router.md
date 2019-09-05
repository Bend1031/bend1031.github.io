---
layout: post
title: 将树莓派用作"软路由"
date: 2019-09-05
author: Bend
header-img: img/post-bg-raspberry_pi.jpg
catalog: true
tags: 
        - 树莓派
---
>无意中在网上发现了将树莓派用作「软路由」的方法，于是亲手操作，完成后记录一下。

## 什么是软路由

在介绍软路由之前，我们先来定义一下「硬路由」

「硬路由」:

>就是我们现在随便在京东淘宝搜索路由器搜出来的，从一开始就是按照路由器规范设计出来的硬件设备。

「软路由」
>利用现有的硬件（可能是电脑或者树莓派之类的）配合软件来实现路由器的功能。

就我而言，我只是简单地将树莓派用作路由器来使用而已。
下面正式介绍安装步骤。

## 安装

### 挑选固件

首先我们要确定要刷入的固件，类似于给电脑装系统。

>OpenWrt 是适合于嵌入式设备的一个 Linux 发行版，我选择的是别人编译的好 OpenWrt 固件，共有两种固件，其中 IPV4 Only 固件适用于 不需要连通 IPV6 网络的情况 （如果你没有连通 IPV6 的需求，IPV4 Only 固件也是推荐选择），IPV4+IPV6 固件适用于需要连通 IPV6 网络的使用情况。

**此固件仅支持树莓派！**

带有 factory 字样的文件为安装固件 ，下载固件到本地并解压即可得到 factory 格式的 img 镜像文件。

带有 sysupgrade 字样的文件为升级固件，下载固件到本地并解压即可得到 sysupgrade 格式的 img 镜像文件。

其中，文件名中带有 ext4 字样的为 ext4 固件，文件名中带有 squashfs 字样的为 squashfs 格式固件。

squashfs 格式的固件适用于 “不折腾” 的用户，其优点是可以比较方便地进行系统还原。
为了简单一点，我们就选择这个固件。
**我们选择带有 squashfs 和 factory 字样的文件进行下载，类似于 openwrt-brcm2708-bcm2709-rpi-2-squashfs-factory.img.gz 这样**

### 下载

固件下载
蓝奏云 （只提供最新版下载）:
32 位 （适用于 2/3B/3B+):

IPV4 Only:

https://www.lanzous.com/b791329

IPV4+IPV6:

https://www.lanzous.com/b791330

### 安装

固件格式不同，但是它们刷入 SD 卡 的方法是一样的，在 Windows 下你可以使用 Win32 Disk Imager 或者 Etcher 将 img 固件写入 SD 卡，在 Linux 下你可以使用 dd 命令写入。
可以看我前面的文章
[树莓派初始设置之安装系统](https://bend1031.github.io/2019/04/15/Raspberry-Pi-initial-setup-installation-system/)

## 初始设置

刷入固件通电开机后，稍等 30 秒 你将可以搜索到一个 SSID 为： Openwrt 的 WIFI 热点，连接此热点，在浏览器地址栏输入：

http://192.168.1.1

即可进入 Luci 控制面板。同时你也可以选择用网线连接树莓派和电脑，在浏览器输入相同的地址来进入控制面板。登陆控制面板时用户名默认为 root，密码默认为 password。

### 网口设置

刷入固件后树莓派的网口默认为 Lan 口，如果你有拨号需求（我校为 PPPoE 拨号上网）或者需要将树莓派设置为子路由的话，需要将树莓派的网口改为 Wan 口，配置方法如下：
**以下内容每做完一步后必须点击 “保存” 而不是 “保存 & 应用”，做完全部步骤之后才可点击右上角的 “未保存的配置” 应用所有修改，否则可能会造成在设置过程中无法连接到树莓派的情况发生**

1. 进入 “网络 - 接口”，点击 “添加新接口”：
![添加新接口](https://raw.githubusercontent.com/Bend1031/PictureBed/master/img/20190905172339.png)

2. 在 “新接口的名称” 中填入 wan（小写），“新接口的协议” 依据具体情况而定，如果要将树莓派的作拨号用，则选择 PPPOE，如果想要用网线与上一设备 （如路由器） 的 Lan 口相连的话则选择 DHCP 客户端，在接口选项中，选择以太网适配器 "eth0"，选择完成后，点击右下角的 “提交”。
![配置新接口](https://raw.githubusercontent.com/Bend1031/PictureBed/master/img/20190905172433.png)

3. 之后在 Lan 接口的 “物理设置” 中修改取消勾选 eth0，点击下方的 “保存” 而不是 “保存 & 应用”
![取消 Lan 口绑定的 eth0 接口](https://raw.githubusercontent.com/Bend1031/PictureBed/master/img/20190905172458.png)

最后点击右上角的 “未保存的配置” 应用所有修改即可。

值得一提的是，如果你是使用网线方式连接电脑和树莓派的话，当你把树莓派的网口改为 Wan 口后，你将无法通过浏览器进入 Luci 控制面板，但是使用无线方式连接到树莓派还是可以正常进入控制面板的，所以当你发现电脑无法进入控制面板后，不要惊慌，拔掉网线连接树莓派的无线热点即可正常进入控制面板～

### WiFi 设置

对于我使用的树莓派 3b+来说，能正常使用的是 radio2 接口， 5Ghz 频段工作正常。建议修改密码，就能正常使用了。

## 参考资料

[软路由搭建攻略：从小白到大白](https://post.m.smzdm.com/p/az59qdwo/)

[自编译 OpenWrt R9.6.19 固件，支持 Raspberry Pi 2B/3B/3B+](https://mlapp.cn/369.html)
