本文参考：[升级GCC版本到11.1 - 云+社区 - 腾讯云](https://cloud.tencent.com/developer/article/1894845)

## 升级GCC版本到11.1

  GCC11终于发正式版了, 4月底官方终于发了11.1正式版. 对于我们来说, 项目实际使用基于C++17的协程已经有段时间, stackless在没有compiler额外生成代码Buff的加持下, Stack变量的人肉处理, 花括号对代码的隔离, 还是会导致一些额外的工作量, 便利性上不那么完美. 既然GCC11.1已经发了, 我们之前的GCC8.3也在正常工作中, 升级成本比我们想象的低, 那新版到来, 又能解决项目的一些实际痛点, 升级的动力自然就比较足了. 

## 1. 下载GCC11.1源码

  GCC的源码仓库地址为: [https://github.com/gcc-mirror/gcc](https://zhuanlan.zhihu.com/gcc的Git地址) ,在浏览器打开这个网址后，不要急于下载，先选择gcc的版本，如下图所示：

![image-20220518215012621](https://pic.xinsong.xyz/img/202205182150806.png)



如上图所示找到GCC11.1,并点击 "releases/gcc-11.1.0"完成仓库的切换,  然后直接在网页上下载zip包, 自行上传至服务器后解压.

```shell
unzip gcc-releases-gcc-11.1.0.zip
```

也可以通过直接git clone的方式来拉取对应的gcc源码, 进入自己的home目录执行如下命令:

```shell
git clone --branch releases/gcc-11.1.0 https://github.com/gcc-mirror/gcc.git
```

两种方式效果一样, 获取到源代码后, 将当前目录切换到GCC源码根目录, 进入下一步. 源码目录如下图所示:

![image-20220518215022746](https://pic.xinsong.xyz/img/202205182150846.png)

## 2. 安装依赖库

  新的GCC源码内置了依赖库的获取脚本, GCC所依赖的mpfr, gmp, mpc, isl都可以使用内置脚本直接获取, 比老版本简单非常多,  在GCC目录下, 执行:

```javascript
./contrib/download_prerequisites
```

此命名会自动下载GCC编译需要的几个依赖库. 

## 3.配置和编译

  前文也提到了, 我们需要同时保留老版本的GCC, 所以配置项里需要指定安装的目录, 配置命令如下 :

```javascript
./configure --prefix=/usr/local/gcc-11.1.0 --enable-bootstrap --enable-languages=c,c++ --enable-threads=posix --enable-checking=release --enable-multilib --with-system-zlib
```

  我们仅会使用GCC做C与C++的编译, 所以此处语言也仅选择了这两者.    配置完成后, 我们进入编译安装阶段. 此处可以直接打开并行编译执行make命令, 比如笔者机器是2核的, 此处直接将并发数设置为4进行编译, 实测效果不错.

```javascript
make -j4
```

  **记得一定要root权限, 不然可能会因为权限不足安装失败.**

```shell
sudo -s
make install
```

  这时整个gcc的安装过程已经成功执行完成, 按如下方法测试GCC是否正确安装:

```shell
/usr/local/gcc-11.1.0/bin/gcc --version
```

![image-20220518215030007](https://pic.xinsong.xyz/img/202205182150053.png)

得到上图的输出, 则GCC11.1已经成功安装.

## 4. CMake中的使用, ABI兼容问题

  请参考 [升级GCC到8.3](https://zhuanlan.zhihu.com/p/345928428) 中的相关部分.

## 5. LD_LIBRARY_PATH的问题

  如果代码没有全部切换到11.1, 那我们还需要额外的一步操作才能让gcc11.1编译出的程序正常的运行:

```javascript
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/gcc-11.1.0/lib64
```

可以直接将本行追加到~/.bashrc, 避免每次都单独写shell脚本追加该项. 注意更改~/.bashrc后记得重新连接终端, 让修改生效. 如果同时也添加了gcc-8.3.0, 注意gcc-11.1.0的export要在8.3.0之前, 否则还是会报LD相关的问题.

## 6. GDB的问题

  GCC11.1开始, 要求使用支持C++11的编译器进行编译, 可能是由于这个改变, 生成的App可以正常运行, 但不能挂接GDB, 表现是用GDB启动生成的App就直接Crash, 报Segment Fault.

  我们需要升级GDB到较新的版本,  就能解决该问题(具体出错的原因没有细查).  升级方法很简单:

  到GNU官网下载比较新的GDB源码包并解压并编译安装, 笔者使用的shell是:

```javascript
wget http://ftp.gnu.org/gnu/gdb/gdb-10.2.tar.gz
tar -zxf gdb-10.2.tar.gz
cd gdb-10.2
./configure
make -j4
sudo make install
```

  默认安装的路径是/usr/local/bin, 并不会覆盖老的GDB(老的在/usr/bin/下), 所以使用VSCode或者自行运行的时候, 需要正确指定一下GDB的版本, 检查GDB的版本:

## 小结

  至此我们已经完成了GCC11.1的安装和相关环境的适配, 就笔者项目而言, 近期主要会用到的特性如下:

1. C++20的coroutine
2. concept(预计可以比较好的简化反射相关的代码) 
3. modules

