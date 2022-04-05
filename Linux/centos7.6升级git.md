#  centos升级git版本

本文讲述如何升级 centos 系统的 git 版本。高版本 git 增加了一些好用的功能，比如"git pull 支持指定项目目录"等。本文以 centos7 为例讲解。

> 本文参考了文档[centos 6.x/7.x 使用 yum 升级 git 版本 (opens new window)](https://blog.slogra.com/post-721.html)。



### 升级 centos7 的 git 版本

1. 卸载旧版本 git

   yum remove git

2. 安装 git 仓库

```shell
rpm -ivh http://opensource.wandisco.com/centos/7/git/x86_64/wandisco-git-release-7-1.noarch.rpm
```

3. 安装 高版本 git 

yum install git -y

4. 完整安装过程如下

```shell
[root@iZ2ze7011et12xez70sp3dZ ~]# rpm -ivh http://opensource.wandisco.com/centos/7/git/x86_64/wandisco-git-release-7-1.noarch.rpm
获取http://opensource.wandisco.com/centos/7/git/x86_64/wandisco-git-release-7-1.noarch.rpm
警告：/var/tmp/rpm-tmp.92db5f: 头V4 DSA/SHA1 Signature, 密钥 ID 3bbf077a: NOKEY
准备中...                          ################################# [100%]
正在升级/安装...
   1:wandisco-git-release-7-1         ################################# [100%]
[root@iZ2ze7011et12xez70sp3dZ ~]# yum install git -y
已加载插件：fastestmirror
Loading mirror speeds from cached hostfile
WANdisco-git                                                                                                                                                  | 2.9 kB  00:00:00
WANdisco-git/7/x86_64/primary_db                                                                                                                              | 126 kB  00:00:07
正在解决依赖关系
--> 正在检查事务
---> 软件包 git.x86_64.0.2.22.0-1.WANdisco.437 将被 安装
--> 正在处理依赖关系 perl-Git = 2.22.0-1.WANdisco.437，它被软件包 git-2.22.0-1.WANdisco.437.x86_64 需要
--> 正在处理依赖关系 perl(Git)，它被软件包 git-2.22.0-1.WANdisco.437.x86_64 需要
--> 正在处理依赖关系 perl(Git::I18N)，它被软件包 git-2.22.0-1.WANdisco.437.x86_64 需要
--> 正在检查事务
---> 软件包 perl-Git.noarch.0.2.22.0-1.WANdisco.437 将被 安装
--> 解决依赖关系完成

依赖关系解决

=====================================================================================================================================================================================
 Package                                架构                                 版本                                                   源                                          大小
=====================================================================================================================================================================================
正在安装:
 git                                    x86_64                               2.22.0-1.WANdisco.437                                  WANdisco-git                               8.4 M
为依赖而安装:
 perl-Git                               noarch                               2.22.0-1.WANdisco.437                                  WANdisco-git                                23 k

事务概要
=====================================================================================================================================================================================
安装  1 软件包 (+1 依赖软件包)

总下载量：8.4 M
安装大小：42 M
Downloading packages:
警告：/var/cache/yum/x86_64/7/WANdisco-git/packages/perl-Git-2.22.0-1.WANdisco.437.noarch.rpm: 头V4 DSA/SHA1 Signature, 密钥 ID 3bbf077a: NOKEY    ] 6.1 kB/s |  49 kB  00:23:30 ETA
perl-Git-2.22.0-1.WANdisco.437.noarch.rpm 的公钥尚未安装
(1/2): perl-Git-2.22.0-1.WANdisco.437.noarch.rpm                                                                                                              |  23 kB  00:00:03
(2/2): git-2.22.0-1.WANdisco.437.x86_64.rpm                                                                                                                   | 8.4 MB  00:10:27
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
总计                                                                                                                                                  14 kB/s | 8.4 MB  00:10:27
从 file:///etc/pki/rpm-gpg/RPM-GPG-KEY-WANdisco 检索密钥
导入 GPG key 0x3BBF077A:
 用户ID     : "WANdisco (http://WANdisco.com - We Make Software Happen...) <software-key@wandisco.com>"
 指纹       : 69c1 be83 da54 cbed 6889 72f8 e9f0 e922 3bbf 077a
 来自       : /etc/pki/rpm-gpg/RPM-GPG-KEY-WANdisco
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
警告：RPM 数据库已被非 yum 程序修改。
  正在安装    : perl-Git-2.22.0-1.WANdisco.437.noarch                                                                                                                            1/2
  正在安装    : git-2.22.0-1.WANdisco.437.x86_64                                                                                                                                 2/2
  验证中      : git-2.22.0-1.WANdisco.437.x86_64                                                                                                                                 1/2
  验证中      : perl-Git-2.22.0-1.WANdisco.437.noarch                                                                                                                            2/2

已安装:
  git.x86_64 0:2.22.0-1.WANdisco.437

作为依赖被安装:
  perl-Git.noarch 0:2.22.0-1.WANdisco.437

完毕！
[root@iZ2ze7011et12xez70sp3dZ ~]#     git --version
git version 2.22.0
[root@iZ2ze7011et12xez70sp3dZ ~]#

```