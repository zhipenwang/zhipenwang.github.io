---
title: composer安装与使用
date: 2017-04-21 19:21:01
tags: Composer
categories: composer使用
---
## 安装
1、点击下载：[composer](https://getcomposer.org/download/)

<center>
![](/img/composer-1-1.png)
</center>
2、下载好之后双击直接安装，Next即可：

<center>
![](/img/composer-1-2.png)
</center>
3、然后选择php.exe文件：

<center>
![](/img/composer-1-3.png)
</center>

接下来就一直next到安装完成即可。

## 使用
>[info]使用composer安装ThinkPHP5

通过[composer中国镜像](https://pkg.phpcomposer.com/)进行初步了解
![](/img/composer-1-4.png)

##开始命令行操作
### 1、中国镜像下载
参考[tp5看云文档](http://www.kancloud.cn/thinkphp/thinkphp5_quickstart/145249)

由于国外网站访问很慢，国内提供了很好的镜像，所以使用国内镜像：
~~~
Composer config -g repo.packagist composer https://packagist.phpcomposer.com
~~~
![](/img/composer-1-5.png)

### 2、下载tp5框架
进入站点根目录www操作命令行：
~~~
composer create-project topthink/think tp5  --prefer-dist
~~~
其中tp5可写填写任何你愿意的名称
![](/img/composer-1-6.png)

## 框架安装完成
耐心等待几分钟的安装过程，安装完成之后本地已经生成了tp5的新框架内容
![](/img/composer-1-7.png)

## composer主要的功能：安装扩展包
composer的最主要的功能：dependency manager for PHP
进入项目目录tp5，执行命令：
~~~
composer require riverslei/payment
~~~
下载riverslei/payment 集成支付宝、微信支付等流行的支付接口到项目中：
Composer.json中新增了这一句：
![](/img/composer-1-8.png)

项目目录中也已经安装好了扩展包：
![](/img/composer-1-9.png)