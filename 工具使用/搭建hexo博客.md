## 安装hexo

安装hexo之前需要安装Nodejs组件，这个在我的另一篇文章：

Hexo是我们博客的框架，我们需要在我们的电脑里创建一个文件夹，可以命名为Blog，Hexo框架与你发布的博客网页以后都会在这个文件中。创建好后进入文件夹中，按住shift键，右键鼠标点击打开powershell窗口；

![image-20220706165706028](https://pic.xinsong.xyz/img/202207061657071.png)

打开后

使用npm命令安装Hexo，命令行窗口输入：

```
npm install -g hexo-cli
```

等待一会儿即可，接着输入命令初始化我们的博客：

```
hexo init blog
```

上面的两个命令都作用于我们刚刚创建的Blog文件夹。

然后我们就可以看到我们的Blog/文件夹目录下会出现一个blog文件夹，接着我们进入blog文件夹；

输入命令：

```
cd blog
```

接下来我们来检测我们网站的雏形，依次输入以下命令；

```
hexo new test_my_site

hexo g

hexo s
```

刚刚的三个命令依次是新建一篇博客文章、生成网页、在本地预览的操作。

在浏览器地址窗口输入：localhost:4000

即可访问到我们本地的博客内容。

![image-20220706171006095](https://pic.xinsong.xyz/img/202207061710498.png)

未经修改过的博客页面应该是上面这个样子的，也可以看到我们刚刚创建的test_my_site这篇文章。

这样就说明我们本地的博客页面是正常的。

**现在来介绍常用的Hexo 命令**

npm install hexo -g #安装Hexo
npm update hexo -g #升级
hexo init #初始化博客

命令简写
hexo n "我的博客" == hexo new "我的博客" #新建文章
hexo g == hexo generate #生成
hexo s == hexo server #启动服务预览
hexo d == hexo deploy #部署

hexo server #Hexo会监视文件变动并自动更新，无须重启服务器
hexo server -s #静态模式
hexo server -p 5000 #更改端口
hexo server -i 192.168.1.1 #自定义 IP
hexo clean #清除缓存，若是网页正常情况下可以忽略这条命令



## 推送网站

刚刚我们看到的只是我们本地的预览，别人并无法访问，接下来要做的就是推送网站，也就是发布网站，让我们的网站可以被更多的人访问。

### 创建Github仓库

进入Github网页创建一个新仓库，点击New repository

仓库名为固定格式： Github用户名.github.io

例如我的sxfinn.github.io

![image-20220706173413453](https://pic.xinsong.xyz/img/202207061734669.png)

我们使用 ssh 免密部署，这种方式可以避免输密码的繁琐，并且速度也是最快的。

###  创建密钥对

为了方便运行 GitHub Actions 时登录 GitHub 账号，我们使用 SSH 方式登录。就是要把设备的私钥交给 GitHub Actions，公钥交给 GitHub，需要去 Settings 里去配置。

进入git bash

```
ssh-keygen -t rsa -C "Github的邮箱地址"

# 例如 ssh-keygen -t rsa -C "123123123@gmail.com"
```

代码参数含义：

-t 指定密钥类型，默认是 rsa ，可以省略。
-C 设置注释文字，比如邮箱。
-f 指定密钥文件存储文件名。



输入后接着按三个回车

```
[root@localhost ~]# ssh-keygen -t rsa       <== 建立密钥对，-t代表类型，有RSA和DSA两种
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):   <==密钥文件默认存放位置，按Enter即可
Created directory '/root/.ssh'.
Enter passphrase (empty for no passphrase):     <== 输入密钥锁码，或直接按 Enter 留空
Enter same passphrase again:     <== 再输入一遍密钥锁码
Your identification has been saved in /root/.ssh/id_rsa.    <== 生成的私钥
Your public key has been saved in /root/.ssh/id_rsa.pub.    <== 生成的公钥
The key fingerprint is:
SHA256:K1qy928tkk1FUuzQtlZK+poeS67vIgPvHw9lQ+KNuZ4 root@localhost.localdomain
The key's randomart image is:
+---[RSA 2048]----+
|           +.    |
|          o * .  |
|        . .O +   |
|       . *. *    |
|        S =+     |
|    .    =...    |
|    .oo =+o+     |
|     ==o+B*o.    |
|    oo.=EXO.     |
+----[SHA256]-----+

```

密钥对生成后默认的位置是用户文件。以 windows 为例：密钥对文件在 `C:\Users\用户名`里，其中 Users 可能因为系统原因显示的是`用户`。这个文件夹里会有一个`.ssh` 的目录，这个里面就是我们的密钥对。

其中 `id_rsa` 是私钥，`id_rsa.pub` 是公钥。

- 配置公钥，应该已经配好，不然如何上到的项目资源，配置路径：github 网站–>Settings–>SSH and GPG keys

  新增一个公钥点击 Add SSH key，然后把 `id_rsa.pub` 这个文件用文本文档打开，将内容复制进去。
  
  

使用下面的命令测试是否成功：

```
ssh -T git@github.com
```

出现如下信息则说明添加成功。

![image-20220706180343464](https://pic.xinsong.xyz/img/202207061803493.png)

### 配置部署信息

在配置之前我们要解释一个概念，在blog根目录里的_config.yml文件称为**站点配置文件**，之后我们对博客配置的修改都是通过此文件来进行的。

如下图：

![image-20220706171616325](https://pic.xinsong.xyz/img/202207061716433.png)

现在我们要做的就是将Hexo与Github关联起来，打开站点的配置文件_config.yml，下拉到最后修改为：

```
deploy:
  type: git
  repo: 你的地址
  branch: master
```

**注意**：repo一行中“你的地址”，即为你刚刚创建的GitHub仓库的ssh链接。

例如我的：

![image-20220706180816807](https://pic.xinsong.xyz/img/202207061808015.png)

保存站点配置文件。

我们刚刚配置的文件其实就是在我们执行推送到远端这个hexo d这个命令时，让hexo知道该推送到哪里去，很显然我们部署在我们的GitHub仓库里。



安装Git部署插件，输入命令：（仍然是Blog/blog 目录下）

```
npm install hexo-deployer-git --save
```

一定要记得执行此命令否则无法自动部署。



这时我们输入三条命令：

```
hexo clean

hexo g

hexo d
```

其实第三条的 hexo d 就是部署网站命令，d是deploy的缩写。完成后，打开浏览器，在地址栏输入你的放置个人网站的仓库路径，即你刚刚创建的仓库名。

你就会发现你的博客已经上线了，可以在网络上被访问了，这与你在本地预览的博客内容是一样的。

![image-20220706181004727](https://pic.xinsong.xyz/img/202207061810219.png)



## 绑定域名

虽然在网络上已经可以访问到我们的网站了，但我们大多数人都还是想使用自己的个性域名来访问网站的，这就需要绑定我们的域名。接下来我将演示阿里云域名绑定。其他厂商区别也都不大，可以作为参照。

1. 登录到阿里云，进入域名控制台点击解析；

![image-20220706181840964](https://pic.xinsong.xyz/img/202207061818021.png)

2. 添加2个解析记录

第一个解析记录的记录类型为A，记录值为ping你的github用户名.github.io的ip地址，填入进去



第二个解析记录的记录类型为CNAME，记录值为你的新建的仓库名——你的github用户名.github.io 



3. 登录GitHub，进入之前创建的仓库，点击setting，设置Custom domain，输入你的域名。

![image-20220706190711806](https://pic.xinsong.xyz/img/202207061907861.png)

这样就通过我们的个性化域名来访问我们自己的网站了。

但是这样做每次推送到远端时这个Custom domain都会被覆盖，需要重新输入，因此还需要做如下操作：



进入Blog/blog/source目录下，创建一个记事本文件，输入你的域名。建议是带www的域名。

![image-20220706191228751](https://pic.xinsong.xyz/img/202207061912780.png)

保存即可，命名为**CNAME**，注意保存类型选择**所有文件**而不是**文本文件**。

这样我们每次推送到远端时就可以保证我们始终都能使用此域名进行访问。



然后再进入blog文件中打卡powershell，依次执行：

```
hexo clean

hexo g

hexo d
```

这时候我们在浏览器中输入我们的域名，就可以访问我们的网站了。
