---
title: Hexo添加字数统计、阅读时长、浏览次数
date: 2017-04-19 19:21:01
tags: [hexo,next]
categories: 搭建博客
---
## 统计插件

### 配置
NexT 主题默认已经集成了文章【浏览次数】、【字数统计】、【阅读时长】统计功能，如果我们需要使用，只需要修改主题配置文件 _config.yml 即可。如下所示：
配置浏览次数
```
busuanzi_count:
  # count values only if the other configs are false
  enable: true
  # custom uv span for the whole site
  site_uv: true
  site_uv_header: <i class="fa fa-user"></i> 访问人数
  site_uv_footer:
  # custom pv span for the whole site
  site_pv: true
  site_pv_header: <i class="fa fa-eye"></i> 总访问量
  site_pv_footer:
  # custom pv span for one page only
  page_pv: true
  page_pv_header: <i class="fa fa-file-o"></i> 浏览
  page_pv_footer: 次

```
配置字数统计、阅读时长，wordcount与min2read统计功能
```
# Post wordcount display settings
# Dependencies: https://github.com/willin/hexo-wordcount
post_wordcount:
  item_text: true
  wordcount: true         # 单篇 字数统计
  min2read: true          # 单篇 阅读时长
  totalcount: false       # 网站 字数统计
  separated_meta: true
```

预览的时候如果出现字数统计和阅读时长失效的情况，一般是因为没有安装 hexo-wordcount 插件，查看 Hexo 插件：
```
hexo --debug
``
### 安装

如果没有安装 hexo-wordcount 插件，先安装该插件：
```
npm i --save hexo-wordcount
```
*** Node 版本 7.6.0 之前,请安装 2.x 版本 (Node.js v7.6.0 and previous) ，安装命令如下：***
```
npm install hexo-wordcount@2 --save
```
安装完成后，重新执行启动服务预览就可以了。

### 显示文字

打开`post.swig`文件，路径如下：`xxx_blog/themes/next/layout/_macro/post.swig`

![](/img/view_count.png)