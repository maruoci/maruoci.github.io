---
title: Docker基础(一)
date: 2018-07-23 14:55:10
categories:
- Docker学习
tags:
- Docker
---

### 1. Docker 的核心

  + 镜像(Image)
  
  + 容器(Container)
  
  + 仓库(Repository)
  
  镜像：
  
  容器：
  
  仓库：
  
### 2. Docker 的安装

#### 2.1 基于CentOS的安装

  + 先更新yum 软件源缓存 `sudo yum update `
  
  + 安装 docker-engine `sudo yum install -y docker-engine`
  
  + 验证Docker安装 `docker version`
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-23/74528464.jpg)
  

### 3. 使用Docker镜像

#### 3.1 获取镜像 
  
  `docker pull NAME[: TAG]`  
  
  + NAME： 镜像名。如：redis 、ubuntu等。
  + TAG ： 镜像标签，一般指版本信息，默认不填时去latest标签。

#### 3.2 查看镜像信息

  使用 `docker images` 查看本地主机的已有镜像的基本信息。
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-23/58994961.jpg)  
  
  + repository : 镜像所属仓库
  + tag :   镜像标签
  + image_id : 镜像ID
  + created: 镜像的最后更新时间
  + size : 镜像的大小
  
  `docker images` 支持更多选项，可使用 `man docker images`来查看

#### 3.3 查找镜像

  `docker search` 来查找镜像
  
  `docker serach redis`
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-23/33413710.jpg)
  
  返回信息包含了镜像名字、描述、星际、是否官方创建、是否自动创建等信息。

  其他选项，`is-automated`是否自动创建，`stars` 镜像的星级。
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-23/17399743.jpg)
  
  `docker search --filter=stars=3 --filter=is-automated=true redis` 添加过滤条件
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-23/16545980.jpg)
  
#### 3.4 删除镜像

  `docker rmi [-f|--force] [--help] [--no-prune] IMAGE [IMAGE...] `
  
  -f| --force : 强制删除选项，默认不会删除有容器运行的镜像，除非配置该选项。
  
  IMAGE[IMAGE..]: 删除镜像可以按tag 或 ID 来过滤。
  

#### 3.5 其他命令

  + `docker tag ubuntu:latest myubuntu:latest `可以为镜像添加标签
  + `docker inspect redis ` 可以查看镜像的详细信息
  + `docker inspect redis -f {{".Architecture"}} `可以获取镜像特定的字段信息
  + `docker history redis:latest` 可以查看镜像历史

#### 3.6 创建镜像

  创建镜像有三种方式，基于已有镜像的容器创建，基于本地模板导入，基于Dockerfile创建。
  
  此处留到后面再进行补充。    

#### 3.7 保存与载入镜像

  `docker save [-o | --output[=OUTPUT]] redis:tag` 保存镜像
  
  `docker load [-o | --input[=INPUT]]` 载入镜像

#### 3.8 上传镜像

  `//TODO`    
  
### 4. 操作docker容器

#### 4.1 创建容器

  `docker create -it redis `可以创建一个容器，新建容器是停止状态
  
  create 命令 选项参数比较复杂，它的参数主要包含容器运行模式相关、容器环境配置相关、容器资源限制和安全保护相关。
  
  `docker start myredis` 可以启动一个redis容器
  
  `docker run [OPTIONS] IMAGE [COMMAND] [ARG...]` 可以直接创建一个容器并启动它。
  
  `docker run --name container-name -d image-name` --name 指定新建容器名字，-d 容器后台运行并打印容器id

#### 4.2 中止容器

  `docker stop [--help] [-t|--time[=10]] CONTAINER [CONTAINER...]`
  
  docker 停止某容器，等待一段超时时间（默认10秒）后，会kill掉容器。
  
  `docker kill [--help] [-s|--signal[="KILL"]] CONTAINER [CONTAINER...]`
  
  docker 直接kill掉容器
  
  `docker restart [--help] [-t|--time[=10]] CONTAINER [CONTAINER...]`
  
  docker restart 重启容器

#### 4.3 进入容器

  进入docker容器有三种方式 `attact` 、`exec`
  
  `docker attach [--detach-keys[=[]]] [--no-stdin] [--sig-proxy[=true]] CONTAINER`
  
  --detach-keys: 指定退出容器的快捷键，默认是 ctrl+p 或 ctrl+q
  
  --no-stdin: 是否关闭标准输入，默认打开
  
  --sig-proxy[=true]: 是否代理收到的系统信号给应用进程，默认true
  
  `docker exec [-d|--detach] [--detach-keys[=[]]] [-e|--env[=[]]] [-i|--interactive] [--privileged] [-t|--tty] [-u|--user[=USER]] CONTAINER COMMAND [ARG...]`
  
  -i|--interactive : 打开标准输入接受用户输入命令，默认false
  
  --privileged: 是否给执行命令以高权限，默认false
  
  -t|--tty : 分配伪终端，默认false
  
  `docker exec -it my-redis bash ` 保持输入打开并分配一个伪终端，启动一个bash。
  
  这个命令是对容器常用的操作方式。
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-23/91821147.jpg)
  
  还要第三种通过nsenter链接容器。

#### 4.4 删除容器  
  
  `docker rm [-f|--force] [-l|--link] [-v|--volumes] CONTAINER [CONTAINER...]`
  
#### 4.5 导入和导出容器

  `docker export [--help] [-o|--output[=""]] CONTAINER`  和镜像的删除差不多
  
  `docker import [-c|--change[=[]]] [-m|--message[=MESSAGE]] [--help] file|URL|-[REPOSITORY[:TAG]]`
  
### 5 疑问

  镜像与容器的区别    