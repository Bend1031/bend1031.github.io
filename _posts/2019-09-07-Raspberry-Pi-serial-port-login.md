---
layout: post
title: 树莓派串口登录
date: 2019-09-07
author: Bend
header-img: img/post-bg-raspberry_pi.jpg
catalog: true
tags:
    - 树莓派
---

在承接久远的上文 [树莓派初始设置之安装系统](https://bend1031.github.io/2019/04/15/Raspberry-Pi-initial-setup-installation-system/), 今天我来写一下安装完系统后怎么进行登录。

工具：

* 树莓派 3B+

* 笔记本电脑
  
* 电源线（ **电源适配器官方说明为 5V 2.5A，而使用 USB 转 TTL 转换器，供电时只有 5V 1A** ）

* 内存卡、读卡器一枚。
  
* 手机热点/wifi
  
* USB 转 TTL 转换器一个，杜邦线（母口对母口）四根。

补充：
树莓派红灯为电源指示灯，闪烁则说明供电不足或不正常，正常为常亮。绿灯为工作灯，闪烁为正常。

强调：**不能同时使用串口供电和充电口供电**

建议：用电源线供电，不使用串口供电

## 文件修改

要让树莓派使用串口登录，首先我们先用读卡器连接电脑，修改
boot 文件夹下的 config.txt 文件，在最后一行加上

```
dtoverlay=pi3-miniuart-bt
```

保存退出。

## 串口连接

USB TO TTL 模块：
![USB TO TTL 模块](https://raw.githubusercontent.com/Bend1031/PictureBed/master/img/usb_to_ttl1.jpg)

我们需要连接好 TXD RXD GND 三条线，然后将其连接到树莓派的 GPIO 口上。

![树莓派 3B+ 管脚图](https://raw.githubusercontent.com/Bend1031/PictureBed/master/img/Image.png)

连线方法：

| USB TO TTL 模块 | 树莓派管脚 |
| TXD             | RXD        |
| RXD             | TXD        |
| GND             | GND        |

也就是只需要树莓派上的 6，8，10 三个管脚。

然后在电脑上的设备管理器查看端口

![](https://raw.githubusercontent.com/Bend1031/PictureBed/master/img/20190907150002.png)

这里我们可以看到是 COM3

然后打开 Putty

![](https://raw.githubusercontent.com/Bend1031/PictureBed/master/img/20190907150236.png)

最后点 Open，进入后**先回车**，就可以输入用户名和密码了。

默认账号名 pi 密码：raspberry。

注意：**输入密码的时候不会显示出来，就跟没有输入一样。只管输入按回车即可。**

## 参考资料

[树莓派 3b：TTL 串口登录问题](https://blog.csdn.net/feitingfj/article/details/56046188)

[树莓派 3B+（无显示器）实现串口登录](https://blog.csdn.net/sm16111/article/details/95631108)
