---
title: Zookeeper
data: 2025-5-9
categories:
    - 计算机科学
    - 系统软件
    - 分布式系统
    - 分布式计算平台
    - Hadoop集群
---
# Zookeeper

## 环境搭建

### 本地式

### 伪分布式

### 完全分布式

##### 安装zookeeper与配置

将`Zookeeper`传入`/opt/software`
```shell
[user@hostname]$ cd /opt/software
[user@hostname]$ tar -zxvf apache-zookeeper-3.5.7-bin.tar.gz -C /opt/module/
[user@hostname]$ mv apache-zookeeper-3.5.7-bin/ zookeeper-3.5.7


[user@hostname]$ 
[user@hostname]$ myid
[user@hostname]$ xsync zk
[user@hostname]$ 



[user@hostname]$ cd zookeeper-3.5.7
[user@hostname]$ mkdir zkData
[user@hostname]$ mv conf/zoo_sample.cfg zoo.cfg
[user@hostname]$ vim zoo.cfg
# 添加或修改
dataDir=/opt/module/zookeeper-3.5.7/zkData

server.2=hadoop102:2888:3888
server.3=hadoop103:2888:3888
server.4=hadoop104:2888:3888

[user@hostname]$ xsync zoo.cfg
```