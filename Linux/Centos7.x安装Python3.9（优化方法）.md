### Centos7.6安装python3.9

---

#### 1.安装相应的编译工具

建议在root下操作，会方便很多，一定要安装，否则编译安装会报错。

```shell
yum -y groupinstall "Development tools"
yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel
yum install -y libffi-devel
yum install zlib* -y
```

#### 2.下载安装包

```shell
wget https://www.python.org/ftp/python/3.9.12/Python-3.9.12.tar.xz
```

#### 3.解压

```shell
tar -xvJf  Python-3.9.12.tar.xz
```

#### 4.创建编译安装目录

```shell
mkdir /usr/local/python3.9
```

#### 5.安装

```shell
cd Python-3.9.12
./configure --prefix=/usr/local/python3.9 --enable-optimizations --with-ssl 
#第一个指定安装的路径,不指定的话,安装过程中可能软件所需要的文件复制到其他不同目录,删除软件很不方便,复制软件也不方便.
#第二个可以提高python10%-20%代码运行速度.
#第三个是为了安装pip需要用到ssl,后面报错会有提到.
make && make install
```

enable-optimizations 这个参数在服务器使用的是低版本的gcc时会报错，可以先升级gcc。

不过这好像是个概率事件，偶尔会成功。

参考链接：https://stackoverflow.com/questions/41405728/what-does-enable-optimizations-do-while-compiling-python

#### 6.创建软链接

相当于windows环境变量，如下写不会默认还是Python2.7，不需要修改yum配置。

```shell
ln -sf /usr/local/python3/bin/python3.9 /usr/local/bin/python3
# 软链接至/bin/python3方便写脚本
ln -s /usr/local/python3/bin/python3.9 /bin/python3
ln -sf /usr/local/python3/bin/pip3 /usr/local/bin/pip3
```

如果建立时提示如下报错信息：

```shell
ln: failed to create symbolic link '/usr/bin/python3': File exists
```

**解决方法：**

ln -sf 即参数多加个f即可

```shell
ln -sf /usr/local/python3/bin/python3.9 /bin/python3
```

#### 7.验证是否成功

```shell
python3 -V
pip3 -V
```

#### 8.报错处理

**错误1.**

```shell
zipimport.ZipImportError: can't decompress data; zlib not available Makefile:1099: recipe for target 'install' failed make: *** [install] Error 1
```

需要安装依赖

```shell
yum -y install zlib1g-dev
```

**错误2.**

```shell
ModuleNotFoundError: No module named '_ctypes'
```

需要安装依赖

```shell
yum -y install libffi-devel 
```

这两个错误需要的依赖已经添加到一开始的依赖安装上去了。

[参考文章](https://blog.csdn.net/elija940818/article/details/79238813)

#### 9.安装pipenv

在centos中使用python3.7或以上版本,进行pip install 命令容易报错

```shell
pip is configured with locations that require TLS/SSL, however the ssl module in Python is not available.
Could not fetch URL https:*******: There was a problem confirming the ssl certificate: 
Can't connect to HTTPS URL because the SSL module is not available. - skipping
```

 **在./configure过程中，如果没有加上–with-ssl参数时，默认安装的软件涉及到ssl的功能不可用，刚好pip3过程需要ssl模块，而由于没有指定，所以该功能不可用。解决办法是重新对python3.6进行编译安装，用一下过程来实现编译安装:**

```shell
cd Python-3.7.2
./configure --with-ssl
make && make install
```

即可正常使用pip安装.
这个也在安装python的时候指定了.

#### 10.修改pip安装源

修改系统pip安装源
在家目录下新建.pip文件夹,进入文件夹新建文件pip.conf之后写入相应镜像网站地址

```shell
cd ~
mkdir .pip
cd .pip
vim pip.conf

#进入后添加以下内容,保存退出.
[global]
index-url = https://mirrors.aliyun.com/pypi/simple
```

修改pipenv安装源
在自己的虚拟环境中找到Pipfile文件,将其中的url = "https://pypi.org/simple"修改为你需要的国内镜像,如https://mirrors.aliyun.com/pypi/simple/

```shell
[root@localhost myproject]# vim Pipfile 


[[source]]
name = "pypi"
url = "https://pypi.org/simple" # 改为url = "https://mirrors.aliyun.com/pypi/simple/"
verify_ssl = true

[dev-packages] #这里是开发环境专属包,使用pipenv install --dev package来安装专属开发环境的包

[packages] # 全部环境的通用包,安装在这里.

[requires]
python_version = "3.7"
```

