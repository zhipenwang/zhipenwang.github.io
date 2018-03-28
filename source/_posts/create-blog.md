---
title: github+hexo搭建个人博客
date: 2017-03-18 19:21:01
tags: Article
categories: 搭建博客
---
**各位有兴趣搭建博客或者正在搭建博客却遇到问题的朋友们，不妨抽空来围观一下我搭建的过程中遇到的坑和填的土。。**

*声明一点，以下所有步骤都是基于Windows系统的搭建，由于本人没有真正在其他系统上搭建过，就不画蛇添足了，所以其他系统的小心不要入错入坑咯*

下面先说一下基本思路（给大家一个清晰的搭建过程）：

> 1、在Windows系统下搭建运行hexo环境的前提是先搭建**nodejs**跟**git**
> 2、搭建好nodejs跟git后就可以在系统中搭建hexo环境
> 3、申请github.com帐号，创建自己的repository（仓库）——注意仓库名必须是：yourusername.github.io，具体原因及内容后面细讲、
> 4、本地hexo环境部署到github上 5、yourusername.github.io查看自己的博客，成功！

### 第一步：搭建nodejs环境
参考菜鸟教程的nodejs，很是详细：[nodejs-菜鸟教程](http://www.runoob.com/nodejs/nodejs-install-setup.html)

大家可以去[nodejs](https://nodejs.org/en/download/)官网下载，记得要下载跟自己Windows系统一致的版本。（64bit的下载window-install的64-bit；同理：32bit的下载32-bit的）。
下载之后双击安装，选择自己要安装的路径地址，一直next到finish就可以了
![](/img/install-node-msi-version-on-windows-step5.png)
安装后检测PATH环境变量是否配置了Node.js，点击开始=》运行=》输入"cmd" => 输入命令"path"，输出结果中有nodejs的环境变量说明安装成功。（如下我的环境变量已经有nodejs了）
~~~
PATH=C:\oraclexe\app\oracle\product\10.2.0\server\bin;C:\Windows\system32;
C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;
c:\python32\python;C:\MinGW\bin;C:\Program Files\GTK2-Runtime\lib;
C:\Program Files\MySQL\MySQL Server 5.5\bin;C:\Program Files\nodejs\;
~~~
然后可以检查一下Node.js版本，直接命令行输入 node --version
![](/img/node-version.png)
ok，nodejs环境到此完成。
### 第二步：搭建GIT环境
同样，大家可以参考[百度经验](http://jingyan.baidu.com/article/90895e0fb3495f64ed6b0b50.html)

下载地址可以前往[git官网](http://msysgit.github.io/)进行下载，同样下载跟自己系统位数一致的版本
下载之后双击选择自己要安装的路径地址一键next安装就好了
![](/img/git-install.jpg)
安装好之后在任意位置右键都可以看到关于git的一些菜单选项：
![](/img/git_menu.png)
ok，至此git安装完成。

### 第三步：搭建hexo环境
在E盘下建立hexo文件夹（或者你选择的任何路径）
在nodejs和git环境都搭建完成后，在刚刚建立好的hexo文件夹中点击鼠标右键，选择**git bash**，
输入以下命令安装hexo
~~~
npm install -g hexo
~~~
接下来是比较重要的步骤，都是在git bash命令行操作，要认真看清楚：
* 1、初始化你的博客
~~~php
	hexo init
~~~
>执行后hexo会在你的站点目录下生成网站所需的文件

* 2、安装node_modules：
~~~
npm install
~~~
>在此目录中生成node_modules

* 3、本地浏览器查看：
~~~
hexo server  (此命令也可以简写为 hexo s)
~~~
>这时候会出现如下信息：
[info] Hexo is running at http://localhost:4000/. 
Press Ctrl+C to stop. 

这个时候在你的浏览器输入http://localhost:4000/你就可以看到本地的博客搭建成功。
这个时候可以轻松一下，随便浏览一下自己的博客熟悉一下

### 第四步：本地hexo部署到github上
1、首先要有github帐号，可以前往[github官网](http://www.github.com/)注册一个自己的github帐号，登录成功后，点击右上角的+号新建一个仓库
![](/img/new_repository.png)
注意，仓库名必须跟自己的username一致，格式是username.github.io
![](/img/username_git.png)