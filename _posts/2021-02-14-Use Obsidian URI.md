---
layout: post
title: 使用 Obsidian URI
date: 2021-02-14
author: Bend
header-img: img/post-bg-debug.png
catalog: false
tags:
- Obsidian
---
> 本文翻译自 <https://publish.obsidian.md/help/Advanced+topics/Using+obsidian+URI>
>
> 更新于 2021-02-14

Obsidian 支持自定义 URI 协议 `Obsidian://` 可用来触发应用程序中的各种操作。这通常在 MacOS 上用于跨应用程序工作流

## 使用 Obsidian URI 之前

为了确保你能正常使用 `obsidian://`, 在使用之前你需要确保

-   在 Windows 上，运行最新的 obsidian 程序即可使用。
-   在 MacOS 上，运行程序的版本需要大于等于 0.8.12
-   在 Linux 上，请 [查看原文呢](https://publish.obsidian.md/help/Advanced+topics/Using+obsidian+URI)

## 使用 Obsidian URIs

Obsidian URIs 通常的格式是

**`obsidian://action?param1=value&param2=value`**

-   其中 `action` 是你通常要执行的操作

## 编码

确保 `valus` 已正确进行 URI 编码。 例如，正斜杠字符 **`/`** 必须编码为 **`％2F`**，空格字符必须编码为 **`％20`**。这特别重要，因为编码不正确的 "保留字符" 可能会破坏 URI 的解释。

## 可用的 actions

### Action `open`

描述：打开 Obsidian 库，通常用来打开库里的笔记

#### 可用的参数：

-   `vault` 可以是库名称也可以是库 ID
    -   vault ID 是分配给 `vault` 的 16 个字符的随机代码。此 ID 对于您计算机上的每个文件夹都是唯一的。例如：ef6ca3e3b524d22f。现在还没有一个简单的方法来查找此 ID，稍后将在 vault 切换器中提供一个。目前在 Windows 的路径为 **% appdata%/obsidian/obsidian.json**。对于 MacOS，将 **% appdata%** 替换为 **~/Library/Application Support/**。对于 Linux，将 **% appdata%** 替换为 **~/.config/**。
-   **`file`** 可以是文件名，也可以是从 vault 根目录到指定文件的相对路径
    -   为了解析目标文件，Obsidian 使用与 vault 中常规 **[[wikilink]]** 相同的链接解析系统。（没看懂 (～￣▽￣)～）（应该是可以直接打开这个名字的笔记）
    -   如果文件后缀是 **md** , 可以省略后缀
-   `path` 系统绝对路径
    -   使用 `path` 参数会覆盖 `vault` 和 `file` 参数
    -   这将导致应用程序搜索包含指定文件路径的最接近的 vault。
    -   然后路径的其余部分替换 `file` 参数。

#### 例子：

-   `obsidian://open?vault=my%20vault`

    打开库 `my vault`. 如果库已经打开，则聚焦到窗口。

-   `obsidian://open?vault=ef6ca3e3b524d22f`

    打开对应 `ef6ca3e3b524d22f`ID 的库

-   `obsidian://open?vault=my%20vault&file=my%20note`

    打开 `my vault` 库下的 `my note` 笔记，假定 `my note` 笔记存在且后缀为 `md`。

-   `obsidian://open?vault=my%20vault&file=my%20note.md`

    同样打开 `my vault` 库下的 `my note` 笔记

-   `obsidian://open?vault=my%20vault&file=path%2Fto%2Fmy%20note`

    打开在 `my vault` 库下的 `path/to/my note` 文件路径下的笔记。

-   `obsidian://open?path=%2Fhome%2Fuser%2Fmy%20vault%2Fpath%2Fto%2Fmy%20note`

    将会寻找包含 `/home/user/my vault/path/to/my note` 的库，然后剩下的路径传递给 `file` 参数。例如，如果在 `/home/user/my vault` 下有库，然后相当于 `file` 参数被设置成 `path/to/my note`。

-   `obsidian://open?path=D%3A%5CDocuments%5CMy%20vault%5CMy%20note`

    将会寻找包含 `D:\Documents\My vault\My note` 的库，然后剩下的路径传递给 `file` 参数。例如，如果在 `D:\Documents\My vault` 下有库，然后相当于 `file` 参数被设置成 `My note`。

### Action `search`

描述：打开库的搜索窗口，并可以选择执行搜索查询。

#### 可用参数：

-   `vault` 可以是库名称也可以是库 ID.，和 `open` 动作一样。
-   `query` （可选），搜索内容

#### 例子：

-   `obsidian://search?vault=my%20vault`

    打开 `my vault` 库，并打开搜索面板。

-   `obsidian://search?vault=my%20vault&query=MOC`

    打开 `my vault`, 并打开搜索面板搜索 `MOC`.

### Action `new`

    描述：在库内创建带有内容的笔记

#### 可用参数：

-   `vault` 可以是库名称也可以是库 ID.，和 `open` 动作一样
-   `name` 要创建的文件名，如果指定了此选项，将根据 “新笔记的默认位置” 首选项选择文件位置。
-   `path` 库的绝对路径，包括名称。仅在未指定 `name` 的情况下生效。
-   `content` (可选) 笔记的内容
-   `silent` (可选) 如果您不想打开新笔记，请设置此项。

#### 例子：

-`obsidian://new?vault=my%20vault&name=my%20note`

这将打开 `my valut` 库，并创建一个称为 `my note` 的笔记

-   `obsidian://new?vault=my%20vault&path=path%2Fto%2Fmy%20note`

这将打开 `my valut` 库，并在 path/to/my note 路径下创建一个名字为 `my note` 的笔记 .

### Action `hook-get-address`

描述：和 [Hook](https://hookproductivity.com/) 一起使用。将当前聚焦的笔记链接复制到剪贴板作为 obsidian://open URL.

使用: obsidian://hook-get-address

#### 可用参数：

-   `vault` (可选) 可以是库名称也可以是库 ID. 如果未指定，当前或上次打开的库将被使用

## 简写格式

除上述格式外，还有两种其他 “简写” 格式可用于打开库和文件：

-   `obsidian://vault/my vault/my note` 等同于 `obsidian://open?vault=my%20vault&file=my%20note`
-   `obsidian:///absolute/path/to/my note` 等同于 `obsidian://open?path=%2Fabsolute%2Fpath%2Fto%2Fmy%20note`

