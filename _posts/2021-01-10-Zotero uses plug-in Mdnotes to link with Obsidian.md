---
layout: post
title: Zotero 利用插件 Mdnotes 与 Obsidian 联动
date: 2021-01-10
author: Bend
header-img: img/post-bg-zotero-obsidian.jpg
catalog: false
tags:
- Zotero
- Obsidian
---
> [Mdnotes](https://github.com/argenos/zotero-mdnotes)  是为 Zotero 创建 Markdown 笔记的一个插件。
>
> Github [最新下载链接](https://github.com/argenos/zotero-mdnotes/releases/download/0.1.2/mdnotes-0.1.2.xpi)

## 安装 Mdnotes 插件

插件下载下来之后是一个后缀为 xpi 的文件，在 Zotero 中选中工具 - 插件

![image-20210110164909762](https://cdn.jsdelivr.net/gh/Bend1031/PictureBed/img/image-20210110164909762.png)

然后点击齿轮按钮 - Install Add-on From File

<img src="https://cdn.jsdelivr.net/gh/Bend1031/PictureBed/img/image-20210110165016701.png" alt="image-20210110165016701" style="zoom:80%;" />

找到刚刚下载的插件，确定之后，重启 Zotero 即可。

## Mdnotes 设置

在工具中找到 Mdnotes preferences

![image-20210110165230252](https://cdn.jsdelivr.net/gh/Bend1031/PictureBed/img/image-20210110165230252.png)

按下图配置，其中第一个 `Use the items citekey as title?` 勾选后的效果是将文献的 citekey 作为 markdown 笔记的标题，而产生这个 citekey 需要下载另一个插件 [zotero-better-bibtex](https://github.com/retorquere/zotero-better-bibtex) ，其 [下载地址](https://github.com/retorquere/zotero-better-bibtex/releases/download/v5.2.107/zotero-better-bibtex-5.2.107.xpi) ，安装方法与 Mdnotes 相同。

<img src="https://cdn.jsdelivr.net/gh/Bend1031/PictureBed/img/image-20210110165330803.png" alt="image-20210110165330803" style="zoom:80%;" />

然后是 `Internal Links`, 如果想与 Obsidian 联动，一定要选择 `[[Wiki style]]` , 若是只想用普通 Markdown 语法记笔记，则可以选择第三项 `[Markdown] style (markdown-style)`。

`Export directory` 选择你文献笔记要放置的地方，记得选择 Obsidian 库的路径。

`Template folder` 选择笔记模板 ** 文件夹 ** 的位置，利用这个笔记模板，可以自定义自己的文献笔记样式。

在这个文件夹下有这么个 md 文件：

![image-20210110170742198](https://cdn.jsdelivr.net/gh/Bend1031/PictureBed/img/image-20210110170742198.png)

`Mdnotes Default Template. md` 是模板的名字，一定不要修改名字。

接着是里面的内容

```text
{{title}}

## Metadata

{{itemType}}

{{author}}

{{date}}

{{dateAdded}}

#{{tags}}

## Zotero Links

{{pdfAttachments}}

{{localLibrary}}

## Abstract

## Inroduction

## Experimental

## Results and discussions

## Conclusions

## 单词与句型
```

其中用两个大括号包裹起来的是占位符，他的作用就是从 Zotero 文献中选取对应信息，放入我们的文献笔记中，这个地方也是我们可以自定义的地方。

## 创建笔记方法

在 Zotero 选中一篇文献条目 (** 不是文献对应的 pdf！**)，右键，出现菜单，选中 `Create Mdnotes file`。

![image-20210110171445492](https://cdn.jsdelivr.net/gh/Bend1031/PictureBed/img/image-20210110171445492.png)

接着出现一个选择框，提示你文献笔记放在哪？如果前面自定义正确的话，应该会出现在 `Export directory` 设置的地方，之后保存即可查看文献笔记的内容。

![image-20210110171741731](https://cdn.jsdelivr.net/gh/Bend1031/PictureBed/img/image-20210110171741731.png)

在对应的文献条目下出现了一个.md 链接，双击打开，在 Obsidian 显示如下：

<img src="https://cdn.jsdelivr.net/gh/Bend1031/PictureBed/img/image-20210110171946301.png" alt="image-20210110171946301" style="zoom: 67%;" />

可以看见文献信息都已经在笔记里了，其中 Zotero Links 下可以直接打开对应的文献位置，Local library 对应文献在 Zorero 中的位置。到此笔记已经成功创建。

## 自定义模板

若是要自定义模板，可前往 (https://argentinaos.com/zotero-mdnotes/docs/placeholders/#item-placeholders) 和（https://www.zotero.org/support/kb/item_types_and_fields ）查看还有什么占位符可用。

注意，使用 `{{}}`,mdnotes 会自动生成格式，若是想要不带格式的话，请使用 `%()`。
