---
layout: post
title: VS Code Remote SSH 配置密钥连接 
date: 2020-09-27
author: Bend
header-img: img/post-bg-debug.png
catalog: false
tags:
- ssh
- VS Code
- Linux
---

## 生成密钥对

（需要电脑上有 Open SSH）

```bash
ssh-keygen -t rsa
```

生成的密钥对默认保存在当前用户的根目录下的.ssh 目录中

```bash
C:\Users\username\.ssh
```

## 上传公钥

id_rsa.pub 上传至 Linux 服务器用户目录

```bash
/root/.ssh
```

## 重命名以及权限修改

上传好后，将 Linux 中的 id_rsa.pub 重命名为 authorized_keys，更改文件权限为 600，更改。ssh 目录权限为 700：

```bash
mv id_rsa.pub authorized_keys
chmod 600 authorized_keys
chmod 700 .ssh
```

## 修改 vscode remote 配置

```bash
Host 服务器
    HostName ip
    User root
    IdentityFile C:/Users/user/.ssh/id_rsa
```
