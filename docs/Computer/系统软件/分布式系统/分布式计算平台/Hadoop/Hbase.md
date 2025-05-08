---
title: Hbase
data: 2025-5-9
categories:
    - 计算机科学
    - 系统软件
    - 分布式系统
    - 分布式计算平台
    - Hadoop集群
---
# Hbase


## 环境搭建


### 完全分布式



##### 安装Hbase并配置

将`Hbase`传入`/opt/software`
```shell
# 进入/opt/software目录
[user@hostname]$ cd /opt/software
# 解压hbase包
[user@hostname]$ tar -zxvf hbase-2.4.11-bin.tar.gz -C /opt/module/

# 进入/opt/module/目录
[user@hostname]$ cd /opt/module/
# 修改hbase包名
[user@hostname]$ mv hbase-2.4.11/ hbase

# 修改环境变量
[user@hostname]$ sudo vim /etc/profile.d/my_env.sh
# 分发环境变量
[user@hostname]$ sudo /home/user/bin/xsync /etc/profile.d/my_env.sh



```

`hbase-site.xml`
```xml

<property>
    <name>hbase.cluster.distributed</name>
    <value>true</value>
</property>
<property>
    <name>hbase.zookeeper.quorum</name>
    <value>hadoop102,hadoop103,hadoop104</value>
    <description>The directory shared by RegionServers.</description>
</property>
<property>
    <name>hbase.rootdir</name>
    <value>hdfs://hadoop102:8020/hbase</value>
    <description>The directory shared by RegionServers.</description>
</property>


```




```shell
mv /opt/module/hbase/lib/client-facing-thirdparty/slf4j-reload4j-1.7.33.jar /opt/module/hbase/lib/client-facing-thirdparty/slf4j-reload4j-1.7.33.jar.bak

```