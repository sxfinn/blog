## 主题安装

 稳定版【建议】

在你的 Hexo 根目录里

```powershell
git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
```

测试版

> 测试版可能存在 bug，追求稳定的请安装稳定版

如果想要安装比较新的 dev 分支，可以

```powershell
git clone -b dev https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
```

> 升级方法：在主题目录下，运行 `git pull`

### 应用主题

修改 Hexo 根目录下的 `_config.yml`，把主题改为`butterfly`

```yaml
theme: butterfly
```

### 安装插件

如果你没有 pug 以及 stylus 的渲染器，请下载安装：

```powershell
npm install hexo-renderer-pug hexo-renderer-stylus --save
```

### 升级建议

为了减少升级主题后带来的不便，请使用以下方法（建议，可以不做）。

在 hexo 的根目录创建一个文件 `_config.butterfly.yml`，并把主题目录的 `_config.yml` 内容复制到 `_config.butterfly.yml` 去。( 注意: 复製的是主题的 `_config.yml` ,而不是 hexo 的 `_config.yml`)

> 注意：不要把主题目录的`_config.yml` 删掉

> 注意： 以后只需要在 `_config.butterfly.yml`进行配置就行。
> 如果使用了 `_config.butterfly.yml`， 配置主题的 `_config.yml` 将不会有效果。

Hexo会自动合并主题中的_`config.yml`和 `_config.butterfly.yml`里的配置，如果存在同名配置，会使用_`config.butterfly.yml`的配置，其优先度较高。

