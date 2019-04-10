---
title: hexo换了设备之后本地没问题，访问域名空白
date: 2019-04-10 13:28:01
tags: Article
categories: 搭建博客
---
* hexo三件套
```
hexo clean
hexo g
hexo d
```

> 本地clone了仓库后，执行hexo s本地没问题
> 问题点：  
> 本地新写了博客，然后执行hexo三件套,线上打开空白了  
>    解决：本地项目目录下Data/和archives/文件内清空，执行hexo三件套  
> 如果还是不行，继续：  
> 	 解决：将本地项目目录下的 .deploy_git/文件夹删除，重新执行hexo三件套  
> 这个时候一般就解决问题了