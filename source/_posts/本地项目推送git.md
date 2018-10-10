---
title: 本地项目推送git
date: 2018-10-10 18:33:00
categories:
- Git
tags: 
- Git
---

### 操作步骤

+ 建立本地maven项目

+ `git init` 添加本地项目git版本控制

+ `git add .` 

+ `git commit -m 'init'`

+ 使用github或gitee 创建一个空项目

+ 复制项目的地址

+ `git remote add origin https://gitee.com/***/demo.git`

+ `git pull --rebase origin master` 获取远程库与本地同步合并（如果远程库不为空必须做这一步，否则后面的提交会失败）

+ `git push -u origin master` 推送到远程

+ `git status` 查看

### 参考

  + [如何用命令将本地项目上传到git](https://www.cnblogs.com/eedc/p/6168430.html)