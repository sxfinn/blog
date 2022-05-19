### 购买对象存储(cos)资源包

---

购买链接：https://curl.qcloud.com/CcQyuzkZ

![image-20220518124256171](https://pic.xinsong.xyz/img/202205182112358.png)

### 创建存储桶列表

> 这里是因为我用做来做个人图床，所以考虑到自己的一个用量，10G完全满足我了。其中流量中的外网下行流量包(图片访问一次就会产生费用)，和CDN回源流量包(CDN节点向 COS 获取数据所产生的CDN回源流量)，其实也要购买的，但是我是小站，用不了那么多，直接选择按量计费，不选择购买这两个的资源包，余额准备几块钱就好了。

* 来到腾讯云[对象存储](https://cloud.tencent.com/product/cos?from=10680)控制台，创建存储

![image-20220518122221501](https://pic.xinsong.xyz/img/202205182112644.png)

![image-20220518122519936](https://pic.xinsong.xyz/img/202205182112631.png)

* 高级配置可以不去管它

![image-20220518122547786](https://pic.xinsong.xyz/img/202205182112564.png)

- 访问权限选择**公有读私有写**，否则图片无法读取，其他的根据自己往下填写就可以。 地域建议离你所在的位置越近越好。 
- 腾讯云头像–>[访问管理](https://cloud.tencent.com/product/cam?from=10680)–> [API密钥管理](https://cloud.tencent.com/product/ssm?from=10680)，创建密钥，就会生成 **APPID、SecretId和SecretKey**

![image-20220518122910316](https://pic.xinsong.xyz/img/202205182112956.png)

* 打开 PicGO；
* 填写好前面三个信息后，**存储空间名和区域**的信息在刚刚创建对象存储控制台那里，这里注意的是，**COS版本选择 v5** ；
* 指定存储路径选择img/，不需要填写自定义域名；

![image-20220518123144471](https://pic.xinsong.xyz/img/202205182112463.png)

![image-20220518123240046](https://pic.xinsong.xyz/img/202205182112589.png)

到此，就可以直接使用Picgo上传图片了，这时图片的链接是 `https://sxnico-imgaes-1310265079.cos.ap-chengdu.myqcloud.com`（对于我而言，取决于你的存储桶名称）

对于访问量少的站点来说完全足够了，不过这还不够好，最近注意到腾讯云COS+CDN的搭配既能不占用服务器的宽带，又能快速上传下载文件，而且以我目前的使用量来说基本上不花钱，这里分享下我的折腾过程。

COS也支持公网直接访问，但是流量费太贵，直接访问不划算，况且腾讯云还每个月会赠送10G的免费CDN流量，完全可以利用起来。

