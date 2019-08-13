---
layout:     post
title: 在GitHub Pages中渲染 LaTex 公式
subtitle: 让博客支持公式显示
date: 2019-08-10
author: Bend
header-img: img/post-bg-universe.jpg
catalog: false
tags:
    - GitHub
    - Jekyll
    - LaTex
    - Markdown
---

>博客环境：Github Pages & Jekyll

<audio id="audio" controls="" preload="none">
<source id="mp3" src="http://m10.music.126.net/20190810114133/360a4b8bbb147fb16cee6161f2d3e872/ymusic/225a/0b65/aa03/90387f0b1f4558c67fe72c77f39e5b62.mp3">
</audio>

昨天在写学习笔记的时候，在书写公式时使用的是 Markdown 自带的 LaTex 语法，但是在网页实际预览的时候发现公式的显示并不正常😢，出现了 LaTex 的源码，影响了阅读效果，为了解决这个问题，进行了一番搜索，成功解决问题。😊

解决的方法是在静态页面中挂载 Javascript 代码，我用到的是 [MathJax](https://www.mathjax.org) 。
官网的介绍是：

>**Beautiful math in all browsers**
>A JavaScript display engine for mathematics that works in all browsers. No more setup for readers. It just works.

下面介绍步骤

1.首先找到博客的目录下的 **head.html**
![目录](https://raw.githubusercontent.com/Bend1031/PictureBed/master/img/20190810092912.png)

2.编辑_includes 目录下的 head.html 文件，在 \<head> 和 \</head> 中间的任意位置加上官方提供的脚本链接：

```html
<head>
    <script type="text/x-mathjax-config">
    MathJax.Hub.Config({ TeX: { equationNumbers: { autoNumber: "all" } } });
    </script>

    <script type="text/x-mathjax-config">
    MathJax.Hub.Config({tex2jax: {
             inlineMath: [ ['$','$'], ["\\(","\\)"] ],
             processEscapes: true
           }
         });
    </script>

    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript">
    </script>
</head>
```

上面的三个脚本分别实现了：

1.整行公式自动编号

2.将两个单美元符号 $ 中间的内容看作行内数学公式（若文本内容中美元符号出现频率较高，建议禁用这一脚本

3.从 mathjax 官网挂载脚本

Thanks!🤣
