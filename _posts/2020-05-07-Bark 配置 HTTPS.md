---
layout: post
title: Bark 配置 HTTPS
date: 2020-05-07
author: Bend
header-img: img/post-bg-bark.png
catalog: true
tags:
- Bark
- HTTPS
- Nginx
---

## [Bark 是什么？](https://sspai.com/post/53090)

**本文主要介绍一下怎么在自己的服务器上部署 Bark 和 HTTPS。**

## 云端的准备

首先我们要准备一个二级域名，便于我们使用 Bark 服务，其次，为了部署 HTTPS，还需要我们为这个二级域名申请一张证书。我用的是阿里云的服务器，所以一下介绍都会用阿里云作为例子。

### 申请证书

![购买证书](https://cdn.bend1031.top/img/购买证书.png)

在 SSL 证书申请页面，我们点击购买证书。

![免费证书](https://cdn.bend1031.top/img/image-20200507194426600.png)

我们申请的是一个单域名的免费证书。( ~~要钱的证书买不起~~ )

点击购买，要等上一段时间（大约一天之内，快的话几分钟），证书就会下来。在申请的时候，我们要填上我们需要证书的域名，我填写的是 `api.bend1031.top` 。

### 云解析解析 DNS

在等待证书下来的过程中，我们还要配置下 DNS 解析。

![DNS 解析](https://cdn.bend1031.top/img/image-20200507195414697.png)

点击添加记录。

![添加解析](https://cdn.bend1031.top/img/image-20200507195643244.png)

当证书申请下来之后，下载备用。在云端的配置就结束了。

## 服务器的设置

### 证书上传

为了方便，我用了宝塔面板来方便配置。

![证书目录](https://cdn.bend1031.top/img/image-20200507200220977.png)

在图中路径下，新建一个`api.bend1031.top`文件夹，然后在这个文件夹里面上传我们申请的证书。

![证书路径](https://cdn.bend1031.top/img/image-20200507200424619.png)

图中的证书名字我已经改了，这里名称不一致没有关系，然后我们把权限设置成和图中一样就行。（鼠标移到文件上，右边会显示权限设置）。

### Nginx 的配置

![配置文件](https://cdn.bend1031.top/img/image-20200507200846830.png)

在图中路径处新建文件`api.bend1031.top.conf` ，然后编辑，填入下方配置。

```
server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name api.bend1031.top ;

    # certs sent to the client in SERVER HELLO are concatenated in ssl_certificate
    ssl_certificate /www/server/panel/vhost/cert/api.bend1031.top/fullchain.pem;
    ssl_certificate_key /www/server/panel/vhost/cert/api.bend1031.top/privkey.pem;
    ssl_session_timeout 10m;
    ssl_session_cache shared:SSL:10m;
    ssl_session_tickets off;

    # modern configuration. tweak to your needs.
    ssl_protocols TLSv1.2;
    ssl_ciphers 'ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256';
    ssl_prefer_server_ciphers on;

    location / {
       proxy_pass http://0.0.0.0:8080;
    }

}

server {
    listen 80 ;
    server_name api.bend1031.top ;

    location / {
       proxy_pass http://0.0.0.0:8080;
    }

}
```

这里我用的是 [Bark 作者的配置](https://github.com/Finb/bark-server/issues/5#issuecomment-473141149)，对不需要的设置进行了删改。

你只需要将其中的 `api.bend1031.top.conf` 改成自己的二级域名，证书的路径填写对（图中 7，8 行，填入你证书的路径和名称）, 还有 Bark 端的 ip 和端口改成你自己用的 ip 和端口。

最后在宝塔面板重载配置，重新启动 Nginx，配置就完成了。

![重启 Nginx](https://cdn.bend1031.top/img/image-20200507201936236.png)
