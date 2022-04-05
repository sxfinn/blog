# 2022-04-04-

### 摘要
> Centos7.x将Python2升级到Python3

### 总结

> 

目录
---
[TOC]

------

#### 1、查看Python版本

```shell
python -V
```



#### 2、更新[yum](https://so.csdn.net/so/search?q=yum&spm=1001.2101.3001.7020)源

```shell
yum update
```



#### 3、安装依赖

```shell
yum install yum-utils

yum-builddep python
```



#### 4、下载python

```shell
wget https://www.python.org/ftp/python/3.8.0/Python-3.8.0.tgz
```



#### 5、安装Python相关依赖

```shell
yum -y install zlib-devel bzip2-devel openssl-devel ncursesdevelsqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel libffi-devel
```



#### 6、安装c，c++

```shell
yum -y install gcc g++
```



#### 7、创建安装目录

```shell
mkdir /usr/local/python3
```



#### 8、解压

```shell
tar xf Python-3.7.5.tgz
```



#### 9、编译

```shell
# 指定安装的路径,不指定的话,安装过程中可能软件所需要的文件复制到其他不同目录,删除软件很不方便,复制软件也不方便
cd Python-3.7.5/
# 配置安装目录
./configure --prefix=/usr/local/python3 --enable-optimizations --with-ssl
# 编译
make
```



#### 10、安装

```shell
make install
```



#### 11、创建软链接

```shell
ln -s /usr/local/python3/bin/python3 /usr/bin/python3

ln -s /usr/local/python3/bin/pip3 /usr/bin/pip3
```



#### 12、完成

```shell
python3 -V

pip3 -V
```



#### 13、更改yum配置(非必要)

取决于你是否将python3设置为了默认，如果是可以执行下面操作。

因为其要用到python2.x才能执行，否则会导致yum不能正常使用（不管安装 python3的那个版本，都必须要做的）

```shell
vim /usr/bin/yum 
把 #! /usr/bin/python 修改为 #! /usr/bin/python2
vim /usr/libexec/urlgrabber-ext-down 
把 #! /usr/bin/python 修改为 #! /usr/bin/python2
```

