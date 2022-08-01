## 1. 简介

Clash for Windows（CFW）应该是目前Windows和macOS上最好用的基于规则的跨平台代理工具软件，允许用户可视化操作和支持主流SS/V2ray/Trojan多协议。Clash.Net在此基础上对UI进行了美化，也推荐使用。

## 2.转码托管 - 机场订阅和策略组

### 转码托管机场链接

一般情况下，这些订阅转换API转码不会保存你的节点信息。通俗来说，只是把你的字符转换成另外一种计算机语言，并套进对应的代理工具软件配置参数的链接中，有顾虑的可以自己搭建API。

🔗**订阅转换托管API推荐**：

[ACL4SSR 在线订阅转换](https://acl4ssr-sub.github.io/)

[边缘@订阅转换API](https://bianyuan.xyz/)

这些API中一般会有相应分流规则的选项，接下来我们介绍直接采用ACL4SSR等大佬维护的分流规则，他们的规则策略其实已经能满足绝大部分用户的使用场景。

我们就以转换为最常见的Clash为例，Clash相比其他的客户端有一个特殊之处也需要特别说明：**新版内核出现后，新旧订阅不能互相兼容**。

**进入API，有两种模式**：

1. 基础模式；
2. 进阶模式；

基础模式直接生成的话，就是使用**默认规则并启用clash新参数的转换**。对于最新版的原版clash可以直接使用。

进阶模式中，是否启用clash新参数的开关在右下角的“更多选项”里，并且支持选择分流规则和策略组。

由于基础模式的局限性，我们使用进阶模式转换订阅链接。

👉 **转换托管步骤**

1. 输入的订阅/节点链接，选择生成类型，这里我们选 **`Clash`**（也有叫Clash新参数的），界面中的**远程配置**即clash的代理规则配置文件，然后按照下图操作。

![image-20220801195425903](https://pic.xinsong.xyz/img/202208011954102.png)

2. 选择客户端，选择Clash。
3. 选择后端地址，留空或者默认即可。
4. 选择远程配置，这里我选择ACL4SSR Online默认版分组比较全(与Github同步)。
5. 勾选更多选项。
   * 如需打游戏或者使用微软应用，则需勾选下面的“启用UDP”；
   * 如果机场中有chacha20协议的SSR节点（目前仍不支持，会导致无法导入订阅），就需要勾选那个“过滤非法节点”；
   * **Clash New Field**是否勾选视客户端版本而定，参照下方；

> **Clash New Field**选项是只有转换为Clash链接才生效，勾选表示使用新版的写法。新版clash不能兼容旧订阅，旧版clash不能兼容使用新参数的订阅。

**以下为Clash新参数支持情况，请根据版本确定是否勾选Clash New Field**：

[clash内核](https://github.com/Dreamacro/clash)在`2020/08/16`release的`1.1.0`版本开始支持SSR。

[Windows版clash](https://github.com/Fndroid/clash_for_windows_pkg)在`2020/08/21`release的`0.11.5`版本中引入新内核，也就是从这个版本开始，就必须clash新参数。这个版本之前的Windows版clash使用新参数会无法导入订阅。

[Mac版clash](https://github.com/yichengchen/clashX)（clashx）在`2020/09/06`release的`1.30.2`版本开始使用新内核，和Windows版一样，从这个版本开始必须使用新参数，之前的版本必须不使用新参数且不支持SSR。

[路由器版](https://github.com/vernesong/OpenClash)的openclash使用的TUN内核（这玩意只有一个预览版）在`2020/07/25`的`v0.39.5-beta`版开始添加新参数支持，引入新内核。

[Android版clash](https://github.com/Kr328/ClashForAndroid)在`2020/09/03`release的`2.1.5`版开始支持新参数。可以从谷歌play商店直接安装（搜不到就是安卓版本不够）。

根据我自己的需求，这里我勾选了`Emoji`,`Clash New Field`,`UDP`.

3. 点生成订阅链接，获得转码后的链接地址，下面是按照图内的地址转码后得到的结果：

```
https://pub-api-1.bianyuan.xyz/sub?target=clash&url=https%3A%2F%2Fcate.com&insert=false&config=https%3A%2F%2Fraw.githubusercontent.com%2FACL4SSR%2FACL4SSR%2Fmaster%2FClash%2Fconfig%2FACL4SSR_Online.ini&emoji=true&list=false&tfo=false&scv=false&fdn=false&sort=false&udp=true&new_name=true
```

**注意**：

1. 如果订阅支持tfo就把这项后面的false改成true，默认为false。
2. 有些机场订阅可能要**开启跳过证书检验**才能正常使用，如果无法使用，请尝试修改参数`scv=false`为`scv=true`。

最终得到的链接导入客户端即可。