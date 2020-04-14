---
layout: post
title: 用 jsDelivr 来提高图片的访问速度
date: 2020-04-14
author: Bend
header-img: img/post-bg-js-version.jpg
catalog: true
tags:
- GitHub
- jsDelivr
---

## 图床问题

图床一直以来是困扰我的问题之一，在建立博客之处，选择的是 GitHub 图床，主要是取其免费易操作的特性。但是由于 GitHub 在国内的访问总是会出现各种小问题，导致文章中的图片总是不能正常显示，不经意间看到利用 jsDelivr 来提高图片的访问速度的方法，所以记录一下。

## jsDelivr

jsDelivr 提供了对 GitHub 的 CDN 加速服务，在世界各地都有服务器，极大提升了 GitHub 上静态资源的访问速度。

## PicGo

PicGo 是一个图床工具，支持将图片上传到各种图床上。

PicGo 的插件 picgo-plugin-pic-migrater 可以一键将博客文章 markdown 的图床统一迁移到了 GitHub 上。

为了利用 PicGo 将上传的图片经过 jsDelivr 上传到 GitHub 上，我们需要在 PicGo 的自定义域名上填写相关 `网址` 。如图
![example](https://cdn.jsdelivr.net/gh/Bend1031/PictureBed/img/20200414195832.png)

经过这样设置后，图片的加载速度明显快了很多。
