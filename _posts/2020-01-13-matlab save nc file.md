---
layout: post
title: Matlab 保存 nc 文件
date: 2020-01-13
author: Bend
header-img: img/post-bg-universe.jpg
catalog: false
tags: 
- Matlab
---
```matlab
% save nc file
fid = fopen('sphere.nc','w');
for i=1:nsize
    fprintf(fid, 'X%.6f\tZ%.6f\n', cutx(i),cutz(i));
end;
fclose(fid);
```
