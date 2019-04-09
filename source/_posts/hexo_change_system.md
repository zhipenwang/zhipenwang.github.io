---
title: hexo在其他设备或系统上续写
date: 2017-03-20 18:21:01
tags: Article
categories: 搭建博客
---
>注意：需要将原系统中的原项目所有内容都发布到github上  
> 新系统需要安装node、git

>1. 新系统中git clone blog@github.com到本地  
>2. 新系统中切换分支 git checkou branch  
>3. 安装hexo：npm install hexo
>4. 初始化：npm install; npm install hexo-deployer-git  
>5. 然后就可以执行hexo：hexo clean; hexo g; hexo s