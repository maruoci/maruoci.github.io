---
title: Git:常用操作
date: 2020-03-12 21:34:47
categories:
- Git
tags: 
- Git
---

**移除Git的文件夹版本控制**

![](https://raw.githubusercontent.com/maruoci/images/master/Java/Git/git-use-01.jpg)

提交Git版本控制时，ignore文件未配置导致把target目录推送到远程。

```
git rm -r -n --cached "target/"  ## 删除文件表预览
git rm -r --cached "target/"	 ## 删除文件
git commit -m "删除target目录"   ##  提交
git push origin master 			 ## 推送远程
## 粗暴做法（第二种需要先把ignore文件配置好）
git rm -r –-cached . 			 ## 删除所有本地暂存区文件
git add . 						 ## 重新提交并推送
git commit -m “update .gitignore”
```

 删除后记得添加ignore配置。

<!-- more -->