## 安装hexo

安装hexo之前需要安装Nodejs组件，这个在我的另一篇文章：

Hexo是我们博客的框架，我们需要在我们的电脑里创建一个文件夹，可以命名为Blog，Hexo框架与你发布的博客网页以后都会在这个文件中。创建好后进入文件夹中，按住shift键，右键鼠标点击打开powershell窗口；

![image-20220706165706028](https://pic.xinsong.xyz/img/202207061657071.png)

打开后

使用npm命令安装Hexo，命令行窗口输入：

```powershell
npm install -g hexo-cli
```

等待一会儿即可，接着输入命令初始化我们的博客：

```powershell
hexo init blog
```

上面的两个命令都作用于我们刚刚创建的Blog文件夹。

然后我们就可以看到我们的Blog/文件夹目录下会出现一个blog文件夹，接着我们进入blog文件夹；

输入命令：

```powershell
cd blog
```

接下来我们来检测我们网站的雏形，依次输入以下命令；

```powershell
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

进入Github网页创建一个新仓库，点击**New repository**

仓库名为固定格式： **Github用户名.github.io**

例如我的sxfinn.github.io

![image-20220706173413453](https://pic.xinsong.xyz/img/202207061734669.png)

我们使用 ssh 免密部署，这种方式可以避免输密码的繁琐，并且速度也是最快的。

###  创建密钥对

为了方便运行 GitHub Actions 时登录 GitHub 账号，我们使用 SSH 方式登录。就是要把设备的私钥交给 GitHub Actions，公钥交给 GitHub，需要去 Settings 里去配置。

进入git bash

```powershell
ssh-keygen -t rsa -C "Github的邮箱地址"

# 例如 ssh-keygen -t rsa -C "123123123@gmail.com"
```

代码参数含义：

-t 指定密钥类型，默认是 rsa ，可以省略。
-C 设置注释文字，比如邮箱。
-f 指定密钥文件存储文件名。

输入后接着按三个回车

```powershell
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

```powershell
ssh -T git@github.com
```

出现如下信息则说明添加成功。

![image-20220706180343464](https://pic.xinsong.xyz/img/202207061803493.png)

### 配置部署信息

在配置之前我们要解释一个概念，在blog根目录里的`_config.yml`文件称为**站点配置文件**，之后我们对博客配置的修改都是通过此文件来进行的。

如下图：

![image-20220706171616325](https://pic.xinsong.xyz/img/202207061716433.png)

现在我们要做的就是将Hexo与Github关联起来，打开站点的配置文件`_config.yml`，下拉到最后修改为：

```yaml
deploy:
  type: git
  repo: 你的地址
  branch: master
```

**注意**：repo一行中“你的地址”，即为你刚刚创建的**GitHub仓库**的ssh链接。

例如我的：

![image-20220706180816807](https://pic.xinsong.xyz/img/202207061808015.png)

保存站点配置文件。

我们刚刚配置的文件其实就是在我们执行推送到远端这个hexo d这个命令时，让hexo知道该推送到哪里去，很显然我们部署在我们的GitHub仓库里。



安装Git部署插件，输入命令：（仍然是Blog/blog 目录下）

```powershell
npm install hexo-deployer-git --save
```

一定要记得执行此命令否则无法自动部署。



这时我们输入三条命令：

```powershell
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

2. 添加解析记录



**方案一（建议）：如果是想用主域名和www的域名访问站点**

需要添加两个解析记录：

第一个解析记录的记录类型为A，主机记录为@，记录值为ping 你的github用户名.github.io的ip地址，填入为下列 IP 中的至少一个

- 185.199.108.153
- 185.199.109.153
- 185.199.110.153
- 185.199.111.153

第二个解析记录的记录类型为CNAME，主机记录为www，记录值为你的新建的仓库名——你的github用户名.github.io 

注意：这里直接添加两个CNAME一个类型为@另一个为www都指向 你的github用户名.github.io 也可以的（但主机记录为@的域名添加CNAME记录值可能会有与其他服务，例如电子邮件的冲突问题，因此不建议）



**方案二：如果是想使用单独的一个二级域名（包括www域）如blog域名访问站点**

Custom domain填入相应域名再添加一个解析记录，记录类型为CNAME，指向 你的github用户名.github.io 即可。



**方案三：如果是单独的只想使用主域名访问站点**

添加解析记录的记录类型为A，主机记录为@，记录值为ping 你的github用户名.github.io的ip地址，填入为下列 IP 中的一个至少一个

- 185.199.108.153
- 185.199.109.153
- 185.199.110.153
- 185.199.111.153



### Page的自动双重定向

如下图添加了我们填写的Custom domain的解析记录会报错：**DNS valid  for primary**，并且提示我们www域名未正确配置，这是为何？

![image-20220826182121725](https://pic.xinsong.xyz/img/202208261821888.png)

带着些历史原因，通常我们的印象中www域名和主域名是访问同一个站点的，大部分网站都这个机制，包括Github Page。

这段时间搞了几种框架的网站建设了，我发现不是域名指向站点的 IP 就能访问站点。

以typecho搭建的网站为例，即使一个域名指向了我们的 IP ，但如果我们的站点中没有保存这个域名的信息，也就是没有添加此域名作为访问站点的域名，那么这个域名仍然无法访问我们的站点，也就是说访问站点时是有域名认证的~，能解析到我的 IP 和站点还不行，我的站点还必须认证了此域名。

利用GitHub page部署网站也有一个这个问题，那就是page的 IP 就那么4个，是如何能够访问我们的站点而不是其他人的站点呢？实际上GitHub是根据我们提交的CNAME去确定我们的站点的，CNAME的值会去填充Custom domain，而这个Custom domain会和我们的站点形成一种绑定关系，我们只有使用这个Custom domain才能正确访问站点。

既然只能通过这一个Custom domain来访问站点，那么Github Page是怎么实现的www域名和主域名都能访问站点这样的机制呢？

**原因是Github Page提供了双重定向的机制**。

GitHub Page推荐我们通过DNS提供商在配置一个主域名同时配置一个 `www` 子域名，这样主域名和www的域名都可以访问站点

如果您通过 DNS 提供商正确配置一个[顶级域名](https://doc.yonyoucloud.com/doc/wiki/project/github-pages-basics/custom-domains.html)（例如 `example.com` ）和一个匹配的 `www` 子域名（例如 `www.example.com` ），GitHub 服务器会自动地创建双重定向。

正确配置这两个域名后：

- 如果你的 `CNAME` 文件为 `example.com`，那么 `www.example.com` 会定向到 `example.com`。
- 如果你的 `CNAME` 文件为 `www.example.com`，那么 `example.com` 会定向到 `www.example.com`。

虽然只能填写一个Custom domain，**也只能通过此Custom domain访问站点**，但是通过DNS提供商对www域名和主域名DNS的正确配置GitHub Page会为我们自动创建双重定向，可以让 非Custom domain：**www域 和主域**其中一个 重定向到Custom domain，从而能够达到www域名和主域名都能访问站点的效果。而如果使用其他子域名（非www），则只能使用这一个子域名访问站点。自动重定向仅适用于`www`子域和其主域名之间，不适用于任何其他子域。

例如，如果您将`www.example.com`站点配置为自定义域，并且为主域和`www`域设置了 GitHub Pages DNS 记录，`example.com`则将**重定向**到`www.example.com`,如果您将`example.com`站点配置为自定义域，并且为主域和`www`域设置了 GitHub Pages DNS 记录，`www.example.com`则将**重定向**到`example.com`.

正是由于Page支持这样的机制，GitHub Page在我们Custom domain设置为 主域名或者www域名时，**会主动检测**DNS提供方是否将主域名和www域名都正确配置了（即使我们并不想使用重定向机制），像上图这样只添加了我们填写的Custom domain的这一条`xinsong.xyz`的解析记录，会提示我们：**DNS valid for primary**，www.xinsong.xyz就不能正确访问我们的站点，只能通过xinsong.xyz去访问。而如果这里的Custom domain是www.xinsong.xyz，并且也只添加了www的解析记录，那么这里也会提示：**DNS valid for primary**，并且报错：xinsong.xyz配置错误，也只能通过www.xinsong.xyz访问站点。如果不想看到Page报错和提示或者想使用双重定向机制，那么按照上面的**方案一**做就能正确配置不会报错了。

因此提示 DNS valid for primary 是Github Page主动检查是否满足重定向机制的结果，一旦提示我们DNS valid for primary，说明未能创建双重定向，只有使用当前的Custom domain才能访问我们的站点。



参考：[管理 GitHub Pages 站点的自定义域 - GitHub Docs](https://docs.github.com/cn/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)



3. 登录GitHub，进入之前创建的仓库，点击setting，设置Custom domain，输入你的域名。

![image-20220706190711806](https://pic.xinsong.xyz/img/202207061907861.png)

这样就通过我们的个性化域名来访问我们自己的网站了。

但是这样做每次推送到远端时这个**Custom domain**都会被覆盖，需要重新输入，因此还需要做如下操作：

进入Blog/blog/source目录下，创建一个记事本文件，输入GitHub Page页面的Custom domain。这里建议是带www的域名。





![image-20220706191228751](https://pic.xinsong.xyz/img/202207061912780.png)

保存即可，命名为**CNAME**，注意保存类型选择**所有文件**而不是**文本文件**。

这样我们每次推送到远端时就可以保证我们始终都能使用此域名进行访问。

然后再进入blog文件中打卡powershell，依次执行：

```powershell
hexo clean

hexo g

hexo d
```

这时候无论怎么折腾，我们在浏览器中输入我们的域名，就可以访问我们的网站了。



## 最终

GitHub Page推荐我们在配置一个主域名的同时配置一个 `www` 子域名。

如果您通过 DNS 提供商正确配置一个[顶级域名](https://doc.yonyoucloud.com/doc/wiki/project/github-pages-basics/custom-domains.html)（例如 `example.com` ）和一个匹配的 `www` 子域名（例如 `www.example.com` ），GitHub 服务器会自动地创建双重定向。

正确配置这两个域名后：

- 如果你的 `CNAME` 文件为 `example.com`，那么 `www.example.com` 会定向到 `example.com`。
- 如果你的 `CNAME` 文件为 `www.example.com`，那么 `example.com` 会定向到 `www.example.com`。

**警告**：不要在你的 DNS 提供商上为你的自定义主域名创建一个 `CNAME` 记录！这样做可能会导致与其他服务，如电子邮件等，在该域的问题。



所以根据GitHub Page的**官方说明**，总结下：

* 使用子域名时（非www域），建议在DNS提供商上为该子域名创建一个 `CNAME` 记录，那么只能通过此子域去访问站点
* 单独使用主域名时（不配置www域），建议在DNS提供商上为该主域名创建一个 `A` 记录（CNAME可行，但不建议，可能发生电子邮件及其他服务的冲突问题），那么只能通过此主域访问站点
* 使用www/主域名时，建议在DNS提供商上为该主域名创建一个 `A` 记录，在DNS提供商上为该www域名创建一个 `CNAME` 记录即可自动创建双重定向（定向到我们CNAME文件中的地址，无论CNAME文件为 `www.example.com` 还是 `example.com` ），使用www域和主域均可访问站点
* 单独使用www域时，建议在DNS提供商上为该子域名创建一个 `CNAME` 记录，那么只能通过此www域去访问站点

自动重定向仅适用于`www`子域和其主域名之间，不适用于任何其他子域。



你可以使用除了 `www` 以外的一个自定义子域名和一个自定义顶端域名来通过域名重定向（有时候也叫“域名转发”）。但是，请注意，这只能用于用户和组织的 Pages，而不是项目的 Pages。



参考：[在你的 DNS 提供者上配置 A 记录的技巧 - GitHub Pages 指南 - UDN开源文档](https://doc.yonyoucloud.com/doc/wiki/project/github-pages-basics/tip-record.html)
