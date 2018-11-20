---
title: requests库的使用
date: 2018-11-17 16:57:30
categories:
- Python
tags:
- Python
---

requests相比urllib库来讲在处理cookie、登录验证、代理设置等方面拥有更强大的操作能力。

requests属于第三方库，使用前需要先`pip install requests`安装。

## 参考资料

 **Python3网络爬虫开发实战-崔庆才** 
 
<!-- more --> 

## 基础用法

 **get()**
 
 ```
 get(
    url,         
    params,         # 通过url传递的参数
    **kwargs,       # 关键字参数
      [ data,           # 表单参数 或 文件对象
        json,           # json序列化参数
        headers,        # 表头
        cookies,        # cookie
        files,
        auth,           # 认证参数
        timeout,        # 超时设置
        proxies,
        ...
      ]
 )
 
 return Response 包含 status_code 、 encoding、 reason、content等
 
 import requests
 requests.get('http://www.baidu.com')
 
 param={
    'name': 'zhangsan',
    'age': 23
 }
 requests.get('http://www.baidu.com', params=param)
 ```