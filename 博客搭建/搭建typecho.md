## 搭建Typecho个人博客教程

![image-20220810151100623](https://pic.xinsong.xyz/img/202208101511778.png)

**Typecho**是一个基于[PHP](https://zh.m.wikipedia.org/wiki/PHP)的开源部落格程序。它使用多种数据库（[MySQL](https://zh.m.wikipedia.org/wiki/MySQL)、[PostgreSQL](https://zh.m.wikipedia.org/wiki/PostgreSQL)、[SQLite](https://zh.m.wikipedia.org/wiki/SQLite)、[MariaDB](https://zh.m.wikipedia.org/wiki/MariaDB)）储存数据，在[GPLv2](https://zh.m.wikipedia.org/wiki/GNU通用公共许可协议)许可证下发行。

### 特性

---

**扩展**

Typecho的程序设计逻辑与[WordPress](https://zh.m.wikipedia.org/wiki/WordPress)相似，它通过插件与模板机制对程序进行扩展。它们可以在不更改部落格内容和Typecho核心部分时，修改部落格的界面和功能。同时Typecho使用独特的模块化架构，这使得扩展十分便利。

**Markdown**

Typecho使用的是[Markdown](https://zh.m.wikipedia.org/wiki/Markdown)语法，通过HyperDown（页面存档备份，(存于互联网档案馆）解析器进行解析。Markdown是一种轻量级标记语言，它允许人们使用易读易写的纯文本格式编写文档，这也是当下大多数部落格程序所采用的编辑器语法。

**简洁**

Typecho的程序本体不到500KB，而它的数据表在不包括扩展生成的数据表时仅7张。整站只需几个接口，通过静态继承快速传递参数，插件越多，功能只会越强大，对速度影响却微乎其微。

**自适应**

Typecho的默认模板和后台，全部采用了[响应式设计](https://zh.m.wikipedia.org/wiki/响应式网页设计)。所以Typecho的大多数自制模板，都采用了自适应设计。



### 准备

---

- [x] **云服务器一台**
- [x] **域名一个**（如果是国内服务器需要备案）

### 本教程使用的环境

---

* **CentOS 7.9.2009 x86_64(Py3.7.9)**
* **宝塔面板腾讯云专享版 7.9.3（预装）**



**进入面板，安装LNMP，即Nginx、MySQL和PHP，这些环境是运行Typecho程序必不可少的。推荐使用PHP7以上版本，其他保持默认即可。**

看大家网站需要什么环境进行选择。如果是生产环境推荐大家使用编译安装，如果只是测试环境选择极速安装。两者的区别是编译安装慢但稳定，极速安装虽然慢但是没编译安装稳定。耐心等待，可以在左上角查看进度。

![image-20220811142442722](https://pic.xinsong.xyz/img/202208111424059.png)

* **nginx-1.20**
* **pureftpd-1.0.49**
* **mysql-5.7**
* **php-8.0**
* **phpmyadmin-5.1**



### 开始搭建

---

* **添加站点**

![image-20220811143510394](https://pic.xinsong.xyz/img/202208111435483.png)

* **填写域名，多个域名分行填写，没有域名可以用ip地址**
* **FTP可不选，个人认为面板上传文件就很方便**
* **创建数据库**

![image-20220811143749802](https://pic.xinsong.xyz/img/202208111437863.png)

* **保存好密码以及用户名，后面会用到**

![image-20220811143843136](https://pic.xinsong.xyz/img/202208111438172.png)



### 上传Typecho文件

---

* **进入Typecho官网下载正式版**  [下载 - Typecho Official Site](https://typecho.org/download)

![image-20220811162552557](https://pic.xinsong.xyz/img/202208111625649.png)

* **进入网站根目录**

![image-20220811143956112](https://pic.xinsong.xyz/img/202208111439164.png)

* **全选文件删除**（user.ini为放跨站配置可以不删)

![image-20220811144958097](https://pic.xinsong.xyz/img/202208111449216.png)

* **上传typecho.zip文件**

![image-20220811145207231](https://pic.xinsong.xyz/img/202208111452381.png)

* **解压typecho.zip**

![image-20220811145341434](https://pic.xinsong.xyz/img/202208111453517.png)

* **删除压缩包**

![image-20220811145441036](https://pic.xinsong.xyz/img/202208111454154.png)



### 解析域名

---

* **进入域名控制台，添加解析记录**
* **将你的域名解析到服务器的ip地址**

添加两条解析记录如下：

![image-20220811140250896](https://pic.xinsong.xyz/img/202208111402013.png)



### 安装Typecho

---

* **浏览器地址栏输入域名xinsong.xyz**

![image-20220811145526630](https://pic.xinsong.xyz/img/202208111455717.png)

* **仅填写红框，其他保持默认即可**（使用刚刚保存的数据库名以及密码）

![image-20220811145845315](https://pic.xinsong.xyz/img/202208111458429.png)

* **设置用户名和登录密码以及邮箱**（用于登录站点）

![image-20220811145958034](https://pic.xinsong.xyz/img/202208111459116.png)

* **安装成功**

![image-20220811165344511](https://pic.xinsong.xyz/img/202208111653669.png)

* **访问站点**



![image-20220811150059818](https://pic.xinsong.xyz/img/202208111500977.png)

### 设置伪静态和地址重写

---

这一步尤其重要，正确设置伪静态和固定链接可以保证网站被正常访问，顺序一定不要搞错了，先在宝塔设置伪静态规则，再设置Typecho固定链接，否则会开启固定链接会报错，未报错也可能导致除首页之外的任何页面都访问不了。

若未设置伪静态直接启用地址重写会报错，坚持开启则会导致无法访问网站文章。

![image-20220811163544452](https://pic.xinsong.xyz/img/202208111635500.png)

#### 设置伪静态规则

* **进入宝塔面板进行站点设置**

![image-20220811152704519](https://pic.xinsong.xyz/img/202208111527694.png)

* **在宝塔/www/wwwroot/你的域名下 用typecho伪静态**
* **在宝塔/www/wwwroot/你的域名下/又一个文件夹下才是typecho程序 用typecho2**

![image-20220811152759067](https://pic.xinsong.xyz/img/202208111527150.png)

#### 设置固定链接

登录网站后台：设置→永久链接

* **启用地址重写功能**（必须）
* **保存设置**

![image-20220811152946173](https://pic.xinsong.xyz/img/202208111529275.png)

## 主题 & 插件

Typecho 博客本身不带主题/插件商店，因此主题和插件需要自己到论坛、网上去找，下载后上传到网站目录的相应文件夹中，再到网站后台启用即可。

- **插件位置：**网站目录/usr/plugins
- **主题位置：**网站目录/usr/themes
- **附件位置：**网站目录/usr/uploads



### 安装过程可能会遇到的问题

---

1. **开启放跨站攻击无法访问站点**（宝塔默认开启）

​		解决方法一：[宝塔默认启用"防止跨站"攻击后，网站打不开，善用open_basedir参数](http://sebcxy.com/article/95)

​		解决方法二：关闭放跨站攻击（不推荐）

2. **地址重写功能开启失败**

​		解决方法：开启伪静态



参考：

[30分钟搭建 Typecho 个人博客教程 - 知乎](https://zhuanlan.zhihu.com/p/34211709)

[宝塔面板安装Typecho开发版本 – 一个轻量的博客程序 - 大鸟博客](https://www.daniao.org/11348.html)

[【博客搭建】Typecho个人博客搭建，快速安装，超小白（很简单的） - 掘金](https://juejin.cn/post/6847902219690328071#heading-3)

[宝塔typecho的伪静态是选择typecho还是typecho2-Web技术-全球主机交流论坛 - Powered by Discuz!](https://91ai.net/thread-844024-3-1.html)

[宝塔默认启用"防止跨站"攻击后，网站打不开，善用open_basedir参数](http://sebcxy.com/article/95)







[宝塔面板安装WordPress（超详细） - 知乎](https://zhuanlan.zhihu.com/p/145116769)

[使用宝塔面板搭建WordPress网站](http://cloud.yundashi168.com/archives/1039#i-8)

[宝塔面板搭建WordPress网站完整教程 - 简书](https://www.jianshu.com/p/293c94adc11d)

