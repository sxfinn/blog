# WordPress 固定链接(伪静态)的设置方法及建议设置

WordPress 是一个 CMS 管理系统，也就是说，WordPress 的文章、页面、存档页都是通过程序从数据库里面获取数据生成的。虽然 WordPress 的页面可以有千千万万个，但是我们访问这些页面的入口只有一个，那就是根目录的「index.php」，我们访问一个页面时，其实访问的就是「inxdex.php」这个文件，然后这个文件根据全局变量和页面的一些参数，从数据库中获取数据、生成页面，最终发送给我们。



### 为什么要设置伪静态？

---

默认安装环境下，我们访问 WordPress 文章页的 URL 地址为 `https://exaple.com/index.php?p=123`，这个 URL 中，example.com 是我们的域名，“index.php” 就是我们上面说到的 WordPress 页面入口文件，`?p=123` 是这个 URL 的参数，其中 `123` 为页面 ID，index.php 就是根据这个页面 ID 从 [WordPress 数据库](https://www.wpzhiku.com/tag/wordpress数据库/)中获取页面内容，生成页面展示给我们的。



### 怎么去掉 index.php？

---

为了让 URL 更好看一些，对 SEO 更友好一些，很多朋友都会想把这个 index.php 去掉，怎么办到呢？很简单，在 WordPress 的「**设置 > 固定链接**」设置中选择除了「**朴素**」之外的其他选项就可以了，保存之后，我们就为 WordPress 开启了伪静态设置。

为了让 URL 更具语义，我们建议选择「**文章名**」的 URL 结构，如果你不想让 URL 中出现中文，可以使用我们开发的 [Wenprise Pinyin Slug](https://www.wpzhiku.com/wenprise-pinyin-slug/) 插件自动把 URL 中的中文转换为拼音或英文名。



### Apache、Nginx、IIS 等服务器对伪静态的支持

---

做了固定链接设置之后，可能有朋友发现，自己的网站，除了首页，其他页面都打不开了，这是因为你的服务器没有配置对 URL 重定向（也就是伪静态的支持），我们需要为服务器打开 URL 重定向支持。



### Nginx 伪静态规则

---

根据我们所了解到的情况，大部分 WordPress 站点现在都是使用的 Nginx 作为 Web 服务器，为 Nginx 添加伪静态设置非常简单，找到您的虚拟主机配置文件，添加以下 Nginx 规则，然后运行 `nginx -s reload` 重新加载 Nginx 配置就可以了。

```markup
location / {
    index index.php index.html index.htm;
    try_files $uri $uri/ /index.php?$args;
}
```



### IIS 伪静态规则

---

相信用 IIS 作为 WordPress 的 Web 服务器的朋友不多吧？具体设置方法就不说了，以防万一用得着，把 IIS URL 重定向的设置方法参考链接放到这里好了。

- [IIS URL Rewrite 模块](https://www.iis.net/downloads/microsoft/url-rewrite)
- [Enabling Pretty Permalinks in WordPress](https://docs.microsoft.com/en-us/iis/extensions/url-rewrite-module/enabling-pretty-permalinks-in-wordpress)



### Apache 伪静态规则

---

大多数 Apache 服务器都支持使用 .htaccess 文件设置重定向规则，只要您的服务器支持 .htacees 文件，在设置 WordPress 固定链接的时候，WordPress 会自动生成或更新这个文件，不用太多d的设置。

如果您的 Apache 服务器不支持 .htaccess，找管理员提供支持或者参考下面的链接设置即可。

[怎么在 Apache 设置设置 htaccess 文件](https://www.linode.com/docs/web-servers/apache/how-to-set-up-htaccess-on-apache/)

基本上，支持伪静态设置是每个 WordPress 服务器的基本修养之一，URL 伪静态也是 WordPress SEO 需要做的基本事项之一，每个有追求的 WordPress 站点都要做到。