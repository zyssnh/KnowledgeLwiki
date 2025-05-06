---
title: Hadoop
data: 2025-5-6
categories:
    - 计算机科学
    - 系统软件
    - 分布式系统
    - 分布式计算平台
---

# hadoop

<!-- 
```yaml
title: hadoop
data: 2025-5-6
categories: 
    - 计算机科学
    - 系统软件
    - 分布式系统
    - 分布式计算平台
``` 
-->

## 环境搭建

### 完全分布式

VMware

CentOS

CentOS-7

- 最小系统
> 安装依赖 : `net-tool` `vim`

net-tool:

vim:


- 桌面版

##### IP与主机名称设置

###### IP设置

修改`ip`地址
```shell
[user@hostname]$ su root
[root@hostname]\# vim /etc/sysconfig/network-scripts/ifcfg-ens33
# 增加与修改
BOOTPROTO="static"
IPADDR=192.168.10.100
GATEWAY=192.168.10.2
DNS1=192.168.10.2
```

###### 主机名称修改

修改`hostname`名称
```shell
[root@hostname]\# vim /etc/hostname
# 增加或修改
hadoop100
```

###### 主机名称映射
```shell
[root@hostname]\# vim /etc/hosts
# 增加
192.168.10.100  hadoop100
192.168.10.101  hadoop101
192.168.10.102  hadoop102
192.168.10.103  hadoop103
192.168.10.104  hadoop104
192.168.10.105  hadoop105
192.168.10.106  hadoop106
192.168.10.107  hadoop107
192.168.10.108  hadoop108

```

> IP与主机名称设置结束 -> 使用`reboot`重启系统  使用`ping`测试网络

###### Windows主机映射文件
```powershell
# 打开
PS C:\Windows\System32\drivers\etc\hosts
# 添加
192.168.10.100  hadoop100
192.168.10.101  hadoop101
192.168.10.102  hadoop102
192.168.10.103  hadoop103
192.168.10.104  hadoop104
192.168.10.105  hadoop105
192.168.10.106  hadoop106
192.168.10.107  hadoop107
192.168.10.108  hadoop108
```

##### 安装软件仓库`epel-release`
> `epel-release` : `Extra Packages for Enterprise Linux`

```shell
[root@hostname]\# yum install -y epel-release
```

##### 关闭防火墙
```shell
# 关闭防火墙
[root@hostname]\# systemctl stop firewalld

# 关闭自启防火墙
[root@hostname]\# systemctl disable firewalld.serviece
```

##### 创建用户
```shell
[root@hostname]\# useradd username
[root@hostname]\# passwd password
```

- 为所建用户添加`sudo`权限
```shell
[root@hostname]\# vim /etc/sudoers
# 添加
## Allow root to run any commands anywhere
root      ALL=(ALL)       ALL

## Allows people in group whell to run all commands
%whell    ALL=(ALL)       ALL
username  ALL=(ALL)       NOPASSWD:ALL  # 添加

```

##### 创建文件夹
```shell
[root@hostname]\# mkdir /opt/module
[root@hostname]\# mkdir /opt/software
# 修改文件夹的所有者与所属组
[root@hostname]\# chown username:username /opt/module
[root@hostname]\# chown username:username /opt/software
```

##### 卸载系统JDK
```shell
[root@hostname]\# rpm -qa | grep -i java | xargs -n1 rpm -e --nodeps 
```
- `rpm -qa` : 查询所安装的全部`rpm`软件包
- `grep -i` : 忽略大小写
- `xargs -n1` : 表示每次传递一个参数
- `rpm -e --nodeps` : 强制删除软件



> `reboot`重启系统





## 报错解决

###### 软件仓库安装失败

```shell
[root@hostname]\# yum install -y epel-release
已加载插件：fastestmirror, langpacks
Loading mirror speeds from cached hostfile
Could not retrieve mirrorlist http://mirrorlist.centos.org/?release=7&arch=x86_64&repo=os&infra=stock error was
14: curl#6 - "Could not resolve host: mirrorlist.centos.org; 未知的错误"

  One of the configured repositories failed (未知),
  and yum doesn't have enough cached data to continue. At this point the only
  safe thing yum can do is fail. There are a few ways to work "fix" this:
      1. Contact the upstream for the repository and get them to fix the problem.
      2. Reconfigure the baseurl/etc. for the repository, to point to a working
        upstream. This is most often useful if you are using a newer
        distribution release than is supported by the repository (and the
        packages for the previous distribution release still work).
      3. Run the command with the repository temporarily disabled
            yum --disablerepo=<repoid> ...
      4. Disable the repository permanently, so yum won't use it by default. Yum
        will then just ignore the repository until you permanently enable it
        again or use --enablerepo for temporary usage:

            yum-config-manager --disable <repoid>
        or
            subscription-manager repos --disable=<repoid>
      5. Configure the failing repository to be skipped, if it is unavailable.
        Note that yum will try to contact the repo. when it runs most commands,
        so will have to try and fail each time (and thus. yum will be be much
        slower). If it is a very temporary problem though, this is often a nice
        compromise:
            yum-config-manager --save --setopt=<repoid>.skip_if_unavailable=true
Cannot find a valid baseurl for repo: base/7/x86_64

```

1. 手动创建`CentOS-Base.repo`
```shell
# 备份并删除原有错误配置
[root@hostname]\# mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak

# 创建新的 CentOS-Base.repo
[root@hostname]\# vim /etc/yum.repos.d/CentOS-Base.repo

# 适用于 CentOS 7 Vault
[base]
name=CentOS-7 - Base (Vault)
baseurl=http://vault.centos.org/centos/7/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
enabled=1

[updates]
name=CentOS-7 - Updates (Vault)
baseurl=http://vault.centos.org/centos/7/updates/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
enabled=1

[extras]
name=CentOS-7 - Extras (Vault)
baseurl=http://vault.centos.org/centos/7/extras/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
enabled=1

```

2. 验证仓库配置
```shell
# 清理并重建缓存
[root@hostname]\# yum clean all
[root@hostname]\# yum makecache
```

3. 安装`EPEL`仓库
```shell
[root@hostname]\# yum install -y epel-release
```