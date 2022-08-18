## 简介

**Clash for Windows**（CFW）应该是目前Windows上最好用的基于规则的跨平台代理工具软件，允许用户可视化操作和支持主流SS/V2ray/Trojan协议，Clash.Net在此基础上对UI进行了美化，比较推荐使用。



## 转码托管 - 机场订阅和策略组

网络上有许多转码托管API，只需要用到一个你的机场订阅链接，就能自动生成转码后的链接地址，通过嵌套在线规则策略组的链接，从而能够应用各种分流规则和策略组，最终API会返回一个链接地址，我们导入此链接地址至客户端，就能按照指定嵌套的规则分流了。



### 转码托管机场链接

一般情况下，这些订阅转换API转码不会保存你的节点信息。通俗来说，只是把你的字符转换成另外一种计算机语言，并套进对应的代理工具软件配置参数的链接中，有顾虑的可以自己搭建API。



🔗**订阅转换托管API推荐**：

[ACL4SSR 在线订阅转换](https://acl4ssr-sub.github.io/)（用的人数很多，后端比较稳定）

[边缘@订阅转换API](https://bianyuan.xyz/)（比较稳定）

[在线订阅转换工具](https://sub.v1.mk/)（魔改版后端，全网功能最全！站点界面很特殊）

这些API中一般会有相应分流规则的选项，接下来我们介绍直接采用ACL4SSR等大佬维护的分流规则，他们的规则策略其实已经能满足绝大部分用户的使用场景。

我们就以转换为最常见的**Clash**为例，**Clash**相比其他的客户端有一个特殊之处也需要特别说明：**新版内核出现后，新旧订阅不能互相兼容**。



**进入API，有两种模式**：

1. 基础模式；

​		**使用场景**例如：当机场提供的 Surge 配置足以满足需求，但额外需要使用 Clash 订阅时，此时可以使用基础方式进行转换。

​		基础模式直接生成的话，就是使用**默认规则并启用clash新参数的转换**，对于最新版的原版clash可以直接使用，旧版clash无法使用。

2. 进阶模式；

​		**使用场景**：即对 `调用地址` 甚至程序目录下的 `配置文件` 进行个性化的编辑以满足不同的需求。

​		进阶模式中，是否启用clash新参数的开关在右下角的“更多选项”里，并且支持选择分流规则和策略组。

​		

由于基础模式的局限性，我们使用**进阶模式**转换订阅链接。



👉 **转换托管步骤**

1. 输入的订阅/节点链接（有多个需要分行写）。

![image-20220801195425903](https://pic.xinsong.xyz/img/202208011954102.png)

2. 选择客户端，这里选择Clash。
3. 选择后端地址，留空或者默认即可。
4. 选择远程配置，**远程配置**即clash的代理规则配置文件，这里我选择ACL4SSR Online默认版分组比较全(与Github同步)。
5. 根据需求勾选更多选项。
   * 如需打游戏或者使用Microsoft Store应用，则需勾选下面的“启用UDP”；
   * 如果机场中有chacha20协议的SSR节点（目前仍不支持，会导致无法导入订阅），就需要勾选那个“过滤非法节点”；
   * **Clash New Field**是否勾选视客户端版本而定，参照下方：

**以下为Clash新参数支持情况，请根据版本确定是否勾选Clash New Field**：

[clash内核](https://github.com/Dreamacro/clash)在`2020/08/16`release的`1.1.0`版本开始支持SSR。

[Windows版clash](https://github.com/Fndroid/clash_for_windows_pkg)在`2020/08/21`release的`0.11.5`版本中引入新内核，也就是从这个版本开始，就必须clash新参数。这个版本之前的Windows版clash使用新参数会无法导入订阅。

[Mac版clash](https://github.com/yichengchen/clashX)（clashx）在`2020/09/06`release的`1.30.2`版本开始使用新内核，和Windows版一样，从这个版本开始必须使用新参数，之前的版本必须不使用新参数且不支持SSR。

[路由器版](https://github.com/vernesong/OpenClash)的openclash使用的TUN内核（这玩意只有一个预览版）在`2020/07/25`的`v0.39.5-beta`版开始添加新参数支持，引入新内核。

[Android版clash](https://github.com/Kr328/ClashForAndroid)在`2020/09/03`release的`2.1.5`版开始支持新参数。可以从谷歌play商店直接安装（搜不到就是安卓版本不够）。

> 注：**Clash New Field**选项是只有转换为Clash链接才生效，勾选表示使用新版的写法。新版clash不能兼容旧订阅，旧版clash不能兼容使用新参数的订阅。



根据我自己的需求，这里我勾选了`Emoji`,`Clash New Field`,`UDP`.

6. 点生成订阅链接，获得转码后的链接地址，下面是按照图内的地址转码后得到的结果：

```
https://pub-api-1.bianyuan.xyz/sub?target=clash&url=https%3A%2F%2Fcate.com&insert=false&config=https%3A%2F%2Fraw.githubusercontent.com%2FACL4SSR%2FACL4SSR%2Fmaster%2FClash%2Fconfig%2FACL4SSR_Online.ini&emoji=true&list=false&tfo=false&scv=false&fdn=false&sort=false&udp=true&new_name=true
```

### 调用参数

可用参数远不止这些，介绍一下以上出现的可能必须需要修改的参数：

1. scv：跳过证书检验；

**注意**：

1. 如果订阅支持tfo就把这项后面的false改成true，默认为false。
2. 有些机场订阅可能要**开启跳过证书检验**才能正常使用，如果无法使用，请尝试修改参数`scv=false`为`scv=true`。**原理**是因为部分机场用的中转提供的域名是不能通过tls认证的，需要关闭对证书验证才可以使用。



每个API生成的链接**显式调用的参数**都有差异，如果参数不写，参数的**默认配置**（true/false）与选择的后端有关，如不满足自身使用场景，请自行修改参数。

参考**官方文档**：[subconverter/README-cn.md at master · tindy2013/subconverter](https://github.com/tindy2013/subconverter/blob/master/README-cn.md)



#### 调用参数说明

| 调用参数      | 必要性 | 示例                         | 解释                                                         |
| ------------- | ------ | ---------------------------- | ------------------------------------------------------------ |
| target        | 必要   | surge&ver=4                  | 指想要生成的配置类型，详见上方 [支持类型](https://github.com/tindy2013/subconverter/blob/master/README-cn.md#支持类型) 中的参数 |
| url           | 可选   | https%3A%2F%2Fwww.xxx.com    | 指机场所提供的订阅链接或代理节点的分享链接，需要经过 [URLEncode](https://www.urlencoder.org/) 处理，**可选的前提是在 `default_url` 中进行指定**。也可以使用 data URI。可使用 `tag:xxx,https%3A%2F%2Fwww.xxx.com` 指定该订阅的所有节点归属于`xxx`分组，用于配置文件中的`!!GROUP=XXX` 匹配 |
| group         | 可选   | MySS                         | 用于设置该订阅的组名，多用于 SSD/SSR                         |
| upload_path   | 可选   | MySS.yaml                    | 用于将生成的订阅文件上传至 `Gist` 后的名称，需要经过 [URLEncode](https://www.urlencoder.org/) 处理 |
| include       | 可选   | 详见下文中 `include_remarks` | 指仅保留匹配到的节点，支持正则匹配，需要经过 [URLEncode](https://www.urlencoder.org/) 处理，会覆盖配置文件里的设置 |
| exclude       | 可选   | 详见下文中 `exclude_remarks` | 指排除匹配到的节点，支持正则匹配，需要经过 [URLEncode](https://www.urlencoder.org/) 处理，会覆盖配置文件里的设置 |
| config        | 可选   | https%3A%2F%2Fwww.xxx.com    | 指 外部配置 的地址 (包含分组和规则部分)，需要经过 [URLEncode](https://www.urlencoder.org/) 处理，详见 [外部配置](https://github.com/tindy2013/subconverter/blob/master/README-cn.md#外部配置) ，当此参数不存在时使用 主程序目录中的配置文件 |
| dev_id        | 可选   | 92DSAFA                      | 用于设置 QuantumultX 的远程设备 ID, 以在某些版本上开启远程脚本 |
| filename      | 可选   | MySS                         | 指定所生成订阅的文件名，可以在 Clash For Windows 等支持文件名的软件中显示出来 |
| interval      | 可选   | 43200                        | 用于设置托管配置更新间隔，确定配置将更新多长时间，单位为秒   |
| rename        | 可选   | 详见下文中 `rename`          | 用于自定义重命名，需要经过 [URLEncode](https://www.urlencoder.org/) 处理，会覆盖配置文件里的设置 |
| filter_script | 可选   | 详见下文中 `filter_script`   | 用于自定义筛选节点的js代码，需要经过 [URLEncode](https://www.urlencoder.org/) 处理，会覆盖配置文件里的设置。出于安全考虑，链接需包含正确的 `token` 参数，才会应用该设置 |
| strict        | 可选   | true / false                 | 如果设置为 true，则 Surge 将在上述间隔后要求强制更新         |
| upload        | 可选   | true / false                 | 用于将生成的订阅文件上传至 `Gist`，需要填写`gistconf.ini`，默认为 false (即不上传) ,详见 [自动上传](https://github.com/tindy2013/subconverter/blob/master/README-cn.md#自动上传) |
| emoji         | 可选   | true / false                 | 用于设置节点名称是否包含 Emoji，默认为 true                  |
| add_emoji     | 可选   | true / false                 | 用于在节点名称前加入 Emoji，默认为 true                      |
| remove_emoji  | 可选   | true / false                 | 用于设置是否删除节点名称中原有的 Emoji，默认为 true          |
| append_type   | 可选   | true / false                 | 用于在节点名称前插入节点类型，如 `[SS]`,`[SSR]`等            |
| tfo           | 可选   | true / false                 | 用于开启该订阅链接的 TCP Fast Open，默认为 false             |
| udp           | 可选   | true / false                 | 用于开启该订阅链接的 UDP，默认为 false                       |
| list          | 可选   | true / false                 | 用于输出 Surge Node List 或者 Clash Proxy Provider 或者 Quantumult (X) 的节点订阅 或者 解码后的 SIP002 |
| sort          | 可选   | true / false                 | 用于对输出的节点或策略组按节点名进行再次排序，默认为 false   |
| sort_script   | 可选   | 详见下文 `sort_script`       | 用于自定义排序的js代码，需要经过 [URLEncode](https://www.urlencoder.org/) 处理，会覆盖配置文件里的设置。出于安全考虑，链接需包含正确的 `token` 参数，才会应用该设置 |
| script        | 可选   | true / false                 | 用于生成Clash Script，默认为 false                           |
| insert        | 可选   | true / false                 | 用于设置是否将配置文件中的 `insert_url` 插入，默认为 true    |
| scv           | 可选   | true / false                 | 用于关闭 TLS 节点的证书检查，默认为 false                    |
| fdn           | 可选   | true / false                 | 用于过滤目标类型不支持的节点，默认为 true                    |
| expand        | 可选   | true / false                 | 用于在 API 端处理或转换 Surge, QuantumultX, Clash 的规则列表，即是否将规则全文置入订阅中，默认为 true，设置为 false 则不会将规则全文写进订阅 |
| append_info   | 可选   | true / false                 | 用于输出包含流量或到期信息的节点, 默认为 true，设置为 false 则取消输出 |
| prepend       | 可选   | true / false                 | 用于设置插入 `insert_url` 时是否插入到所有节点前面，默认为 true |
| classic       | 可选   | true / false                 | 用于设置是否生成 Clash classical rule-provider               |
| tls13         | 可选   | true / false                 | 用于设置是否为节点增加tls1.3开启参数                         |
| new_name      | 可选   | true / false                 | 如果设置为 true，则将启用 Clash 的新组名称 (proxies, proxy-groups, rules) |

根据我自身的使用情况，我选择的参数配置为true的有：**emoij，udp，expand，scv，clash新字段名**。（许多后端默认开启expand：展开规则全文到订阅，因此可以不写expand参数）。



最终得到的链接导入客户端即可。



## 其他客户端订阅链接的转换

只有Clash由于内核更新导致的新旧版无法兼容而有了`Clash New Field`选项，因此其他客户端的链接转换无需考虑是否勾选`Clash New Field`。

其他订阅链接的转换都是按照如上操作只需更换目标客户端即可。
