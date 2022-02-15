---
layout: post
title: Python 中的模块化与路径问题
date: 2022-02-15
author: Bend
header-img: img/post-bg-rwd.jpg
catalog: True
tags:
- python
---

当尝试python写一些稍微大型的程序的时候，简单的一个py文件已经无法胜任，这时更加有效的方法是尝试模块化的操作，即将函数、类、常量拆分成不同的文件，把它们放在同一个文件夹，然后使用 from your_file import function_name, class_name 的方式调用。

但是为了区分不同的模块，往往是需要将不同模块放到不同的文件夹当中，此时不同文件夹下的文件相互调用就会出现问题。

例如对于下面这样一个文件目录来说：

```
.
├── utils
│   ├── utils.py
│   └── class_utils.py
├── src
│   └── sub_main.py
└── main.py
```

对于这样一个目录，main.py调用子目录下 utils 中的 utils.py 只需要 `import utils.utils` 即可。但此时对于 sub_main.py 这个文件来说，如果要调用utils.py则会出现问题，因为这两个文件没有处在同一文件夹内，若是运行sub_main.py , python会在src文件夹下搜索，自然就找不到utils.py了。

说到这里，需要引申出python查找模块文件的方式

## python 解释器的模块搜寻路径

对于python解释器来说，当import某个模块时，解释器首先搜索具有该名称的

* 内置模块

如果没有找到，将在变量 sys.path 给出的目录列表中搜索名为 模块名.py 的文件。sys.path 包含了以下几个目录：

* 输入脚本的当前目录；
* PYTHONPATH 环境变量目录（windows设置的系统变量）
* 标准链接库目录
* 第三方目录（site-packages目录）
* .pth文件的内容（如果存在的话）

通过运行下面这一段代码，可以看到 Python 的导包路径

```python
import sys

print('导包路径为: ')

for p in sys.path:

 print(p)
```

以我一个环境的导包路径为例：

```
导包路径为: 
d:\Code\rail\RailCode
D:\Code\rail
D:\Engirneering\Anaconda\envs\rail\python38.zip
D:\Engirneering\Anaconda\envs\rail\DLLs
D:\Engirneering\Anaconda\envs\rail\lib
D:\Engirneering\Anaconda\envs\rail
D:\Engirneering\Anaconda\envs\rail\lib\site-packages
```

第一行为脚本的当前目录
第二行为我设置的 PYTHONPATH 环境变量目录
第三行往后则是标准库目录以及第三方目录
（注意此时我没有设置 .pth 文件）

为了解决前文中提出的不同文件夹下文件互相调用查找不到包的情况，根据解释器的模块搜寻路径，有以下几种方式：

### 1. 修改 sys.path

最常见的方式就是：

把当前路径添加到 `sys.path` 中，且为了避免命名冲突，最好添加到列表的头部，而不是用 `append` 添加到尾部。

对于下面这样一个文件路径，父目录为.，子目录有test文件夹和package文件夹，现在我们想要在test.py文件里面调用func

```
.
├── test
│   ├── context.py
│   └── test.py
│   
└── package
    └── func.py
```

为此，我们在context.py中这样写

```python
# context.py

"""使得其他测试文件能正常导入所需包"""

import os

import sys

# 环境里添加包

# 将上级文件夹加入路径搜寻

sys.path.insert(0, os.path.abspath(

os.path.join(os.path.dirname(__file__), '..')))

  

import package
```

在test.py文件中

```python
# test.py
from context import package.func
```

但是这个方案不太好，有一些缺点，看起来就很不优雅，因为按照 python 的代码规范，导包相关的代码应该写在最前面，这种 `导包+代码+导包` 的方式破坏了 `pythonic`

### 2. 修改环境变量

由于我所使用的环境为 windows 平台下用 conda  创建的虚拟环境。在进入环境前，需要利用

```
conda activate env_name
```

来进入 env_name 环境，因此考虑利用conda，在运行激活环境的命令前，自动执行修改环境变量的脚本，从而实现进入环境后，自动修改 `PYTHONPATH` 。conda从v3.8后已经实现了对应功能的支持。
具体来说，在windows平台上，在Anconda安装目录下envs文件夹下进入想要更改的环境，(以 rail 环境举例)。在 `Anaconda\envs\rail` 文件夹下，运行命令, 创建文件夹与对应文件（也可以在资源管理器中直接新建文件夹与文件），env_vars.bat 文件的名字取名是任意的，但是建议设置为 `env_name-env_vars.bat` , 对于rail环境，则是 `rail-env_vars.bat` 。

