## 准备

首先你得有个备案了的域名，之前腾讯云是可以支持默认CDN加速域名的，不过目前已经无法使用了，因此我们只能使用自定义域名了。

腾讯云：[对象存储 开启默认 CDN 加速域名-控制台指南-文档中心-腾讯云-腾讯云](https://cloud.tencent.com/document/product/436/36636)

> 自2022年5月9日起，对象存储（Cloud Object Storage，COS）服务将不再支持新增默认 CDN 加速域名。您已开启、或曾经开启的默认 CDN 加速域名不会受到影响，可以继续使用，但建议您使用自定义 CDN 加速域名代替默认 CDN 加速域名。关于自定义 CDN 加速域名的操作指引，请参见 [开启自定义 CDN 加速域名](https://cloud.tencent.com/document/product/436/36637) 文档。

## 接入域名

步骤如下：

![img](https://pic.xinsong.xyz/img/202205182113776.png)

### 开通 CDN 服务

配置 CDN 前，您需要 [开通 CDN 服务](https://cloud.tencent.com/document/product/228/3149)。如果您已开通 CDN 服务，请继续后续操作步骤.

### 操作步骤

进入 CDN 控制台，在左侧导航栏中找到**域名管理**，单击**添加域名**。

### 域名配置

1. 选择加速区域

2. 填写加速域名

   如果您接入的域名为以下情况，则需要进行域名归属权验证，验证步骤请参考下方：

   - 首次接入该域名。
   - 该域名已被其他用户接入。
   - 接入域名为泛域名。

3. 选择加速类型

4. 其他选填项(后续可在域名管理中更改)

#### 域名配置

* 我只有一个备案了的域名，因此我选择使用此域名加前缀子域名作为我的加速域名

![image-20220518132526621](https://pic.xinsong.xyz/img/202205182113318.png)

由于我已经做过了CDN接入，因此这里不在显示需要验证，通常我们还需要DNS解析验证，步骤如下：

#### DNS 解析验证操作步骤

1. 单击**验证方法**，获取 DNS 验证所需要的解析记录信息，在验证完成前保持页面打开。
   ![img](https://pic.xinsong.xyz/img/202205182113209.png)

2. 如果您的域名解析商为腾讯云，进入 [域名服务控制台](https://console.cloud.tencent.com/cns)，找到该域名并单击解析，添加一条记录类型为 TXT 的 DNS 记录，主机记录填写为 `_cdnauth`。
   ![img](https://pic.xinsong.xyz/img/202205182113726.png)
   如果您的域名解析商为阿里云，同样找到该域名并单击解析，添加一条记录类型为 TXT 的 DNS 记录，主机记录填写为 `_cdnauth`。
   ![img](https://pic.xinsong.xyz/img/202205182113699.png)

   

   如果您的域名解析商为阿里云，进入域名服务控制台，找到该域名并单击解析，添加一条记录类型为 TXT 的 DNS 记录，主机记录填写为 `_cdnauth`。

   ![image-20220518131653355](https://pic.xinsong.xyz/img/202205182113880.png)

3. 等待 TXT 解析生效，单击**验证**按钮进行验证。
   ![img](https://pic.xinsong.xyz/img/202205182113574.png)



> 注意：
>
> 为接入域名验证归属权添加 TXT 记录，无论接入的是三级域名（如：`a.test.com`）还是多级域名（`a.b.test.com`），均是在主域名（`test.com`）下进行的，主域名前的主机记录值填写为：`_cdnauth`。



#### 源站配置

1. 选择源站类型
2. 选择回源协议
3. 输入源站地址
4. 配置回源 HOST

我们使用的是腾讯云的COS源直接选择就好，回源协议就选择https，开启私有桶访问权限。

由于是COS源不需要我们去输入地址，腾讯云中可以直接选择。

回源HOST会在源站地址选择后自动填入，不需要手动输入。

![image-20220518130104117](https://pic.xinsong.xyz/img/202205182113526.png)

#### 服务配置

由于我并没有大文件资源，因此不需要开启分片回源（这里根据个人需求选择）

![image-20220518130412724](https://pic.xinsong.xyz/img/202205182113390.png)

#### 用量封顶配置

根据个人需求选择，这里我是默认配置；

![image-20220518130538604](https://pic.xinsong.xyz/img/202205182113700.png)

全部配置结束后点击“确认提交”

#### 接入完成

完成添加域名操作后，请耐心等待域名配置下发至全网节点，下发时间约5 - 10分钟。

![img](https://pic.xinsong.xyz/img/202205182113071.png)



## 解析CDN域名

1. 返回域名管理，在您域名成功解析前，CNAME 处会有提示 icon。复制此处的 CNAME 值。

![image-20220518132818476](https://pic.xinsong.xyz/img/202205182113415.png)



2. 进入您的域名解析商域名控制台，我这里用的是阿里云。

3. 单击要解析的域名，进入解析记录页。

   ![image-20220518133059663](https://pic.xinsong.xyz/img/202205182113427.png)

4. 进入解析记录页后，单击**添加记录**按钮，开始设置解析记录。

   （这里我添加pic.xinsong.xyz域名进行演示）

![image-20220518133245705](https://pic.xinsong.xyz/img/202205182113478.png)

5. 将记录类型选择为 CNAME。主机记录即域名前缀，可任意填写（如：pic）。记录值填写为步骤1中复制的CNAME值。解析线路，TTL 默认即可。



**注意**：这里我的图片配得有点问题，应该是使用哪个自定义域名就添加相应的CNAME的解析记录，我有两个自定义CDN域名，由于imge.xinsong.xyz已经在使用了，所以这里我使用了pic.xinsong.xyz来演示。

### 验证 CNAME 是否生效

不同的 DNS 服务商CNAME 生效的时间略有不同，一般在半个小时之内生效。您可以通过 nslookup 或 dig 的方式来查询 CNAME 是否生效，若应答的CNAME记录是我们配置的CNAME，则说明配置成功，此时您已成功开启加速服务。

这里我使用dig的方法验证。

打开CMD ，命令行输入dig + 你的自定义域名；

![image-20220518133618125](https://pic.xinsong.xyz/img/202205182113963.png)



## 配置CND域名

进入CDN控制台—>域名管理—>要配置的域名

![image-20220518134043491](https://pic.xinsong.xyz/img/202205182113083.png)

访问控制可以进行防盗链配置以及ip黑白名单等，这里我仅仅设置了ip限频

![image-20220518134222118](https://pic.xinsong.xyz/img/202205182113342.png)

根据个人需求配置即可。

### 申请ssl证书

---

通常一个ssl证书只能绑定一个域名，即使是子域名也是需要的。因此还需要申请一个证书并且绑定我们的CDN域名。

腾讯云提供了免费的证书申请，期限为一年过期了重新申请就行。

 申请免费证书：https://cloud.tencent.com/document/product/400/6814

流程：

![img](https://pic.xinsong.xyz/img/202205182113158.png)

比较简单，具体参照官方文档。

### 添加证书

---

申请结束后，进入“https配置”中添加证书；

腾讯云中申请的证书由腾讯云托管，我们选择已托管证书中添加证书。

https配置指南：https://cloud.tencent.com/document/product/228/41687

![image-20220518140646331](https://pic.xinsong.xyz/img/202205182113235.png)

**配置https，建议如下图更改**

1. 打开HTTPS 2.0；
2. 开启HTTPS强制跳转；

![image-20220518140519520](https://pic.xinsong.xyz/img/202205182113441.png)

## Picgo配置

到这里就可以使用我们的CDN域名了。

进入Picgo腾讯云COS设置：

这里只用修改两个东西

存储路径：随便一个名字

设定自定义域名：将您**接入的域名**填入“设定自定义域名”一栏

![image-20220519110831849](https://pic.xinsong.xyz/img/202205191108961.png)

验证图片上传选项

![image-20220519105752449](https://pic.xinsong.xyz/img/202205191110682.png)

到此就结束了，我们可以使用腾讯云提供的CDN服务了。



**最新更详细的内容请参考**：

[对象存储 开启自定义 CDN 加速域名-控制台指南-文档中心-腾讯云](https://cloud.tencent.com/document/product/436/36637)

[内容分发网络 CDN 接入域名-快速入门-文档中心-腾讯云](https://cloud.tencent.com/document/product/228/41215)

[内容分发网络 CDN 配置 CNAME-快速入门-文档中心-腾讯云](https://cloud.tencent.com/document/product/228/3121)

[内容分发网络 CDN 配置指南-文档中心-腾讯云](https://cloud.tencent.com/document/product/228/37851)

[内容分发网络 CDN 域名配置-配置指南-文档中心-腾讯云](https://cloud.tencent.com/document/product/228/6283)
