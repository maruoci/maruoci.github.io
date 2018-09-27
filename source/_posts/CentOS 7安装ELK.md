---
title: CentOS 7下安装ELK
date: 2018-09-27 14:49:25
categories:
- ELK
tags: 
- ELK
- CentOS
---

> ELK是三个开源软件的缩写，分别表示：Elasticsearch , Logstash, Kibana , 新增了一个FileBeat。
>  + Elasticsearch是个开源分布式搜索引擎，提供搜集、分析、存储数据三大功能。它的特点有：分布式，零配置，自动发现，索引自动分片，索引副本机制，restful风格接口，多数据源，自动搜索负载等。
>  + Logstash 主要是用来日志的搜集、分析、过滤日志的工具，支持大量的数据获取方式。一般工作方式为c/s架构，client端安装在需要收集日志的主机上，server端负责将收到的各节点日志进行过滤、修改等操作在一并发往elasticsearch上去。
>  + Kibana 也是一个开源和免费的工具，Kibana可以为 Logstash 和 ElasticSearch 提供的日志分析友好的 Web 界面，可以帮助汇总、分析和搜索重要数据日志。
>  + Filebeat它是一个轻量级的日志收集处理工具,适合于在各个服务器上搜集日志后传输给Logstash。目前Beats包含四种工具：
>    + Packetbeat（搜集网络流量数据）
>    + Topbeat（搜集系统、进程和文件系统级别的 CPU 和内存使用情况等数据）
>    + Filebeat（搜集文件数据）
>    + Winlogbeat（搜集 Windows 事件日志数据）

<!-- more -->


## 环境

  ```
  # 当前操作系统版本
  [root@DJI elk]#  cat /etc/redhat-release
  CentOS release 6.9 (Final)
  # 查看JDK版本
  [root@DJI elk]# java -version
  java version "1.8.0_131"
  Java(TM) SE Runtime Environment (build 1.8.0_131-b11)
  Java HotSpot(TM) 64-Bit Server VM (build 25.131-b11, mixed mode)

  # 查看JDK安装目录
  [root@localhost ~]# echo $JAVA_HOME$
  /usr/local/jdk1.8.0_181$
  ```

## 准备工作
  
  **JDK**
  
  ELK 官方推荐版本是JDK8 , 本机已安装，忽略此步。
  
  **创建用户**

  ElasticSerach要求以非root身份启动，所以我们要创建一个用户
  
  ```
  [root@DJI elk]# groupadd elasticsearch
  [root@DJI elk]# useradd elasticsearch -g elasticsearch
  [root@DJI elk]# chown -R elasticsearch.elasticsearch /opt/elk/elasticsearch-6.2.3
  ```
  
  **安装包准备**

  ```
  [root@DJI ~]# cd /opt/elk/
  [root@DJI elk]# wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.2.3.tar.gz
  [root@DJI elk]# wget https://artifacts.elastic.co/downloads/kibana/kibana-6.2.3-linux-x86_64.tar.gz
  [root@DJI elk]# wget https://artifacts.elastic.co/downloads/logstash/logstash-6.2.3.tar.gz
  [root@DJI elk]# tar -zvxf elasticsearch-6.2.3.tar.gz  
  [root@DJI elk]# tar -zvxf kibana-6.2.3-linux-x86_64.tar.gz 
  [root@DJI elk]# tar -zvxf logstash-6.2.3.tar.gz
  ```
  
  **关闭防火墙并禁止自启**
  
  ```
    # CentOS6
    [root@DJI elk]# service iptables stop
    [root@DJI elk]# chkconfig iptables off 
    # CentOS7
    [root@DJI elk]# systemctl stop firewalld
    [root@DJI elk]# systemctl disable firewalld
  ```

## 启动

### 启动elasticsearch
  
  ```
  [root@DJI elk]# su elasticsearch
  [root@DJI elk]# cd elasticsearch-6.2.3
  [elasticsearch@DJI elasticsearch-6.2.3]$ bin/elasticsearch -d
  [elasticsearch@DJI elasticsearch-6.2.3]$ tail -f /opt/elk/elasticsearch-6.2.3/logs/elasticsearch.log
  ```
  
  验证启动
  
  ```
  [root@DJI elk]# curl 127.0.0.1:9200
  {
    "name" : "vXT66DG",
    "cluster_name" : "elasticsearch",
    "cluster_uuid" : "iNsOaE3ITTKma3PSD3RFQQ",
    "version" : {
      "number" : "6.2.3",
      "build_hash" : "c59ff00",
      "build_date" : "2018-03-13T10:06:29.741383Z",
      "build_snapshot" : false,
      "lucene_version" : "7.2.1",
      "minimum_wire_compatibility_version" : "5.6.0",
      "minimum_index_compatibility_version" : "5.0.0"
    },
    "tagline" : "You Know, for Search"
  }
  [root@DJI elk]# 
  ```
  
  其他配置
  
  1. 目前访问使用127.0.0.1访问，如果需要支持ip访问，需要修改配置文件。
  
  ```
  # conf/elasticsearch.yml 
  network.host: 192.168.1.72
  ```
  
  重新启动(elasticsearch 用户)，elasticsearch报错。
  
  ```
  [2018-09-27T11:07:52,254][ERROR][o.e.b.Bootstrap          ] [Tt56N7M] node validation exception
  [2] bootstrap checks failed
  [1]: max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]
  [2]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
  ```
  
  **[1]: max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]**
  
  解决办法：切换root用户，` /etc/security/limits.conf`中添加配置。 该配置无需重启系统，使用elasticsearch重登录即可。
  
  ```
  * soft nofile 65536
  * hard nofile 131072
  * soft nproc 2048
  * hard nproc 4096
  ```
  
  **[2]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]**
  
  解决办法：切换root用户，`/etc/sysctl.conf`中添加`vm.max_map_count = 655360`执行`sysctl -p`
  
  到此，重启成功。
  
  
### 启动Logstash
  
  添加配置
  
  ```
  [root@DJI logstash-6.2.3]# vi logstash-tcp-input.conf
  ## conf 内容
  input {
      tcp {
          port => 4560  # 监听4560端口作为输入
          codec => json_lines
      }
  }
  # 数据过滤
  filter {
    if "_jsonparsefailure" in [tags] {
        drop { }
    }
  }
  # 输出配置为本机的9200端口，这个是ElasticSerach服务的监听端口  
  output {
      elasticsearch {
          hosts => ["192.168.1.72:9200"]
          action => "index"
          index => "%{[appname]}"
      }
  }
  ## conf 内容
  ```
  
  启动logstash
  
  ```
  [root@DJI logstash-6.2.3]# nohup bin/logstash -f default.conf –config.reload.automatic &
  [root@DJI logstash-6.2.3]# tail -f logs/logstash-plain.log
  ```

### 启动Kibana  
  
  修改配置
  
  ```
  # config/kibana.yml
  # server.host: "localhost"
  # elasticsearch.url: "http://localhost:9200"
  修改为
  # server.host: "192.168.1.72"
  # elasticsearch.url: "http://192.168.1.72:9200"
  ```
  
  `nohup bin/kibana & `启动
  
  访问 http://192.168.1.72:5601 可看到Kibana的页面。

## 小结

  到此ELK的安装就结束了。

## 参考资料
  
 + [CentOS7搭建ELK-6.2.3版本](https://blog.csdn.net/boling_cavalry/article/details/79836171)
 + [ELK原理与介绍](https://www.cnblogs.com/aresxin/p/8035137.html)