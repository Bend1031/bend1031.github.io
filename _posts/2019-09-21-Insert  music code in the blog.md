---
layout:     post
title: GitHub Pages 博客 插入音乐播放器 APlayer
subtitle: Insert  music code in the blog
date: 2019-09-21
author: Bend
header-img: img/post-bg-music.jpg
catalog: true
tags:
    - Jekyll
    - GitHub
    - Music
---
我使用的是 Jekyll 来搭建的博客，因为最近想在博客里加一个播放器的功能，所以忙活了一晚上，终于搞定了。

先直接来个简单粗暴的，一步到位的方法。

## 在博客模板里插入代码

```html
<!--APlayer-->
<script src="https://cdn.jsdelivr.net/npm/vconsole/dist/vconsole.min.js"></script>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/aplayer@1.10.1/dist/APlayer.min.css">
<script src="https://cdn.jsdelivr.net/npm/hls.js/dist/hls.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/aplayer@1.10.1/dist/APlayer.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/color-thief-don@2.0.2/src/color-thief.js"></script>
<script src="https://cdn.jsdelivr.net/npm/meting@2.0.1/dist/Meting.min.js"></script>
<meting-js server="netease" type="playlist" id="167985096" fixed="true"></meting-js>
```

具体的插入位置是在 _includes 文件夹下的 footer.html 文件里，找一个不会干扰其他代码的地方就好啦。

![](https://raw.githubusercontent.com/Bend1031/PictureBed/master/img/20190921223054.png)

## APlayer

然后我来解释一下到底发生了什么。首先，我们在博客里面添加的播放器的名字是 Aplayer，是一个开源的播放器。

- [APlayer 的 GitHub 主页](https://github.com/MoePlayer/APlayer)
- [APlayer 的中文文档](https://aplayer.js.org/#/zh-Hans/)

简单来说，就是在网页里嵌入了一些 js 脚本（~~我也不知道说的对不对~~），这里我们用的是从 cdn 上加载，也就是通过联网获取别处的 js 文件，放进自己的网页中，这样的优势是不用在本地新建文件，缺点嘛就是万一这个资源没了，你的播放器也就没了，还有就是每次都要联网加载，~~应该会降低网页打开速度~~。 还有一种就是把 js 文件保存在本地，从本地加载，但显然不如从 CDN 加载来的方便快捷。

我先来解释下每一行的作用

```html
<!--APlayer-->
<script src="https://cdn.jsdelivr.net/npm/vconsole/dist/vconsole.min.js"></script>
```

第一行为注释，不用讲，第二行嘛，不懂，不讲（~~其实我也不知道有没有用，应该是不用的，因为我比较懒，就没删~~）😂

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/aplayer@1.10.1/dist/APlayer.min.css">
<script src="https://cdn.jsdelivr.net/npm/hls.js/dist/hls.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/aplayer@1.10.1/dist/APlayer.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/color-thief-don@2.0.2/src/color-thief.js"></script>
```

第一行是加载 css 样式，也就是播放器的皮肤；

第二行是加载了一个 hls.js 文件，这个不太懂。

第三行是我们的正主 APlayer。

第四行是根据封面自适应主题色，但我好像没太看出来效果是什么。

## MetingJS

- [MetingJS 的 GitHub 项目主页](https://github.com/metowolf/MetingJS)

但是这样其实也有缺点，因为 APlayer 需要手动配置音乐 url 和歌词封面等音乐相关信息，比较麻烦。MetingJS 就是用来配合它的。

可以这样理解，APlayer 只是一个播放器，歌曲什么的都还没有，需要我们自己添加，但是 MetingJS 就可以帮我们把自己在网易或者 QQ 音乐的歌单里的歌，直接放进播放器里，省心！

```html
<script src="https://cdn.jsdelivr.net/npm/meting@2.0.1/dist/Meting.min.js"></script>
<meting-js server="netease" type="playlist" id="167985096" fixed="true"></meting-js>
```

第一行是加入了 Meting.js。

第二行是我们重点的配置环节。

我们可以先看看 [MetingJS 的文档](https://github.com/metowolf/MetingJS#Option)。

- server 后面的是我们要用的音乐平台，可以填写的有`netease`, `tencent`, `kugou`, `xiami`, `baidu` 。这里我用的是网易 server="netease"

- type 后面可以填写 `song`, `playlist`, `album`, `search`, `artist`，分别是歌曲，歌单，专辑，搜索，歌手。这里我用的是歌单，所以 type="playlist"。

- id 是我们歌单的链接号码，这个数字我们可以登录网页版的网易云，打开一个歌单，从网址就能看出 id 是多少。

![](https://raw.githubusercontent.com/Bend1031/PictureBed/master/img/20190921232812.png)

- fixed 其实是 APlayer 的选项，作用是开启「吸底模式」`true` 是开启，默认情况是 `false`。作用就是一个缩小的选项，不多赘述。

到这里，我们的教程就差不多了，可以改的选项还有一些，大家可以自己去看文档了。

## 参考资料

[使用 APlayer && MetingJS 在博文中优雅的嵌入音频](http://yangyingming.com/article/428/)

[添加全站 APlayer 播放器](https://cloud.tencent.com/developer/article/1157669)
