---
layout: post
title: 通过 VS Code 同步本地代码到 GitHub
date: 2019-12-13
author: Bend
header-img: img/post-bg-debug.png
catalog: false
tags: 
- Git
- Github
- VS Code
---
## 将本地代码使用 Git 管理

1. 打开 VS Code , 打开要同步的文件夹，进行 Git 初始化   ![1](https://raw.githubusercontent.com/Bend1031/PictureBed/master/img/20191213184410.png)
2. 在 Github 上新建储存库 ![2](https://raw.githubusercontent.com/Bend1031/PictureBed/master/img/20191213185335.png)
3. 打开 Git 的 Bash 窗口，使用 `cd` 命令移动到要管理的文件，
4. 使用命令

```git
git remote add https://github.com/Bend1031/matlab.git
```

然后就可以使用 VS Code 自带的 Git 啦
