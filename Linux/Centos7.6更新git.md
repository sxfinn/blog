##  Centos升级git版本

本文讲述如何升级 centos 系统的 git 版本。高版本 git 增加了一些好用的功能，比如"git pull 支持指定项目目录"等。本文以 centos6/7 为例讲解。

> 本文参考了文档[centos 6.x/7.x 使用 yum 升级 git 版本 (opens new window)](https://blog.slogra.com/post-721.html)。



### 升级centos6/7的git版本

1. 安装 git 仓库

```text
# 如果是 CentOS 6 系统就安装这个吧
yum install http://opensource.wandisco.com/centos/6/git/x86_64/wandisco-git-release-6-1.noarch.rpm

# 如果是 CentOS 7 系统就安装下面两个之一吧
yum install http://opensource.wandisco.com/centos/7/git/x86_64/wandisco-git-release-7-1.noarch.rpm
# 或者
yum install http://opensource.wandisco.com/centos/7/git/x86_64/wandisco-git-release-7-2.noarch.rpm
```

2. 安装新版本git

​		yum install git



### 升级 centos7 的 git 版本

1. 卸载旧版本 git

   yum remove git

2. 安装 git 仓库

```shell
rpm -ivh http://opensource.wandisco.com/centos/7/git/x86_64/wandisco-git-release-7-1.noarch.rpm
```

3. 安装 高版本 git 

​		yum install git -y

4. 完整安装过程如下

```shell
[root@VM-4-9-centos ~]# rpm -ivh http://opensource.wandisco.com/centos/7/git/x86_64/wandisco-git-release-7-1.noarch.rpm
Retrieving http://opensource.wandisco.com/centos/7/git/x86_64/wandisco-git-release-7-1.noarch.rpm
warning: /var/tmp/rpm-tmp.bg3oRZ: Header V4 DSA/SHA1 Signature, key ID 3bbf077a: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:wandisco-git-release-7-1         ################################# [100%]
[root@VM-4-9-centos ~]# yum install git -y
Loaded plugins: fastestmirror, langpacks
Repository epel is listed more than once in the configuration
Loading mirror speeds from cached hostfile
WANdisco-git                                                                                                                                  | 2.9 kB  00:00:00     
WANdisco-git/7/x86_64/primary_db                                                                                                              | 153 kB  00:00:01     
Resolving Dependencies
--> Running transaction check
---> Package git.x86_64 0:2.31.1-1.WANdisco.469 will be installed
--> Processing Dependency: perl-Git = 2.31.1-1.WANdisco.469 for package: git-2.31.1-1.WANdisco.469.x86_64
--> Processing Dependency: perl(Git) for package: git-2.31.1-1.WANdisco.469.x86_64
--> Processing Dependency: perl(Digest::SHA) for package: git-2.31.1-1.WANdisco.469.x86_64
--> Processing Dependency: perl(Git::I18N) for package: git-2.31.1-1.WANdisco.469.x86_64
--> Running transaction check
---> Package perl-Digest-SHA.x86_64 1:5.85-4.el7 will be installed
--> Processing Dependency: perl(Digest::base) for package: 1:perl-Digest-SHA-5.85-4.el7.x86_64
---> Package perl-Git.noarch 0:2.31.1-1.WANdisco.469 will be installed
--> Running transaction check
---> Package perl-Digest.noarch 0:1.17-245.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

=====================================================================================================================================================================
 Package                                  Arch                            Version                                        Repository                             Size
=====================================================================================================================================================================
Installing:
 git                                      x86_64                          2.31.1-1.WANdisco.469                          WANdisco-git                          8.7 M
Installing for dependencies:
 perl-Digest                              noarch                          1.17-245.el7                                   os                                     23 k
 perl-Digest-SHA                          x86_64                          1:5.85-4.el7                                   os                                     58 k
 perl-Git                                 noarch                          2.31.1-1.WANdisco.469                          WANdisco-git                           23 k

Transaction Summary
=====================================================================================================================================================================
Install  1 Package (+3 Dependent packages)

Total download size: 8.8 M
Installed size: 41 M
Downloading packages:
(1/4): perl-Digest-1.17-245.el7.noarch.rpm                                                                                                    |  23 kB  00:00:00     
(2/4): perl-Digest-SHA-5.85-4.el7.x86_64.rpm                                                                                                  |  58 kB  00:00:00     
warning: /var/cache/yum/x86_64/7/WANdisco-git/packages/perl-Git-2.31.1-1.WANdisco.469.noarch.rpm: Header V4 DSA/SHA1 Signature, key ID 3bbf077a: NOKEY  --:--:-- ETA 
Public key for perl-Git-2.31.1-1.WANdisco.469.noarch.rpm is not installed
(3/4): perl-Git-2.31.1-1.WANdisco.469.noarch.rpm                                                                                              |  23 kB  00:00:00     
(4/4): git-2.31.1-1.WANdisco.469.x86_64.rpm                                                                                                   | 8.7 MB  00:00:03     
---------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                2.6 MB/s | 8.8 MB  00:00:03     
Retrieving key from file:///etc/pki/rpm-gpg/RPM-GPG-KEY-WANdisco
Importing GPG key 0x3BBF077A:
 Userid     : "WANdisco (http://WANdisco.com - We Make Software Happen...) <software-key@wandisco.com>"
 Fingerprint: 69c1 be83 da54 cbed 6889 72f8 e9f0 e922 3bbf 077a
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-WANdisco
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
Warning: RPMDB altered outside of yum.
  Installing : perl-Digest-1.17-245.el7.noarch                                                                                                                   1/4 
  Installing : 1:perl-Digest-SHA-5.85-4.el7.x86_64                                                                                                               2/4 
  Installing : git-2.31.1-1.WANdisco.469.x86_64                                                                                                                  3/4 
  Installing : perl-Git-2.31.1-1.WANdisco.469.noarch                                                                                                             4/4 
  Verifying  : perl-Git-2.31.1-1.WANdisco.469.noarch                                                                                                             1/4 
  Verifying  : perl-Digest-1.17-245.el7.noarch                                                                                                                   2/4 
  Verifying  : 1:perl-Digest-SHA-5.85-4.el7.x86_64                                                                                                               3/4 
  Verifying  : git-2.31.1-1.WANdisco.469.x86_64                                                                                                                  4/4 

Installed:
  git.x86_64 0:2.31.1-1.WANdisco.469                                                                                                                                 

Dependency Installed:
  perl-Digest.noarch 0:1.17-245.el7                   perl-Digest-SHA.x86_64 1:5.85-4.el7                   perl-Git.noarch 0:2.31.1-1.WANdisco.469                  

Complete!
[root@VM-4-9-centos ~]# git --version
git version 2.31.1
```



