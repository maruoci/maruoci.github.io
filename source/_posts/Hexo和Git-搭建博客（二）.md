---
title: Hexo和Git 搭建博客（二）
date: 2018-06-23 00:55:22
tags: hexo, git
---

# Hexo和Git 搭建博客（二）

>  本节主要描述多台电脑之间如何同步并更新博客。
>  主要是按照分支提交源文件，master提交静态文件的原则。

## 创建hexo分支

在github的`ma***ci.github.io` 上面新建一个`hexo`分支。

并settings-> branches-> Default branch 将hexo 设置为默认分支并update。

## 克隆

执行`git clone git@github.com:maruoci/maruoci.github.io.git` 将仓库克隆至本地。

```
// 进入该目录
cd ma***ci.github.io
// 查看当前所在分支(hexo)
git branch
```

## 清空hexo分支

```
// 进入该目录
cd ma***ci.github.io
// 删除除.git外的所有内容
// 推送到github
git add -A
git commit -m "init"
git push origin hexo
```

至此，hexo分支应该被清空了，master分支保持不变。

## push分支hexo到github

拷贝hexo分支下唯一的.git目录到 源目录(包含source)。

删除themes下所有的.git、.gitignore的文件。

到博客目录，推送hexo

```
// 推送到github
git add -A
git commit -m "init"
git push origin hexo
```

至此Hexo博客的内容，分支用于更新，master用于发布。已经不再冲突了。

## 新机器操作

环境准备  nodejs、git、hexo、ssh key

选择博客的存放目录，克隆到该目录。

cd 到博客目录下，npm install 、hexo g && hexo s 安装依赖，生成并启动博客。

```
1. 环境准备 nodejs、git、hexo、ssh key
2. 将新电脑ssh key添加到github账户上。
3. clone 仓库的hexo分支到本地。
4. cd ma***ci.github.io
5. 执行 npm install(因为仓库内的.gitignore忽略了依赖库)
6. 新建、更新或其他文章的改动。
7. 依次执行`git add .`、`git commit -m 'update'`、'git push origin hexo'指令。
8. 保证hexo分支版本最新。
9. 执行`hexo clean` ， `hexo d -g` 改动同步更新master分支。
```

之后每次在任何一台电脑上面的操作顺序应该是

```
1. git pull
2. git branch 查看当前分支
3. 依次执行`git add .`、`git commit -m 'update'`、'git push origin hexo'指令。
4. 执行`hexo clean` ， `hexo d -g` 改动同步更新master分支。
```

边验证，边记录。搞定~