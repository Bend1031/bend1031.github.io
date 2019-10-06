---
layout: post
title: 将 Matlab 编码改为 UTF-8
date: 2019-10-06
author: Bend
header-img: img/post-bg-universe.jpg
catalog: false
tags: 
    - Matlab
---
> Matlab 版本：2019a

1. 将 lcdata_utf8.xml 重命名为 lcdata.xml

2. 删除  lcdata.xml 中的

   ```xml
   <encoding name=”GBK”>  
     < encoding_alias name=”936”>  
   </encoding>
   ```

   并将  

   ```xml
   <encoding name=”UTF-8”>  
    <encoding_alias name=”utf8”/>
   </encoding>  
   ```

   改为

   ```xml
   <encoding name=”UTF-8”>  
     <encoding_alias name=”utf8”/>  
     <encoding_alias name=”GBK”/>  
   </encoding>  
   ```

## 参考资料 ##

[Matlab 如何以 UTF-8 编码保存？](https://www.zhihu.com/question/27933621)
