---
title: Python基础(四)：切片生成器等高级特性
date: 2018-10-17 16:57:30
categories:
- Python
tags:
- Python
---

### Python高级特性

#### 切片

  切片就是取子集，python语法中写过
  
  classmates[start:end:step] 完整写法
  
  <!-- more -->
  
  切片可以作用于list、tuple、字符串
  
  发现一个小问题： s = ''时，s[0]会报错，而s[0:1]则不报错  

#### 迭代

  for循环 
  
  for k , v  in dict
  
  for i , x in enumerate(L)
  
  isinstance(L, Iterable) 可以判断对象是否是可以迭代的

#### 列表生成式

  生成式的语法：[x * x for x in range(1,11) if x%2 == 0]  

#### 列表生成器

  列表生成式的 [] 改为 () 即为生成器。
  
  + [x * x for x in range(1,11)] -> (x * x for x in range(1,11))
  
  + yield 相当于一个断路器。执行到yield中断。函数中一旦包含yield 就变为了列表生成器。

#### 迭代器

  迭代器是Iterator，是一个惰性的迭代。可以通过isinstance(obj, Iterable)来判断
  
  list、tuple、str都是Iterable，但不是Iterator  

### 函数式编程
  
#### map()/reduce()

  map(func, *iterable): 按func的函数遍历计算iterable中的每个值，然后返回一个iterator。
  
  from functools import reduce
  reduce(func, sequence[, initial])  函数需要接受两个参数，从sequence中从左到右取两个值计算并与后面的值计算，类推计算最终的值返回。
  如：reduce(lambda x,y:x+y , range(101))
  
    