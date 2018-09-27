---
title: Hexo+git搭建博客
date: 2018-06-23 00:49:25
categories:
- Hexo
tags: 
- Hexo
- Git
---

# Hexo和Git搭建博客

## 1. 准备工作

### 1.1 安装nodejs

  从[官网](https://nodejs.org/en/download/)下载最新的安装版本。Windows Installer (.msi) 64-bit
 
  这里也可以从[淘宝镜像](https://npm.taobao.org/)去下载。
 
  一路默认下一步安装。
  <!-- more -->

### 1.2 安装Hexo

 `npm install -g hexo`全局安装hexo。

 报错信息：

 Q：`npm WARN deprecated titlecase@1.1.2: no longer maintained`
 
 A：`npm -v`查看npm版本，`npm install -g npm `更新版本即可。

### 1.3 准备github帐号

注册github帐号

新建一个repository，名称为 `yourname.github.io`，我的帐号名为`ma***ci`，故我的repository为`ma***ci.github.io`。

### 1.4 安装git.
 
 从[官网](https://git-scm.com/download/win)选择64位下载。
 
 一路默认下一步安装。

## 2. 构建Hexo

### 2.1 环境检测

```
node -v // 检测node是否正常安装
hexo -v // 检测hexo是否正常安装
npm -v  // 检测npm 是否正常安装
git --version //检测git版本
```

### 2.2 初始化项目

切换到构建项目的路径，我的家庭电脑位置是`D:\Document\git_doc\git_blog`。

执行`hexo init` 构建项目

执行`hexo s`即可启动。*http://localhost:4000* 访问本地博客。

## 3.  Hexo 项目推送到Git

### 3.1 准备工作

####  设置Git的user name和email

```
git config --global user.name "ma***ci"
git config --global user.email "m***n_11@126.com"
```

#### github添加密钥

```
// 检查是否存在
cd ~/.ssh
ls
// 生成密钥
ssh-keygen -t rsa -C “m***n_11@126.com”
// 添加密钥到ssh-agent
eval "$(ssh-agent -s)"
// 添加生成的SSH key到ssh-agent
ssh-add ~/.ssh/id_rsa
// github.com 点击头像下方setting， 添加密钥，复制id_rsa.pub内容。
// 测试添加ssh是否成功
ssh -T git@github.com
```

#### 安装deploy扩展

在hexo的目录内，执行`npm install hexo-deployer-git --save` 安装部署的扩展。

### 3.2 配置_config.yml

```xml
deploy:
  type: git
  repo: git@github.com:ma***ci/ma***ci.github.io.git
  branch: master
```

### 3.3 推送

在博客的当前路径执行`hexo d -g` 推送到github。

查看github下的 `ma***ci.github.io` 是否已正常推送成功。

访问 *http//yourname.github.io*  ， 哈哈~ 是不是已经可以正常访问了？

## 4. 建议

npm 默认使用的`https://registry.npmjs.org/`源，建议使用国内镜像。

```
//临时切换
npm --registry https://registry.npm.taobao.org install express
//永久切换使用
npm config set registry https://registry.npm.taobao.org
//验证
npm config get registry
```

或者可以使用阿里的cnpm来下载

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

## 5. 参考文档

[Z皓-使用Hexo+Github一步步搭建属于自己的博客（基础）](https://www.cnblogs.com/fengxiongZz/p/7707219.html)