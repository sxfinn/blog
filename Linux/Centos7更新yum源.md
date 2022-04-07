#### 查看当前的 yum 源

```shell
yum repolist
```

#### 对CentOS-Base.repo进行备份

```shell
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

#### 使用阿里云源替换本地源

```shell
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```

#### 清除yum缓存并更新

```shell
yum clean all
yum makecache
```

#### 安装epel源

```shell
yum list | grep epel-release 
yum install -y epel-release
```

#### 使用阿里开源镜像提供的epel源

```shell
wget -O /etc/yum.repos.d/epel-7.repo http://mirrors.aliyun.com/repo/epel-7.repo
```

#### 再次清除系统yum缓存，并重新生成新的yum缓存

```shell
yum clean all
yum makecache
```

#### 升级所有包

```shell
yum -y update
```

#### 查看新 yum 源

```shell
yum repolist enabled
yum repolist all
```



#### tips

如果在yum makecache的过程中一直报：
Another app is currently holding the yum lock; waiting for it to exit...
不必慌张，输入以下命令即可

```shell
rm -f /var/run/yum.pid
```