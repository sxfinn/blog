### 先说一个快捷键

Window端通过按下CTRL + SHIFT + ALT + D/Q来查看当前分辨率。

### 有时候，您只需要耐心

首次开始流式传输某些内容时，流式传输可能需要一段时间才能达到其最佳质量设置。为了减少加载时间，较高质量的流将在后台开始缓冲，而较低质量的流将立即播放。

有时，您只需要等待Netflix赶上来即可。您可以随时尝试暂停您的内容并等待几秒钟。即使您在配置文件首选项中将质量设置为“高”，Netflix仍默认在流开始时或连接不良时将其质量降低。

若等待一段时间仍未达到4K，那么通过以下的方法一定可以找到原因。

### 显示器及显卡要求

一、拥有至少支持UHD 4K,60Hz的显示器或电视机,什么牌子都行,且显示器上至少有HDMI2.0及以上接口或者DP1.3及以上接口(尤其注意DP1.2及以下接口不支持HDCP2.2,所以不能看奈飞4K；其次分辨率必须要到3840*2160才算达标,3840*1660这种假4K是不行的) 。

二、支援HDMI2.0/a/b或支援DP1.3/1.4的传输线(2021年推荐直接买HDMI2.1或者DP2.0的传输线) 。

三、独立显卡方面: NVIDIA：需要至少GeForce GTX 1050或更高版本显卡+3GB或更高的显存+387.96或更新的驱动程序 AMD：由于AMD对DRM防盗版机制的跟进非常缓慢，目前仅旗下Polaris系列显卡（例如RX470/480/570/580/590），或者Navi系列显卡（例如RX5500/5600/5700）支持观看奈飞4K，Vega系列所有显卡因为不支持Microsoft Playready3.0 DRM，所以全部不能看奈飞4K；对于支持的显卡，需要Adrenalin 2019 Edition 19.8.1或更新的驱动程序。



### 常见浏览器播放Netflix视频时最高支持的分辨率

![image-20220808201550371](https://pic.xinsong.xyz/img/202208082015411.png)



(**补充：表格较老，最新的safari已经支持4KNetflix了**）



### HEVC视频扩展

要观看Netflix 4K视频一定需要下载此扩展才可以，通常微软商店里下载使用是收费的，但是来自厂商的 HEVC 视频扩展在大部分电脑上是免费的。

**免费版地址 任意浏览器输入下面地址**：

ms-windows-store://pdp/?ProductId=9n4wgh0z6vhq

**或者点击下面的链接**：

[来自设备制造商的 HEVC 视频扩展 - Microsoft Store 应用程序](https://apps.microsoft.com/store/detail/%E6%9D%A5%E8%87%AA%E8%AE%BE%E5%A4%87%E5%88%B6%E9%80%A0%E5%95%86%E7%9A%84-hevc-%E8%A7%86%E9%A2%91%E6%89%A9%E5%B1%95/9N4WGH0Z6VHQ?hl=zh-cn&gl=CN)

**收费版地址**： https://www.microsoft.com/zh-cn/p/hevc-video-extensions/9nmzlz57r3t7?activetab=pivot:overviewtab



**然后就是网速要跟得上，4K最少30mb的速度，可以去fast.com测试**。



### 总结

Netflix 可在 Windows 电脑或平板电脑上支持超高清格式。 如需流媒体播放超高清内容，您需要：

- 已安装最新 Windows 更新的 Windows 10 或 Windows 11。

- 必须安装 HEVC 视频扩展。

  > 注意：许多 Windows 10 和 Windows 11 设备可以[免费安装 HEVC 视频扩展](https://www.microsoft.com/p/hevc-video-extensions-from-device-manufacturer/9n4wgh0z6vhq)，但是部分电脑需要[从 Microsoft 处购买](https://www.microsoft.com/p/hevc-video-extensions/9nmzlz57r3t7)才可以安装。 如果您不确定或需要安装视频扩展方面的帮助，请联系设备制造商。

- Microsoft Edge 浏览器或者适用于 Windows 的 Netflix App。

- 支持 60Hz 4K 的显示器（如果是外接显示器，需要 HDCP 2.2 连接）。

  > 注意：1. 连接至您的电脑的每一台显示器都必须满足这些要求才能以超高清格式进行流媒体播放。
  >
  > ​			2. HDCP 2.2 需要显卡接口、显示器接口、传输线都满足 HDMI2.0或者DP1.4以上。

- **如果使用集成 GPU：**Intel 第 7 代酷睿 CPU 或更新版本的 CPU，或 AMD Ryzen CPU。

- **如果使用独立 GPU：**符合[这些要求](https://nvidia.custhelp.com/app/answers/detail/a_id/4583/~/4k-uhd-netflix-content-on-nvidia-gpus)的 Nvidia Geforce GPU，或 AMD Radeon RX 400 系列或更新版本的 GPU。

- 支持流媒体播放超高清内容的 [Netflix 套餐](http://www.netflix.com/ChangePlan)。

- 15 Mbps 或更快的稳定互联网连接速度。

- [流媒体播放画质](https://help.netflix.com/zh-CN/node/87)设置为**“自动”**或**“高”**。

以上所说的要求有任何一个不满足，在视频介绍中都不会出现4K标签，也就代表着无法播放4K分辨率播放。



### 使用软件检查

终极大法：如果还是不能看并且找不到原因，下载测试软件 https://cn.cyberlink.com/prog/bd-support/diagnosis.do

![image-20220808202303212](https://pic.xinsong.xyz/img/202208082023274.png)

安装后如上图操作可得到下图结果：

![image-20220808202326560](https://pic.xinsong.xyz/img/202208082023642.png)

对于框框中的三项即可，这三项都满足，是一定可以观看Netflix的4K视频的，如果都满足依然无法播放，可能是网络的原因。



**HEVC-10bit部分没有通过则说明**：

- 微软的HEVC插件没有购买安装；
- 显卡太旧，不支持10位HEVC解码；

**HDCP2.2部分没有通过则说明**：

- 显示器不是HDMI2.0或者DP1.4以上接口；
- 没有使用HDMI2.0及以上或者DP1.4及以上的传输线；
- 显卡不是HDMI2.0或者DP1.4以上接口；