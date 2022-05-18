

### 购买对象存储(cos)资源包

---

购买链接：https://curl.qcloud.com/CcQyuzkZ

![image-20220518124256171](https://pic.xinsong.xyz/img/202205181242247.png)

### 创建存储桶列表

> 这里是因为我用做来做个人图床，所以考虑到自己的一个用量，10G完全满足我了。其中流量中的外网下行流量包(图片访问一次就会产生费用)，和CDN回源流量包(CDN节点向 COS 获取数据所产生的CDN回源流量)，其实也要购买的，但是我是小站，用不了那么多，直接选择按量计费，不选择购买这两个的资源包，余额准备几块钱就好了。

* 来到腾讯云[对象存储](https://cloud.tencent.com/product/cos?from=10680)控制台，创建存储

![image-20220518122221501](https://pic.xinsong.xyz/img/202205181222687.png)

![image-20220518122519936](https://pic.xinsong.xyz/img/202205181225083.png)

* 高级配置可以不去管它

![image-20220518122547786](https://pic.xinsong.xyz/img/202205181225879.png)

- 访问权限选择**公有读私有写**，否则图片无法读取，其他的根据自己往下填写就可以。 地域建议离你所在的位置越近越好。 
-  腾讯云头像–>[访问管理](https://cloud.tencent.com/product/cam?from=10680)–> [API密钥管理](https://cloud.tencent.com/product/ssm?from=10680)，创建密钥，就会生成 **APPID、SecretId和SecretKey**

![image-20220518122910316](https://pic.xinsong.xyz/img/202205181229503.png)

* 打开 PicGO

* 填写好前面三个信息后，**存储空间名和区域**的信息在刚刚创建对象存储控制台那里，这里注意的是，**COS版本选择 v5** 

![image-20220518123144471](https://pic.xinsong.xyz/img/202205181231570.png)

![image-20220518123240046](https://pic.xinsong.xyz/img/202205181232224.png)

到此，就可以直接使用Picgo上传图片了，这时图片的链接是 `https://sxnico-imgaes-1310265079.cos.ap-chengdu.myqcloud.com`（对于我而言，取决于你的存储桶名称）



对于访问量少的站点来说完全足够了，不过这还不够好，最近注意到腾讯云COS+CDN的搭配既能不占用服务器的宽带，又能快速上传下载文件，而且以我目前的使用量来说基本上不花钱，这里分享下我的折腾过程。

COS也支持公网直接访问，但是流量费太贵，直接访问不划算，况且腾讯云还每个月会赠送10G的免费CDN流量，完全可以利用起来。

### 准备

首先你得有个备案了的域名，之前腾讯云是可以支持默认CDN加速域名的，不过目前已经无法使用了，因此我们只能使用自定义域名了。

腾讯云：[对象存储 开启默认 CDN 加速域名-控制台指南-文档中心-腾讯云-腾讯云](https://cloud.tencent.com/document/product/436/36636)

> 自2022年5月9日起，对象存储（Cloud Object Storage，COS）服务将不再支持新增默认 CDN 加速域名。您已开启、或曾经开启的默认 CDN 加速域名不会受到影响，可以继续使用，但建议您使用自定义 CDN 加速域名代替默认 CDN 加速域名。关于自定义 CDN 加速域名的操作指引，请参见 [开启自定义 CDN 加速域名](https://cloud.tencent.com/document/product/436/36637) 文档。



### 接入域名

步骤如下：

![img](https://pic.xinsong.xyz/img/202205181253520.png)

### 开通 CDN 服务

配置 CDN 前，您需要 [开通 CDN 服务](https://cloud.tencent.com/document/product/228/3149)。如果您已开通 CDN 服务，请继续后续操作步骤.

### 操作步骤

进入 CDN 控制台，在左侧导航栏中找到**域名管理**，单击**添加域名**。

### 域名配置

1. 选择加速区域

2. 填写加速域名

   如果您接入的域名为以下情况，则需要进行域名归属权验证，验证步骤请参考下方

   - 首次接入该域名。
   - 该域名已被其他用户接入。
   - 接入域名为泛域名。

3. 选择加速类型

4. 其他选填项(后续可在域名管理中更改)

#### 域名配置



* 我只有一个备案了的域名，因此我选择使用此域名加前缀子域名作为我的加速域名

![image-20220518132526621](https://pic.xinsong.xyz/img/202205181325701.png)

由于我已经做过了CDN接入，因此这里不在显示需要验证，通常我们还需要DNS解析验证，步骤如下：

### DNS 解析验证操作步骤

1. 单击**验证方法**，获取 DNS 验证所需要的解析记录信息，在验证完成前保持页面打开。
   ![img](https://pic.xinsong.xyz/img/202205181310206.png)

2. 如果您的域名解析商为腾讯云，进入 [域名服务控制台](https://console.cloud.tencent.com/cns)，找到该域名并单击解析，添加一条记录类型为 TXT 的 DNS 记录，主机记录填写为 `_cdnauth`。
   ![img](https://pic.xinsong.xyz/img/202205181310612.png)
   如果您的域名解析商为阿里云，同样找到该域名并单击解析，添加一条记录类型为 TXT 的 DNS 记录，主机记录填写为 `_cdnauth`。
   ![img](https://pic.xinsong.xyz/img/202205181310848.png)

   

   如果您的域名解析商为阿里云，进入域名服务控制台，找到该域名并单击解析，添加一条记录类型为 TXT 的 DNS 记录，主机记录填写为 `_cdnauth`。

   ![image-20220518131653355](https://pic.xinsong.xyz/img/202205181316481.png)

3. 等待 TXT 解析生效，单击**验证**按钮进行验证。
   ![img](https://pic.xinsong.xyz/img/202205181310058.png)



> 注意：
>
> 为接入域名验证归属权添加 TXT 记录，无论接入的是三级域名（如：`a.test.com`）还是多级域名（`a.b.test.com`），均是在主域名（`test.com`）下进行的，主域名前的主机记录值填写为：`_cdnauth`。



#### 源站配置

1. 选择源站类型
2. 选择回源协议
3. 输入源站地址
4. 配置回源 HOST

我们使用的是腾讯云的COS源直接选择就好，回源协议就选择https，开启私有桶访问权限。

![image-20220518130104117](https://pic.xinsong.xyz/img/202205181301195.png)

#### 服务配置

由于我并没有大文件资源，因此不需要开启分片回源（这里根据个人需求选择）

![image-20220518130412724](https://pic.xinsong.xyz/img/202205181304824.png)

#### 用量封顶配置

根据个人需求选择

![image-20220518130538604](https://pic.xinsong.xyz/img/202205181305695.png)

全部配置结束后点击“确认提交”

#### 接入完成

![img](https://pic.xinsong.xyz/img/202205181306960.png)



## 解析CDN域名

1. 返回域名管理，在您域名成功解析前，CNAME 处会有提示 icon。复制此处的 CNAME 值。

![image-20220518132818476](https://pic.xinsong.xyz/img/202205181328530.png)



2. 进入您的域名解析商域名控制台，我用的是阿里云。

3. 单击要解析的域名，进入解析记录页。

   ![image-20220518133059663](https://pic.xinsong.xyz/img/202205181330723.png)

4. 进入解析记录页后，单击**添加记录**按钮，开始设置解析记录。

![image-20220518133245705](https://pic.xinsong.xyz/img/202205181332820.png)

5. 将记录类型选择为 CNAME。主机记录即域名前缀，可任意填写（如：pic）。记录值填写为步骤1中复制的CNAME值。解析线路，TTL 默认即可。



### 配置CDN域名

### 验证 CNAME 是否生效

不同的 DNS 服务商CNAME 生效的时间略有不同，一般在半个小时之内生效。您可以通过 nslookup 或 dig 的方式来查询 CNAME 是否生效，若应答的CNAME记录是我们配置的CNAME，则说明配置成功，此时您已成功开启加速服务。

![image-20220518133618125](https://pic.xinsong.xyz/img/202205181336192.png)



## 配置CND域名

CDN—>域名管理—>要配置的域名

![image-20220518134043491](https://pic.xinsong.xyz/img/202205181340627.png)



![image-20220518134222118](https://pic.xinsong.xyz/img/202205181342291.png)

根据个人需求配置即可，

### 申请ssl证书

https配置指南：https://cloud.tencent.com/document/product/228/41687

 申请免费证书：https://cloud.tencent.com/document/product/400/6814

通常一个ssl证书只能绑定一个域名，即使是子域名也是需要的。因此还需要申请一个证书并且绑定我们的CDN域名。

### 添加证书

申请结束后，去https设置里添加证书

![image-20220518140646331](https://pic.xinsong.xyz/img/202205181406413.png)

配置https

![image-20220518140519520](https://pic.xinsong.xyz/img/202205181405704.png)







