---
title: 字符编码与变量
date: 2018-10-17 16:57:30
categories:
- Python
tags:
- Python
---

> 学习廖大的教程后自己做的算是笔记吧

### 字符编码

  一个字节(byte)是8位(bit)。所以一个字节的最大整数是(1111 1111)，2的八次方 - 1 是255。
  
  最早仅有大小写英文字母、数字或一些符号等127个字符，也就是Ascii表。如 A：65
  
  然后为了显示中文， 中国制定了GB2312编码，日本制定了Shift_JIS里，韩国制定了Euc_kr等等等等。多语言混合的文本下，不可避免会出现冲突。
  
  <!-- more --> 
  
  Unicode应运而生。比如：
  
  A 的 Ascii码 为 65(0100 0001), Unicode码为 0000 0000 0100 0001 UTF的码为 0100 0001  
  中的 Ascii码  不支持中文      , Unicode码为 0100 1110 0010 1101 UTF的码为 1110 0100 1011 1000 1010 1101
   
  + 记事本或文件在处理时的编码转换：
      文件以UTF-8存储，当编辑时，程序读取文件并转存为Unicode编码 到内存中，保存时再转换为UTF-8到文件中。
      
  + 浏览网页时，程序会把动态生成的Unicode编码转换为UTF-8输出给浏览器      

  + '中'.encode(encoding="UTF-8",errors='strict') 方法。
     
    + encode 方法，以encoding指定的编码格式，编码字符串。
    
    + encoding: 编码。比如:utf-8
    
    + errors : 编码错误的处理方案，默认是strict抛UnicodeError异常，还有：ignore

### 字符串处理

### list列表

  list是一种有序的集合，可以随时添加和删除其中的元素。
  
  `classmates = ['Mike', 'Bob', 'Tracy', 'zhangsan', 'wangwu']`
  
  + len(classmates) :5  list元素的个数。
  
  + classmates[0]: list元素第一个元素。
    + classmates[-5]: list元素第一个元素。
    + classmates[-1]: list最后一个元素。
  
  + list的增删改
    + classmates.append('python') 元素末尾追加
    + classmates.insert(1, 'java'),在脚标1的位置追加一个元素，后面的元素顺移。
    + classmates.pop() 删除末尾的元素
    + classmates.pop(-1) 删除末尾的元素
    + classmates[1] = 'php' 直接替换脚标1的位置的元素。
  
  + L = ['123', 40 , True] 列表的元素类型可以不同。
  
  + classmates[0:2]
      + classmates[0:2] 从0位截取，截止到2位。闭开区间。
      + classmates[0:-3] 从0位截取，截止到-3位，闭开区间。
      + classmates[:3] 从0位截取，到3位，闭开区间
      + classmates[0:] 从0位截取，到最后一位，闭开区间
      + classmates[0::1] 从0位截取，到最后一位，step为1，不间隔取数。  
      + classmates[start:end:step] 完整写法
  
  + 其他函数
      + max(list) 列表最大元素
      + min(list) 列表最小元素
      + list(seq) 将元组转换为list
      + list.count(obj) 统计某个元素在列表内的个数
      + list.remove(obj) 删除列表中匹配该元素的第一个
      + list.sort() 列表排序      
          