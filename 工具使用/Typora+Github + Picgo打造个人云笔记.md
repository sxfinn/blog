# 2022-04-14-

### 摘要
> Typora配置
>
> Github 创建仓库和获取Token
>
> jsDelivr
>
> Picgo 设置

### 总结
> 

目录
---
[TOC]

------

### 创建仓库

---

1. **如图创建一个新的仓库**

![image-20220518215708783](https://pic.xinsong.xyz/img/202205182157031.png)



2. **指定仓库名**

![image-20220630083148716](https://pic.xinsong.xyz/img/202206300831903.png)



3. 下载picgo

链接如下：

[Releases · Molunerfinn/PicGo](https://github.com/Molunerfinn/PicGo/releases)

下载后一直下一步傻瓜式安装即可。

4. 进入GitHub图床设置

![image-20220518215729404](https://pic.xinsong.xyz/img/202205182157501.png)

* 仓库名格式如上：`用户名` + `仓库名`
* 分支名根据你的远程分支自行指定

### Token的生成

---

进入GitHub网站的设置页面

1. 进入个人**github**账户setting.

![image-20220518215736888](https://pic.xinsong.xyz/img/202205182157129.png)

2. 点击Developer settings.

![image-20220518215745836](https://pic.xinsong.xyz/img/202205182157000.png)

3. 选择Personal access **tokens**.

![image-20220518215753525](https://pic.xinsong.xyz/img/202205182157651.png)

4. 点击Generate new **token**.

![image-20220518215800979](https://pic.xinsong.xyz/img/202205182158123.png)

5. 为你创建的**token**添加描述

描述一下，例如 `Test`。

![image-20220518215811289](https://pic.xinsong.xyz/img/202205182158447.png)

6. 选择**token**有效期时间。 

   可以选择永不过期

   

7. 为**token**赋予权限。



8. 点击生成。

### Picgo配置

---

- 点击左边图床设置，**选择GitHub图床**，具体配置如下
- 设定仓库名，填写：**GitHub名/库名**
- 分支，**默认填master**
- 设定Token，**刚才保存的token令牌**
- 指定存储路径，**默认填img/**
- 点击确定和设为**默认图床**

一定保存好，这个Token只能查看一次。

![image-20220518215923781](https://pic.xinsong.xyz/img/202205182159930.png)



**进入PicGo设置，打开时间戳重命名**

![image-20220518215930367](https://pic.xinsong.xyz/img/202205182159482.png)

时间戳可以防止命名冲突。

### 免费CDN：jsDelivr+Github

---

如果默认不适用自定义域名，**github**里边这个地方是比较蛋疼的一点，上传的图片啥的，他会给你上传到另一个**文件服务器**里边，这个地址国内不使用vpn是无法访问的，而我们**本地typora的图片链接**和**GitHub远端存储**的链接是一样的，通常如下：`https://raw.githubusercontent.com/sxfinn/Pic/master/img/202204120923663.png`。

但是这个链接我们在国内是无法访问的，因此需求是给自己个人博客搭建图床的同学就会出现图片无法访问的问题。



**解决方法：**

**CDN**的全称是**Content Delivery Network**，**即内容分发网络**。CDN是构建在网络之上的内容分发网络，依靠部署在各地的边缘服务器，通过中心平台的**负载均衡**、**内容分发**、**调度等功能模块**，使用户**就近获取所需内容**，降低网络拥塞，**提高用户访问响应速度和命中率**。CDN的关键技术主要有**内容存储和分发技术**。——百度百科

**放在Github的资源在国内加载速度比较慢**，因此需要使用CDN加速来优化网站打开速度，**jsDelivr + Github**便是免费且好用的CDN，非常适合博客网站使用。

自定义域名格式可以是你的 **用户名/仓库名** 前加上`https://cdn.jsdelivr.net/gh`，这样可以加速我们对图片的访问，默认不更改的话图片会是使用如下`raw.githubusercontent` **前缀**的链接。

使用CND加速后，Typora文档的图片的链接会变成我们设置的自定义域名 ：`https://cdn.jsdelivr.net/gh/用户名/仓库名/2022xxxxxx.png`，而如果我们将文档上传到GitHub服务器上,链接则通常如下：

`https://camo.githubusercontent.com/e27aa2f17225d131bfa13f46412a4cee2ec4796a29f62fca34cebc2421bc2efe/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f737866696e6e2f5069632f696d672f3230323230343132303935393338322e706e67`

例如：我写下这篇文章时的同一张图片

* GitHub上远端的链接

![image-20220518215941821](https://pic.xinsong.xyz/img/202205182159021.png)



* 本地typora的图片链接

![image-20220518220004544](https://pic.xinsong.xyz/img/202205182200661.png)

**GitHub**给出如下解释：为托管您的图像，**GitHub** 使用 开源项目 **Camo**。**Camo** 为每幅图像生成开头为 [https://camo.githubuserconten...](https://link.segmentfault.com/?enc=FVfTQdJcnfN8CKb5g%2BCXBQ%3D%3D.6G%2F0HTMeWyqzLrQ35QGorFMAu8rUW8wz%2BI2As6eE5zgPtIZzA4oV9OfANMP%2F4b0u) 的**匿名 URL 代理**，将会对其他用户隐藏您的浏览器详细信息和相关信息。

参考链接：[Github image without camo - Stack Overflow](https://stackoverflow.com/questions/57857193/github-image-without-camo)



由于**raw.githubusercontent.com** 国内域名无法访问，

如果我们想要在个人博客中插入图片这两种前缀的链接都可以使用：

1. camo.githubusercontent.com ...
2. cdn.jsdelivr.net/gh ...



### Typora设置

---

* **进入typora偏好设置**

![image-20220518220012516](https://pic.xinsong.xyz/img/202205182200695.png)



* **进入图像**

勾选对本地和网络位置上的照片应用上规则。



上传服务选择Picgo

路径是你的Picgo的安装目录，通常在 **Program File**目录中。



以上步骤完成后，点击 **验证图片上传选项**



![image-20220518220019485](https://pic.xinsong.xyz/img/202205182200550.png)

如上图就成功上传了



* 进入GitHub查看图片

![image-20220518220025767](https://pic.xinsong.xyz/img/202205182200843.png)



**大功告成。**

