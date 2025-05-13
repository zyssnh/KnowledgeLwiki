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

## 基础概念
> `Hbase : 定义 : 应用场景 : Hadoop生态`



## 数据模型
> HBase 是一种基于列存储的分布式非关系型数据库 : 特点

### 表 : Table

### 行 : Row

### 列族 : Column Family

### 列限定符 : Column Qualifier

### 单元格 : Cell

### 时间戳 : Timestamp


## 环境搭建


### 完全分布式



##### 安装Hbase并配置

> 将`Hbase`传入`/opt/software`

**进入解压并修改其名**
```bash
# 进入/opt/software目录
[user@hostname]$ cd /opt/software
# 解压hbase包
[user@hostname]$ tar -zxvf hbase-2.4.11-bin.tar.gz -C /opt/module/
```


```bash
# 进入/opt/module/目录
[user@hostname]$ cd /opt/module/
# 修改hbase包名
[user@hostname]$ mv hbase-2.4.11/ hbase

# 修改环境变量
[user@hostname]$ sudo vim /etc/profile.d/my_env.sh
# 分发环境变量
[user@hostname]$ sudo /home/user/bin/xsync /etc/profile.d/my_env.sh

```

**`hbase-site.xml`**
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

**启动hbase**
```bash
[user@hostname]$ start-hbase.sh
```



> !: Jar包冲突解决方法
```bash
mv /opt/module/hbase/lib/client-facing-thirdparty/slf4j-reload4j-1.7.33.jar /opt/module/hbase/lib/client-facing-thirdparty/slf4j-reload4j-1.7.33.jar.bak

```


## Hbase Shell
>

- **hbase shell 基础命令**
```bash
# 进入 hbase shell
[user@hostname]$ hbase shell

# 版本信息
hbase:xxx:x> version

# 状态信息
hbase:xxx:x> status

# 当前系统用户
hbase:xxx:x> whoami

# 帮助
hbase:xxx:x> help

# 退出
hbase:xxx:x> quit
```

### Hbase DDL
> `DDL : Data Definition Language : 数据定义语言`

- **创建表**
```shell
hbase:xxx:x> create 'TableName', 'ColumnFamilyName1', 'ColumnFamilyName2', ...
```
- **查看表结构**
```shell
hbase:xxx:x> describe 'TableName'
hbase:xxx:x> desc 'TableName'
```

- **操作表结构**
```shell
hbase:xxx:x> alter 'TableName', Attribute => xxx, ...
# 同时操作多个
hbase:xxx:x> alter 'TableName', {Attribute => xxx, ...}, {Attribute => xxx, ...}, ...
```

- **查看表的状态**
```shell
# 是否启用
hbase:xxx:x> is_enabled 'TableName'

# 是否禁用
hbase:xxx:x> is_disabled 'TableName'
```

- **操作表的状态**
```shell
# 启用
hbase:xxx:x> enable 'TableName'

# 禁用
hbase:xxx:x> disable 'TableName'
```


- **删除表**
```shell
hbase:xxx:x> drop 'TableName'
```

### Hbase DML
> `DML : Data Manipulation Language : 数据操纵语言`

- **上传数据至表中**
```shell
hbase:xxx:x> put 'TableName', 'RowKey', 'ColumnFamily : Column', 'Value'
```

- **查看表中数据**
```shell
# 查看表中所有数据
hbase:xxx:x> scan 'TableName'

# 查看表中条件数据
hbase:xxx:x> scan 'TableName', COLUMNS => 'ColumnFamilyName : ColumnName'
hbase:xxx:x> scan 'TableName', COLUMNS => ['ColumnFamilyName : ColumnName','ColumnFamilyName : ColumnName',...]
```

- **获得单元格的值**
```shell
hbase:xxx:x> get 'TableName', 'RowKey', 'ColumnFamily : Column', 'ColumnFamily : Column', ...
```

- **统计表中记录数**
```shell
hbase:xxx:x> count 'TableName'
```

- **计数器**
```shell
# 计数器操作
hbase:xxx:x> incr 'TableName', 'RowKey', 'ColumnFamily : Column', CountValue

# 查看计数器
hbase:xxx:x> get_counter 'TableName', 'RowKey', 'ColumnFamily : Column'
```

- **删除数据**
```shell
# 删除某列的单元格
hbase:xxx:x> delete 'TableName', 'RowKey', 'ColumnFamily : ColumnName'

# 删除整行数据
hbase:xxx:x> deleteall 'TableName', 'RowKey'

# 清空整个表
hbase:xxx:x> truncate 'TableName'
```


## Hbase API
>

## 数据管理与运维
>

### 快照管理

- **创建快照**
```shell
hbase:xxx:x> snapshot 'TableName', 'SnapshotName'
```

- **显示快照**
```shell
hbase:xxx:x> list_snapshot

# 可使用正则表达式
hbase:xxx:x> list_snapshot 'RegularExpression'
```

- **生成新表**
```shell
hbase:xxx:x> clone_snapshot 'SnapshotName', 'NewTableName'
```

- **恢复快照**
> `! 恢复快照需要禁用表`
```shell
hbase:xxx:x> restore_snapshot 'SnapshotName'
```

- **删除快照**
```shell
hbase:xxx:x> delete_snapshot 'SnapshotName'
```

## 进阶...
>