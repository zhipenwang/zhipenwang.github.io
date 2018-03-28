---
title: hexo发布文章
date: 2017-03-19 18:21:01
tags: Article
categories: 搭建博客
---
* 在部署和搭建好hexo博客后，怎么写好一篇博客并发布到在线地址呢？

1、找到你本地的博客地址，在source/_posts目录下新建新文件，后缀为md，写一篇markdown文章
2、打开git bash命令端
本地预览：
```
hexo server
```
提交到git仓库，同步到在线博客
```
hexo clean
hexo generate
hexo deploy
```
然后就可以在你的博客看到更新的文章了。