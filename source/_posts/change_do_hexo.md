---
title: 更换电脑后的博客续写
date: 2018-04-04 13:21:01
tags: hexo
categories: 搭建博客
---
当重装电脑之后，或者想在其他电脑上修改博客，可以使用下列步骤：
1. 使用git clone git@github.com:zhipenwang/zhipenwang.github.io.git拷贝仓库（默认分支为master）；
2. 在本地新拷贝的http://zhipenwang.github.io文件夹下通过Git bash依次执行下列指令：
```
npm install hexo
npm install
npm install hexo-deployer-git
```
	（记得，不需要hexo init这条指令）。