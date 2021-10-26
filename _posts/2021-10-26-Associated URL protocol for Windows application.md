---
layout: post
title: 为Windows应用关联URL协议
date: 2021-10-26
author: Bend
header-img: img/post-bg-dog-computer.jpg
catalog: false
tags:
- windows
- Zotero
---

> 最近重装系统之后，很多应用不能正常使用，其中就包含 Zotero。例如这么一个链接 zotero://open-pdf/library/items/74LG8GIM ，以前我是直接点击这个链接就能打开 Zotero ，并且打开相应文献。为了解决这个问题，有了本文。

其实对于这么一个链接 zotero://open-pdf/library/items/74LG8GIM ，其中`zotero`就是协议的名称。在 Windows 上要注册一个URL协议并用相应的应用程序打开需要操作注册表。

在 Windows 中打开注册表，找到`HKEY_LOCAL_MACHINE\Software\Classes`或者`HKEY_CURRENT_USER\Software\Classes` ,然后在里面添加一个项目，以 Zotero为例

![image-20211026202505448](https://cdn.bend1031.top/img/image-20211026202505448.png)

新建一个`zotero`项目后，将第一个默认值改为`URL:zotero`,然后新建一个字符串值，命名为`URL Protocol`。其中zotero为协议的名称，`URL Protocol`,则表明这是一个协议。这样就完成了协议的定义，但是知道这是协议之后还需要用对应的程序打开才行，因此需要设置程序的路径。

在zotero这个项目上右键，选择新建项，然后按图中依次设置`Shell`、`Open`、`Command` 三个项，点击`Command`,然后修改其默认的值为`"E:\Engirneering\Zotero\zotero.exe" -- "%1"` 。其中“E:\Engirneering\Zotero\zotero.exe” 为我电脑上zotero程序的路径，其中路径后面的 `"%1"` 是文件资源管理器传入的参数，其实就是文件的完整路径。

![image-20211026203417528](https://cdn.bend1031.top/img/image-20211026203417528.png)

这样一来，当我点击一个 zotero://open-pdf/library/items/74LG8GIM 这样的链接，或者用浏览器打开个链接时

![image-20211026203852489](https://cdn.bend1031.top/img/image-20211026203852489.png)

就会出现这样的提示了：

![image-20211026203925652](https://cdn.bend1031.top/img/image-20211026203925652.png)

## 参考

[如何为你的 Windows 应用程序关联 URL 协议，以便在浏览器中也能打开你的应用](https://blog.walterlv.com/post/windows-uri-scheme-association.html)

[心之所想、一键直达：你可能不知道的 Windows 快捷方式玩法](https://sspai.com/post/68718)

