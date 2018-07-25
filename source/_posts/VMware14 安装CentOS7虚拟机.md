---
title: VMware 安装CentOS7 虚拟机
date: 2018-07-25 10:15:10
categories:
- Linux
tags:
- Linux
---

#### 软件版本

  宿主机系统： Win7 64位 旗舰版

  VMware14
  
  CentOS-7-x86_64-Minimal-1804.iso
  
  网络模式预备选择Nat模式。
  
  简要描述下三种链接方式的区别。
  
  + 桥接: VMWare会虚拟一块网卡和真正的物理网卡桥接。物理网卡可以上网，那么桥接的软网卡也没有问题。(两者必须在同一网段)
  
  + NAT (推荐): 虚拟机不占用主机所在局域网的ip，使用主机的NAT功能访问局域网和互联网，意味着虚拟机可以访问局域网中的其他电脑，但是其他电脑不知道虚拟机的存在。
  
  + Host-only: 不可以上网
  
#### 安装 CentOS7

  创建虚拟机
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-25/73770783.jpg)
  
  选择“典型安装” 
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-25/64288950.jpg)
  
  选择稍后安装“操作系统”
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-25/54478389.jpg)
  
  选择Linux，版本CentOS 7 64位
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-25/73595117.jpg)
  
  给虚拟机命名，并选择存储位置
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-25/40523175.jpg)
  
  默认虚拟机磁盘 20G，下一步
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-25/31690010.jpg)
  
  自定义硬件配置
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-25/43605820.jpg)
  
  CD中，选择下载好的ISO文件
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-25/74905857.jpg)
  
  点击完成。

#### 配置Nat网络

  选择“编辑”，选择“虚拟网络编辑器”。
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-25/78314349.jpg)
  
  设置“子网” 192.168.19.0 、“子网掩码” 点击应用，然后选择“Nat设置”
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-25/44713366.jpg)

  设置网关 192.168.19.1， 点击确定，完成网络设置。
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-25/35747283.jpg)          
    
  打开网络共享中心，更改适配器设置，找到VMnet8网络
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-25/62825701.jpg)

  右键属性，选择Ipv4，设置VMnet8的ip为刚才虚拟机内设置的网关地址 192.168.19.1。
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-25/42804729.jpg)
  
  完成网络配置。

#### CentOS7 安装 与 网络设置

  开机选择 install CentOS 7 或默认等待
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-25/31857483.jpg)  
  
  等待系统进入到该页面，选择中文，简体中文，下一步
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-25/78599787.jpg)
  
  分别选择“安装位置” 与 “网络和主机名” 进行设置
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-25/72962113.jpg)
  
  选择 “网络和主机名” ，打开 以太网，点击完成
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-25/82939841.jpg)
  
  选择“安装位置 默认配置 自动分区，点击完成
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-25/24162467.jpg)
  
  回到“安装信息摘要”页面，点击“开始安装” 设置Root密码
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-25/83033041.jpg)
  
  root密码，我设置的 root/root，简单密码需要点击两次确认 才可以。
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-25/82140660.jpg)
  
  如果需要创建其他用户，可以再点击创建用户。创建一个其他的用户。这里我就不设置了。等待安装完成。
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-25/44370719.jpg)
  
  重启，我们开始进入CentOS系统进行内部设置吧

  启动界面，输入我们上面设置的root账号与密码：root
  
  `vi /etc/sysconfig/network-scripts/ifcfg-ens33`修改网络配置
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-25/24576548.jpg)
  
  保存，并`network service restart`重启。
  
  `ping www.baidu.com`看看是不是已经可以上网啦?
  
  ![](http://pbsg2r9io.bkt.clouddn.com/18-7-25/41419509.jpg)

  到这里，CentOS7 就安装完成了。
  
#### 遇到的问题

  宿主机使用ssh连接虚拟机时，连接过程特别慢。度娘了一下，需要修改sshd的配置。
  
  `vi /etc/ssh/sshd_config`，放开UseDNS的注释，并将其设置为 no ，重启sshd服务即可。  

#### 参考地址

  + [VMware配置网络的3种方式：NAT、Host-Only、Bridged](https://blog.csdn.net/u014726937/article/details/52768463)
  + [新手学Linux：在VMware14中安装CentOS7详细教程](https://blog.csdn.net/yiyihuazi/article/details/78557216)
  + [解决ssh访问linux虚拟机特别慢](https://blog.csdn.net/gxdvip/article/details/50977652)
  


      