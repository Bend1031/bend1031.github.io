---
layout: post
title: 将 nazurin 从 Heroku 迁移到 Fly.io
date: 2022-12-04
author: Bend
header-img: img/post-bg-fly.jpg
catalog: false
tags:
- Others
---
因为 heroku 平台需要收费使用了，因此选择迁移到其他平台，根据 nazurin 作者 [y-young](https://github.com/y-young/nazurin) 编写的 [迁移文档](https://github.com/y-young/nazurin/wiki/Deploy) 进行迁移，因为迁移过程中还是遇到了一些困难，因此还是记录一下。

## 1. 安装 flyctl

flyctl 是 Fly.io 提供的命令行工具，具体文档地址为 [https://fly.io/docs/hands-on/install-flyctl/](https://fly.io/docs/hands-on/install-flyctl/)。
windows 环境下安装，在 PowerShell 中运行命令，若是网络不畅需要设置系统代理。

```bash
iwr https://fly.io/install.ps1 -useb | iex
```

随后在命令行里注册，会打开注册的网页，建议直接 github 注册登录。

```bash
flyctl auth signup
```

## 2. 下载 github release 里最新版本的源代码

下载链接：<https://github.com/y-young/nazurin/releases/tag/>

主要需要的是源代码下`fly.toml`这个文件！主要的配置都依赖这个文件。
在代码文件夹下运行：

```bash
fly launch --copy-config --image ghcr.io/y-young/nazurin:latest
```

进行配置，app 名字随意配置，地区选 ams (Amsterdam, Netherlands)（给的免费额度多且与 Telegram 服务器近），其他选 no 不进行配置。

```bash
> fly launch --copy-config --image ghcr.io/y-young/nazurin:latest
An existing fly.toml file was found for app nazurin
Creating app in /nazurin
Using image ghcr.io/y-young/nazurin:latest
? App Name (leave blank to use an auto-generated name): {Your Application Name}
Automatically selected personal organization: {Your Organization Name}
? Select region: ams (Amsterdam, Netherlands)        
Created app nazurin in organization personal
Wrote config file fly.toml
? Would you like to set up a Postgresql database now? No
? Would you like to deploy now? No
Your app is ready. Deploy with `flyctl deploy`
```

## 3. 配置设置

参照源代码里的`.env.example`模板新建`.env`文件，将配置写入`.env`文件（因为从 heroku 迁移来的，因此配置的具体内容在 heroku 中）。

- webhook 的配置应该类似 <https://YOUR_APP_NAME.fly.dev/> **注意最后的斜杠！!!**，YOUR_APP_NAME 为前面设置的 app 名字。
- port 项删除，因为在`fly.toml`已经设置了。

在 python 环境下，安装 python-dotenv:

```bash
pip install python-dotenv
```

运行`set_fly_secrets.py`：此步是为了设置`.env`中的配置。

```bash
python ./tools/set_fly_secrets.py
```

## 4. 部署实现

```bash
fly deploy
```

最后成功实现迁移，感谢 y-young 开发的 nazurin！