```
mkdir .\etc\conda\activate.d
mkdir .\etc\conda\deactivate.d
type NUL > .\etc\conda\activate.d\env_vars.bat
type NUL > .\etc\conda\deactivate.d\env_vars.bat
```

编辑 `\etc\conda\activate.d\env_vars.bat` 如下：

```
set PYTHONPATH=D:\Code\rail
```

编辑 `\etc\conda\deactivate.d\env_vars.bat` 如下：

```
set PYTHONPATH=
```

设置完成后，运行 `conda activate rail` 时，环境变量 `PYTHONPATH` 将设置为写入文件的值。运行 `conda deactivate` 时，这些变量将被删除。

这样设置之后，所有的模块的追寻方式，都从项目的根目录开始追溯，这叫做 `相对的绝对路径` 。

> 此外这里给出 macOS and Linux 平台对应的文档，方便读者使用：[macOS and Linux](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#macos-and-linux)

### 3. 在site-packages目录下添加 .pth文件

Pyhthon 会自动读取site-packages目录下，是否存在.pth文件，若是存在，则会将.pth文件中的路径也加入到搜寻当中

通过如下代码，可以找到.pth文件可以放置的位置：

```python
import site

print(site.getsitepackages())
```

例如我的输出是

```
['D:\\Engirneering\\Anaconda\\envs\\rail', 'D:\\Engirneering\\Anaconda\\envs\\rail\\lib\\site-packages']
```

则可以在site-packages目录下新建一个 `path.pth` 文件（文件的命名也是随意的）, 里面写入想要加入的路径，这样当python运行时，也会导入此路径

```python

## path.pth

D:\Code\rail
```

 `windows操作系统记得使用 \\ 或 \ 作为分隔符。`

## 文件路径

类似的，在文件的调用中，也存在着绝对路径与相对路径之间调用产生的问题。
举个栗子：
现在有两个文件 `main.py` 和 `func.py` ，他们的路径关系是：

```
.
|--dir1
    |--main.py
    |--dir2
          |--func.py
          |--test.txt
```

`func.py` 的作用是提供 `load_txt()` 函数，读取**同级目录**下 `test.txt` 文件中的内容并返回。

```python
# func.py

def load_txt()
    filename = './test.txt'
    return open(filenamem, 'r').read()
```

假设现在在 `main.py` 中调用 `load_txt()` 函数：

```python
# main.py
from dir2 import func
 
if __name__ == '__main__':
    print func.load_txt()
```

这个时候会报类似 `找不到文件test.txt` 的错误。

为什么会这样呢？这是因为在函数调用的过程中，当前路径 `.` 代表的是**被执行的脚本文件**的所在路径。在这个情况中， `.` 表示的就是 `main.py` 的所在路径，所以 `load_txt()` 函数会在 `dir1` 文件夹中寻找 `test.txt` 文件。

类似的，这里也可以用到 `相对的绝对路径` 这一概念：将项目的根目录作为基准，从这个基准写路径：

例如在系统根目录下新建 rootpath.py（这不是很优雅，举个例子）

```python
# rootpath.py
import os
    
rootPath = os.path.abspath(os.path.dirname(__file__))
```

在其他需要引用文件的地方导入根目录

```python
from rootpath import rootPath

dataPath = os.path.abspath(rootPath + '\\static\\stopwords.txt')
```

这样就实现了文件路径不会混乱。

That's all!

## 参考资料

[Python 模块搜索路径](https://blog.csdn.net/liuxiao723846/article/details/109002196)

[python 当前路径和导包路径问题全解析](https://segmentfault.com/a/1190000041131903)

[Python 模块搜索路径](https://blog.csdn.net/qq_38934189/article/details/109534444)

[Python--多层包多模块复杂调用](https://www.cnblogs.com/kuzaman/p/9606307.html)

[Python中的相对文件路径的调用](https://www.jianshu.com/p/cd421014cfbb)

[python项目中关于文件路径的各种糟心事](https://blog.csdn.net/muyao987/article/details/89203285)
