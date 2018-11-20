---
title: Python基础(二)：函数的参数类型
date: 2018-09-28 15:16:00
categories:
- Python基础
tags: 
- Python
---

> 学习廖大的教程后自己做的算是笔记吧

### 位置参数

```
  def power(x, n):
    s = 1
    while n>0:
      n = n -1 
      s = x * s
    return s
```

  x和n都是位置参数
  
  <!--more-->

### 默认参数

```
  def power(x, n=2):
      s = 1
      while n>0:
        n = n -1 
        s = x * s
      return s
```
  
  x 为位置参数， n为默认参数。设置默认参数时：
  
  必选参数在前，默认参数在后。
  
  定义默认参数要牢记一点：默认参数必须指向不变对象！

### 可变参数

```
  def calc(*numbers):
    sum = 0
    for n in numbers:
      sum = sum + n * n
    return sum
  print(calc(1, 2))
  L = [1, 2, 3, 4, 5]
  print(calc(*L))
  t = (1, 3, 5)
  print(calc(*t))
```
  
  可以在list或tuple前加*号来将其变为可变参数传递给函数

### 关键字参数

```
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)
person("mark", 40, city='北京', company='YXT')
d = {'city': '北京', 'company': 'YXT'}
person("mark", 40, **d)
```

  > **d 把 d这个dict里的所有k-v都作为关键字参数传递给函数, **d 是d的拷贝 对d产生任何影响 
  
### 命名关键字参数

  + `def person(name, age, *, city, job)` 限制关键字必须且仅包含city和job。(* 用以分割位置参数和 命名关键字参数)
  
  + `def person(name, age, *args, city, job)` 当函数中存在可变参数时，可以不需要 *  
  
  + `def person(name, age, *args, city, job='IT')` 当job作为默认参数时，命名关键字可以不传递
  
  使用命名关键字参数时，要特别注意，如果没有可变参数，就必须加一个*作为特殊分隔符。如果缺少*，Python解释器将无法识别位置参数和命名关键字参数


### 总结

  位置参数、默认参数、可变参数、命名关键字参数、关键字参数
  
    
  