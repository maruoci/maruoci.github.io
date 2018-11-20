---
title: Python基础(一)：包与模块的概念
date: 2018-09-28 09:30:00
categories:
- Python
tags: 
- Python
---

> 本节主要针对一些python文件的常规介绍和使用说明

## 包与模块的概念

  + python 包就是一个包含`__init.py`的文件夹。 `__init__.py`可以是一个空文件，文件夹的名字就是包名
  
  + 模块是包含一组功能的 python 文件，例如：`os.py`, 文件的名字就是模块名
  
  <!--more-->

## import与from的区别

  > 常见的用法有
  >     import package
  >     import module
  >     from package import module
  >     from module import function
  >     from module import *
  >     import package.module
  
  这里以示例说明, 示例的代码结构如下：
  
  <img src="http://pbsg2r9io.bkt.clouddn.com/18-9-28/78818553.jpg" width="1427"/>
  
  **import package** 
  
  引入包时，仅会执行包的`__init__.py`
  
  <img src="http://pbsg2r9io.bkt.clouddn.com/18-9-28/32771996.jpg" width="1427"/>
  
  **`import module`与`from module import *`**
  
  <img src="http://pbsg2r9io.bkt.clouddn.com/18-9-28/42694319.jpg" width="1427"/>
  
  <img src="http://pbsg2r9io.bkt.clouddn.com/18-9-28/29557917.jpg" width="1427"/>
  
  `from module import *`和`from module import function`用法一样，一个是引入全部函数，一个是引入特定函数。  
  
  两者的区别仅在于调用方式的区别，一般来说，为了可读性，尽量避免后面的写法。
  
  **`from package import module` 与 `import package.module`**
  
  <img src="http://pbsg2r9io.bkt.clouddn.com/18-9-28/14121908.jpg" width="1427"/>
  
  <img src="http://pbsg2r9io.bkt.clouddn.com/18-9-28/86341438.jpg" width="1427"/>
  
  两者也是调用方式不同，为可读性，避免使用`from ... import ` 语法。
  
**如何查看Api**
  
  

## 学习参考资料

  + [廖雪峰-Python教程](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000)
  + [python的模块和包机制：import和from..import..](https://blog.csdn.net/vincent2610/article/details/53787350)