![image-20220706220927840](https://pic.xinsong.xyz/img/202207062209946.png)



### 语言

修改站点配置文件 `_config.yml`

默认语言是 en

主题支持三种语言

* default(en)
* zh-CN (简体中文)
* zh-TW (繁体中文)

### 网站资料

修改网站各种资料，例如标题、副标题和邮箱等个人资料，请修改博客根目录的`_config.yml`

### 导航菜单

```yaml
Home: / || fas fa-home
Archives: /archives/ || fas fa-archive
Tags: /tags/ || fas fa-tags
Categories: /categories/ || fas fa-folder-open
List||fas fa-list:
  Music: /music/ || fas fa-music
  Movie: /movies/ || fas fa-video
Link: /link/ || fas fa-link
About: /about/ || fas fa-heart
```

必须是 `/xxx/`，后面`||`分开，然后写图标名。

如果不希望显示图标，图标名可不写。

默认子目录是展开的，如果你想要隐藏，在子目录里添加 `hide` 。

```yaml
List||fas fa-list||hide:
  Music: /music/ || fas fa-music
  Movie: /movies/ || fas fa-video
```

注意：导航的文字可自行修改。

例如：

```yaml
menu:
  首页: / || fas fa-home
  时间轴: /archives/ || fas fa-archive
  标籤: /tags/ || fas fa-tags
  分类: /categories/ || fas fa-folder-open
  清单||fa fa-heartbeat:
    音乐: /music/ || fas fa-music
    照片: /Gallery/ || fas fa-images
    电影: /movies/ || fas fa-video
  友链: /link/ || fas fa-link
  关于: /about/ || fas fa-heart
```

### 代码

> 代码块中的所有功能只适用于 Hexo 自带的代码渲染
>
> 如果使用第三方的渲染器，不一定会有效

#### 代码高亮主题

`Butterfly` 支持6种代码高亮样式：

* darker
* pale night
* light
* ocean
* mac
* mac light

修改 `主题配置文件`

```yaml
highlight_theme: mac
```

#### 代码复制

主题支持代码复制功能

修改 `主题配置文件`

```yaml
highlight_copy: true
```

#### 代码框展开/关闭

在默认情况下，代码框自动展开，可设置是否所有代码框都关闭状态，点击>可展开代码

* true 全部代码框不展开，需点击`>`打开
* false 代码框展开，有`>`点击按钮
* none 不显示`>`按钮

修改 `主题配置文件`

```yaml
highlight_shrink: true #代码框不展开，需点击 '>' 打开
```

> 你也可以在post/page页对应的markdown文件front-matter添加highlight_shrink来独立配置。
>
> 当主题配置文件中的 highlight_shrink 设为true时，可在front-matter添加highlight_shrink: false来单独配置文章展开代码框。
>
> 当主题配置文件中的 highlight_shrink 设为false时，可在front-matter添加highlight_shrink: true来单独配置文章收缩代码框。
>

#### 代码换行

在默认情况下，Hexo 在编译的时候不会实现代码自动换行。如果你不希望在代码块的区域里有横向滚动条的话，那么你可以考虑开启这个功能。

修改 `主题配置文件`

```yaml
code_word_wrap: true
```

如果你是使用 highlight 渲染，需要找到你站点的 Hexo 配置文件_config.yml，将line_number改成false:

```yaml
highlight:
  enable: true
  line_number: false # <- 改这里
  auto_detect: false
  tab_replace:
```

设置`code_word_wrap`之后代码框内比较长的部分会自动换行，而不会有横向滚动条出现。

#### 代码高度限制

> 3.7.0 及以上支持

可配置代码高度限制，超出的部分会隐藏，并显示展开按钮。

```yaml
highlight_height_limit: false # unit: px
```

注意：

1. 单位是 px，直接添加数字，如 200

2. 实际限制高度为 highlight_height_limit + 30 px ，多增加 30px 限制，目的是避免代码高度只超出highlight_height_limit 一点时，出现展开按钮，展开没内容。

3. 不适用于隐藏后的代码块（ css 设置 display: none）

### 社交图标

Butterfly支持 [font-awesome v6](https://fontawesome.com/icons?from=io)图标.

书写格式 `图标名：url || 描述性文字`

```yaml
social:
  fab fa-github: https://github.com/sxfinn || Github
  fas fa-envelope: mailto:sxnicoa@gmail.com || Email
```

### 主页文章节选(自动节选和文章页description)

因为主题UI的关係，`主页文章节选`只支持`自动节选`和`文章页description`。

在butterfly里，有四种可供选择

1. description： 只显示description
2. both： 优先选择description，如果没有配置description，则显示自动节选的内容
3. auto_excerpt：只显示自动节选
4. false： 不显示文章内容

修改 `主题配置文件`

```yaml
index_post_content:
  method: 3
  length: 500 # if you set method to 2 or 3, the length need to config
```

description在front-matter里添加



### 顶部图

配置中的值：

| 配置             | 解释                                                         |
| :--------------- | :----------------------------------------------------------- |
| index_img        | 主页的 top_img                                               |
| default_top_img  | 默认的 top_img，当页面的 top_img 没有配置时，会显示 default_top_img |
| archive_img      | 归档页面的 top_img                                           |
| tag_img          | tag 子页面 的 默认 top_img                                   |
| tag_per_img      | tag 子页面的 top_img，可配置每个 tag 的 top_img              |
| category_img     | category 子页面 的 默认 top_img                              |
| category_per_img | category 子页面的 top_img，可配置每个 category 的 top_img    |

其它页面 （tags/categories/自建页面）和 文章页 的 `top_img` ，请到对应的 `md` 页面设置`front-matter`中的`top_img`

以上所有的 top_img 可配置以下值

> 3.2.0 以下版本的配置只支持
>
> * 留空，true 和 false - 显示默认的顔色
> * img链接 - 显示所配置的图片

| 配置的值                                                     | 效果                                                         |
| :----------------------------------------------------------- | ------------------------------------------------------------ |
| 留空                                                         | 显示默认的top_img（如有），否则显示默认的顔色<br/>（文章页top_img留空的话，会显示 cover 的值） |
| img链接                                                      | 图片的链接，显示所配置的图片                                 |
| 顔色(<br/>HEX值 - #0000FF<br/>RGB值 - rgb(0,0,255)<br/>顔色单词 - orange<br/>渐变色 - linear-gradient( 135deg, #E2B0FF 10%, #9F44D3 100%)<br/>） | 对应的顔色                                                   |
| transparent                                                  | 透明                                                         |
| false                                                        | 不显示 top_img                                               |

`tag_per_img` 和 `category_per_img` 是 3.2.0 新增的内容，可对 tag 和 category 进行单独的配置

并不推荐为每个 tag 和每个 category 都配置不同的顶部图，因为配置太多会拖慢生成速度

```yaml
tag_per_img：
  aplayer: https://xxxxxx.png
  android: ddddddd.png
  
category_per_img：
  随想: hdhdh.png
  推荐: ddjdjdjd.png
```

### 文章置顶

【推荐】hexo-generator-index从 2.0.0 开始，已经支持文章置顶功能。你可以直接在文章的front-matter区域里添加sticky: 1属性来把这篇文章置顶。数值越大，置顶的优先级越大。

### 文章封面

文章的markdown文档上,在`Front-matter`添加`cover`,并填上要显示的图片地址。
如果不配置`cover`,可以设置显示默认的`cover`.

如果不想在首页显示cover,可以设置为false

修改 `主题配置文件`

```yaml
cover:
  # 是否显示文章封面
  index_enable: true
  aside_enable: true
  archives_enable: true
  # 封面显示的位置
  # 三个值可配置 left , right , both
  position: both
  # 当没有设置cover时，默认的封面显示
  default_cover: 
```

当配置多张图片时,会随机选择一张作为cover.此时写法应为

```yaml
default_cover:
  - https://fastly.jsdelivr.net/gh/jerryc127/CDN@latest/cover/default_bg.png
  - https://fastly.jsdelivr.net/gh/jerryc127/CDN@latest/cover/default_bg2.png
  - https://fastly.jsdelivr.net/gh/jerryc127/CDN@latest/cover/default_bg3.png
```

### 文章页相关设置

#### 文章meta显示

这个选项是用来显示文章的相关信息的。

修改 `主题配置文件`

```yaml
post_meta:
  page:
    date_type: both # created or updated or both 主页文章日期是创建日或者更新日或都显示
    date_format: relative # date/relative 显示日期还是相对日期
    categories: true # true or false 主页是否显示分类
    tags: true # true or false 主页是否显示标籤
    label: true # true or false 显示描述性文字
  post:
    date_type: both # created or updated or both 文章页日期是创建日或者更新日或都显示
    date_format: relative # date/relative 显示日期还是相对日期
    categories: true # true or false 文章页是否显示分类
    tags: true # true or false 文章页是否显示标籤
    label: true # true or false 显示描述性文字
```

#### 文章版权

为你的博客文章展示文章版权和许可协议。

修改 `主题配置文件`

```yaml
post_copyright:
  enable: true
  decode: false
  author_href:
  license: CC BY-NC-SA 4.0
  license_url: https://creativecommons.org/licenses/by-nc-sa/4.0/
```

由于Hexo 4.1开始，默认对网址进行解码，以至于如果是中文网址，会被解码，可设置decode: true来显示中文网址。

如果有文章（例如：转载文章）不需要显示版权，可以在文章Front-matter单独设置

```yaml
copyright: false
```

从3.0.0开始，支持对单独文章设置版权信息，可以在文章Front-matter单独设置

```yaml
copyright_author: xxxx
copyright_author_href: https://xxxxxx.com
copyright_url: https://xxxxxx.com
copyright_info: 此文章版权归xxxxx所有，如有转载，请註明来自原作者
```

#### 文章打赏

在你每篇文章的结尾，可以添加打赏按钮。相关二维码可以自行配置。

对于没有提供二维码的，可配置一张软件的icon图片，然后在link上添加相应的打赏链接。用户点击图片就会跳转到链接去。

link可以不写，会默认为图片的链接。

修改 `主题配置文件`

```yaml
reward:
  enable: true
  QR_code:
    - img: /img/wechat.jpg
      link:
      text: 微信
    - img: /img/alipay.jpg
      link:
      text: 支付宝
```

#### TOC

在文章页，会有一个目录，用于显示TOC。

修改 `主题配置文件`

```yaml
toc:
  post: true
  page: false
  number: true
  expand: false
  style_simple: false # for post
```

| 属性         | 解释                                          |
| ------------ | --------------------------------------------- |
| post         | 文章页是否显示 TOC                            |
| page         | 普通页面是否显示 TOC                          |
| number       | 是否显示章节数                                |
| expand       | 是否展开TOC                                   |
| style_simple | 简介模式啊（侧边栏只显示TOC，只对文章页有效） |

##### 为特定的文章配置

在你的文章md文件的头部，加入toc_number和toc，并配置true或者false即可。

主题会优先判断文章Markdown的Front-matter是否有配置，如有，则以Front-matter的配置为準。否则，以主题配置文件中的配置为准

#### 相关文章

相关文章推荐的原理是根据文章tags的比重来推荐

修改 `主题配置文件`

```yaml
related_post:
  enable: true
  limit: 6 # 显示推荐文章数目
  date_type: created # or created or updated 文章日期显示创建日或者更新日
```

#### 文章锚点

开启文章锚点后，当你在文章页进行滚动时，文章链接会根据标题ID进行替换
(注意: 每替换一次，会留下一个歷史记录。所以如果一篇文章有很多锚点的话，网页的歷史记录会很多。)

修改 `主题配置文件`

```yaml
# anchor
# when you scroll in post , the url will update according to header id.
anchor: true
```

#### 文章过期提醒

可设置是否显示文章过期提醒，以更新时间为基准。

```yaml
# Displays outdated notice for a post (文章过期提醒)
noticeOutdate:
  enable: true
  style: flat # style: simple/flat
  limit_day: 365 # When will it be shown
  position: top # position: top/bottom
  message_prev: It has been
  message_next: days since the last update, the content of the article may be outdated
```

limit_day： 距离更新时间多少天才显示文章过期提醒

message_prev ： 天数之前的文字

message_next：天数之后的文字

#### 文章编辑按钮

在文章标题旁边显示一个编辑按钮，点击会跳转到对应的链接去。

```yaml
# Post edit
# Easily browse and edit blog source code online.
post_edit:
  enable: false
  # url: https://github.com/user-name/repo-name/edit/branch-name/subdirectory-name/
  # For example: https://github.com/jerryc127/butterfly.js.org/edit/main/source/
  url:
```

#### 文章分页按钮

可设置分页的逻辑，也可以关闭分页显示

```yaml
# post_pagination (分页)
# value: 1 || 2 || false
# 1: The 'next post' will link to old post
# 2: The 'next post' will link to new post
# false: disable pagination
post_pagination: false
```

| 参数                   | 解释                 |
| ---------------------- | -------------------- |
| post_pagination: false | 关闭分页按钮         |
| post_pagination: 1     | 下一篇显示的是旧文章 |
| post_pagination: 2     | 下一篇显示的是新文章 |

### 头像

修改 `主题配置文件`

```yaml
avatar:
  img: /img/avatar.png
  effect: true # 头像会一直转圈
```

### 图片描述

可开启图片Figcaption描述文字显示

优先显示图片的 title 属性，然后是 alt 属性

修改 `主题配置文件`

```yaml
photofigcaption: true
```

### 复制相关配置

可配置网站是否可以复製、复製的内容是否添加版权信息

```markdown
# copy settings
# copyright: Add the copyright information after copied content (复製的内容后面加上版权信息)
copy:
  enable: true
  copyright:
    enable: true
    limit_count: 50
```

| 配置        | 解释                                                         |
| ----------- | ------------------------------------------------------------ |
| enable      | 是否开启网站复制权限                                         |
| copyright   | 复制的内容后面加上版权信息                                   |
| enable      | 是否开启复复制版权信息添加                                   |
| limit_count | 字数限制，当复制文字大于这个字数限制时，将在复製的内容后面加上版权信息 |

### Footer设置

#### 博客年份

since是一个来展示你站点起始时间的选项。它位于页面的最底部。

修改 `主题配置文件`

```yaml
footer:
  owner:
    enable: true
    since: 2018
```

#### 页脚自定义文本

custom_text是一个给你用来在页脚自定义文本的选项。通常你可以在这里写声明文本等。支持 HTML。

修改 主题配置文件

```yaml
custom_text: Hi, welcome to my <a href="https://butterfly.js.org/">blog</a>!
```

对于部分人需要写 ICP 的，也可以写在 custom_text里

```yaml
custom_text: <a href="icp链接"><img class="icp-icon" src="icp图片"><span>备案号：xxxxxx</span></a>
```

### 右下角按钮

#### 简繁互换

简体繁体互换

右下角会有简繁转换按钮。

修改 `主题配置文件`

```yaml
translate:
  enable: true
  # 默认按钮显示文字(网站是简体，应设置为'default: 繁')
  default: 简
  #网站默认语言，1: 繁体中文, 2: 简体中文
  defaultEncoding: 1
  #延迟时间,若不在前, 要设定延迟翻译时间, 如100表示100ms,默认为0
  translateDelay: 0
  #当文字是简体时，按钮显示的文字
  msgToTraditionalChinese: "繁"
  #当文字是繁体时，按钮显示的文字
  msgToSimplifiedChinese: "简"
```

#### 夜间模式

右下角会有夜间模式按钮

修改 `主题配置文件`

```yaml
# dark mode
darkmode:
  enable: true
  # dark mode和 light mode切换按钮
  button: true
  autoChangeMode: false
```

> V2.0.0 开始增加一个选项，可开启自动切换light mode 和 dark mode
>
> autoChangeMode: 1 跟随系统而变化，不支持的浏览器/系统将按照时间晚上6点到早上6点之间切换为 dark mode
>
> autoChangeMode: 2 只按照时间 晚上6点到早上6点之间切换为 dark mode,其余时间为light mode
>
> autoChangeMode: false 取消自动切换
>

#### 阅读模式

阅读模式下会去掉除文章外的内容，避免干扰阅读。

只会出现在文章页面，右下角会有阅读模式按钮。

修改 `主题配置文件`

```yaml
readmode: true
```

#### 按钮排序

```yaml
# Don't modify the following settings unless you know how they work (非必要请不要修改 )
# Choose: readmode,translate,darkmode,hideAside,toc,chat,comment
# Don't repeat 不要重复
rightside_item_order:
  enable: false
  hide: # readmode,translate,darkmode,hideAside
  show: # toc,chat,comment
```

### 侧边栏设置

#### 侧栏排版

可自行决定哪个项目需要显示，可决定位置，也可以设置不显示侧边栏。

修改 `主题配置文件`

```yaml
aside:
  enable: true
  hide: false
  button: true
  mobile: true # 手机页面（ 显示宽度 < 768px ）是否显示aside内容
  position: right # left or right
  display:
    archive: true
    tag: true
    category: true
  card_author:
    enable: true
    description:
    button:
      enable: true
	  icon: fab fa-github
      text: Github
      link: https://github.com/jerryc127/hexo-theme-butterfly
  card_announcement:
    enable: true
    content: This is my Blog
  card_recent_post:
    enable: true
    limit: 5 # if set 0 will show all
    sort: date # date or updated
  card_categories:
    enable: true
    limit: 8 # if set 0 will show all
    expand: none # none/true/false
  card_tags:
    enable: true
    limit: 40 # if set 0 will show all
    color: false
  card_archives:
    enable: true
    type: monthly # yearly or monthly
    format: MMMM YYYY # eg: YYYY年MM月
    order: -1 # Sort of order. 1, asc for ascending; -1, desc for descending
    limit: 8 # if set 0 will show all
  card_webinfo:
    enable: true
    post_count: true
    last_push_date: true
```

#### 访问人数

访问 busuanzi 的官方网站查看更多的介绍。

修改 `主题配置文件`

```yaml
busuanzi:
  site_uv: true
  site_pv: true
  page_pv: true
```

#### 运行时间

网页已运行时间

修改 `主题配置文件`

```yaml
runtimeshow:
  enable: true
  publish_date: 6/7/2018 00:00:00  
  ##网页开通时间
  #格式: 月/日/年 时间
  #也可以写成 年/月/日 时间
```

#### 最新评论

> 3.1.0 起支持

> 最新评论只会在刷新时才会去读取，并不会实时变化
>
> 由于 API 有 访问次数限制，为了避免调用太多，主题默认存取期限为 10 分鐘。也就是説，调用后资料会存在 localStorage 里，10分鐘内刷新网站只会去 localStorage 读取资料。 10 分鐘期限一过，刷新页面时才会去调取 API 读取新的数据。（ 3.6.0 新增了 storage 配置，可自行配置缓存时间）

在侧边栏显示最新评论板块

修改 `主题配置文件`

```js
# Aside widget - Newest Comments
newest_comments:
  enable: true
  sort_order: # Don't modify the setting unless you know how it works
  limit: 6
  storage: 10 # unit: mins, save data to localStorage
  avatar: true
```

部分配置解释

| 配置    | 解释                     |
| ------- | ------------------------ |
| limit   | 显示的数量               |
| storage | 设置缓存时间，单位：分钟 |
| avatar  | 是否显示头像             |

#### 自定义添加栏目





### 搜索系统

#### 本地搜索

> 记得运行 hexo clean

1. 你需要安装[wzpan/hexo-generator-search：为 Hexo 生成搜索数据的插件。](https://github.com/wzpan/hexo-generator-search)，根据他的相应文档做相应配置。
2. 修改 `主题配置文件`

```yaml
local_search:
  enable: true
  preload: false
  CDN:
```



















