## 生成文章唯一链接

Hexo的默认文章链接格式是年，月，日，标题这种格式来生成的。如果你的标题是中文的话，那你的URL链接就会包含中文，

复制后的URL路径就是把中文变成了一大堆字符串编码，如果你在其他地方用这边文章的url链接，偶然你又修改了改文章的标题，那这个URL链接就会失效。为了给每一篇文章来上一个属于自己的链接，写下此教程，利用 hexo-abbrlink 插件，A Hexo plugin to generate static post link based on post titles ,来解决这个问题。 

参考github官方： hexo-abbrlink 按照此教程配置完之后如下：

1. 安装插件，在博客根目录 [Blogroot] 下打开终端，运行以下指令：

```powershell
npm install hexo-abbrlink --save
```

2. 修改 config.yml 文件中的永久链接：

```yaml
permalink: posts/:abbrlink/ 
# or
permalink: posts/:abbrlink.html
```

有两种设置：

```yaml
alg -- Algorithm (currently support crc16 and crc32, which crc16 is default)
rep -- Represent (the generated link could be presented in hex or dec value)
```

例如：

```yaml
# abbrlink config
abbrlink:
  alg: crc32      #support crc16(default) and crc32
  rep: hex        #support dec(default) and hex
```

最终生成的文章名是根据文章的发布时间戳生成的。
