---
title: urllib库的使用
date: 2018-11-14 21:07:30
categories:
- Python
tags:
- Python
---

> 本API是基于Python3.X的版本，urllib库2.x版本与3.x的版本有较大变动。

 urllib是Python内置的HTTP请求库，包含request，error，parse，robotparser四个模块。
 
## 参考资料

 **Python3网络爬虫开发实战-崔庆才** 

<!-- more --> 

## request模块

 request模块是HTTP请求模块，模拟发送请求。

**urlopen()**

 ```
    ## urlopen可以实现基本请求的发起，不足以构建完整的请求
    urlopen(
        url,            // 请求链接  
        data=None,      // 请求参数，传递时需使用bytes()将参数转为字节流
        [timeout, ]*,   // 超时参数，单位为秒，未指定时采取全局默认时间
        cafile=None,    // 指定CA证书
        capath=None,    // 指定CA路径
        cadefault=False, //参数已弃用
        context=None    // 指定SSL设置，类型必须是ssl.SSLContext类型
    )
 ```
 
**Request类**

 更强大的请求构建方式，支持Headers等信息。
 
 ```
    urllib.request.Request(
        url,           // 请求链接 
        data=None,     // 请求参数，传递时需使用bytes()将参数转为字节流,参数为字典时，可以parse.urlencode(dict)
        headers={},    // 请求头信息，可直接构造，也可以通过请求实例的add_header()方法添加
        origin_req_host=None,   //请求的host名称或IP
        unverifiable=False,     //请求是否无法验证（没太明白） 
        method=None   // 请求方式(GET/POST/PUT等)
    )
 ```
 
## error模块

 urllib的error模块定义了request模块产生的异常。
 
 **URLError**
 
 继承OSError类，是error模块的异常基类
 
 **HTTPError**
 
 是URLError的子类，处理HTTP请求错误。
 
 ```python
    try:
        response = request.urlopen('http://www.jflsznc-slfjsl.com')
    except error.HTTPError as e:
        print(e.reason, '>> e.code :', e.code)
    except error.URLError as e1:
        print(e1.reason)
 ```

## 解析模块 

 urllib 库里还提供了parse 模块，它定义了处理URL 的标准接口，例如实现URL各部分的抽取、合并以及链接转换。
 
 **urljoin()**
 
 ```
 urljoin(
    base,   # 基础链接 
    url,    # 新链接，分析新链接并对新链接缺失的部分进行补充
    allow_fragments=True
   )
 base_url = "http://www.baidu.com/"  # scheme, netloc
 new_url = parse.urljoin(base_url, "http://test.com/abc=123") # http://test.com/abc=123
 new_url = parse.urljoin(base_url, "abc=123")                 # http://www.baidu.com/abc=123
 ```
 
 **urlencode()**
 
 将字典类型的参数序列号为GET请求参数
 
 ```
 urlencode(
    query, 
    doseq=False, 
    safe='', 
    encoding=None, 
    errors=None, 
    quote_via=<function quote_plus at 0x0000000004398A60>)
 
 base_url = "http://www.baidu.com/"  # scheme, netloc
 param = {
    'name': 'zhangsan',
    'age': 123
 }
 result = base_url + parse.urlencode(param)  # http://www.baidu.com/name=zhangsan&age=123
 ```
 
 **quote()**
 
 将内容转化为URL编码的格式，参数中包含中文时使用
 
 ```
 base_url = "http://www.baidu.com/key="  # scheme, netloc
 keyword = '中文'
 result = base_url + parse.quote(keyword)
 print(result)   # http://www.baidu.com/key=%E4%B8%AD%E6%96%87
 ```
 
 **unquote()**
 
 URL解码