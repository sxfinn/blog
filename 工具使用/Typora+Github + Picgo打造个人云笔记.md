# 2022-04-14-

### 摘要
> Typora配置
>
> Github 创建仓库和获取Token
>
>  Picgo 设置

### 总结
> 

目录
---
[TOC]

------

### 创建仓库

---

1. **如图创建一个新的仓库**

![image-20220414205538376](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204142055655.png)



2. **指定仓库名**

![image-20220414205709426](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204142057604.png)



3. 下载picgo

链接如下：

[Releases · Molunerfinn/PicGo](https://github.com/Molunerfinn/PicGo/releases)

下载后傻瓜式安装即可。

4. 进入GitHub图床设置

![image-20220414210016476](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204142100587.png)

* 仓库名格式如上：用户名 + 仓库名
* 分支名根据你自己选择来指定



### Token的生成

---

进入GitHub网站的设置页面

1. 进入个人**github**账户setting.

![image-20220414210616697](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204142106894.png)

2. 点击Developer settings.

![image-20220414210646861](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204142106020.png)

3. 选择Personal access **tokens**.

![image-20220414210726524](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204142107635.png)

4. 点击Generate new **token**.

![image-20220414210822169](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204142108277.png)

5. 为你创建的**token**添加描述

描述一下，例如 `Test`。

![image-20220414211055684](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204142110829.png)

6. 选择**token**有效期时间。 

   可以选择永不过期

   

7. 为**token**赋予权限。



8. 点击生成。

一定保存好，这个Token只能查看一次。

![image-20220414211813274](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204142118405.png)



**进入PicGo设置，打开时间戳重命名**

![image-20220414211900262](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204142119369.png)

时间戳可以防止命名冲突。

### 免费CDN：jsDelivr+Github

---

CDN的全称是Content Delivery Network，即内容分发网络。CDN是构建在网络之上的内容分发网络，依靠部署在各地的边缘服务器，通过中心平台的负载均衡、内容分发、调度等功能模块，使用户就近获取所需内容，降低网络拥塞，提高用户访问响应速度和命中率。CDN的关键技术主要有内容存储和分发技术。——百度百科

放在Github的资源在国内加载速度比较慢，因此需要使用CDN加速来优化网站打开速度，jsDelivr + Github便是免费且好用的CDN，非常适合博客网站使用。



自定义域名格式可以是你的 用户名/仓库名 前加上`https://cdn.jsdelivr.net/gh`，这样可以加速我们对图片的访问，默认不更改的话图片会是到 raw.githubusercontent 前缀的链接，而这个域名在国内是无法访问的。



### Typora设置

---

- 点击左边图床设计，选择GitHub图床，具体配置如下
- 设定仓库名，填写：**GitHub名/库名**
- 分支，**默认填master**
- 设定Token，**刚才保存的token令牌**
- 指定存储路径，**默认填img/**
- 点击确定和设为默认图床



* **进入typora偏好设置**

![image-20220414212633035](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204142126180.png)



* **进入图像**

勾选对本地和网络位置上的照片应用上规则。



上传服务选择Picgo

路径是你的Picgo的安装目录，通常在 Program File目录中。



以上步骤完成后，点击 **验证图片上传选项**



![image-20220414213132552](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204142131607.png)

如上图就成功上传了



* 进入GitHub查看图片

![image-20220414213304180](https://cdn.jsdelivr.net/gh/sxfinn/Pic/img/202204142133258.png)



**大功告成。**


