---
title: composer构建PHP框架——初始化
date: 2017-04-21 19:21:01
tags: Composer
categories: composer使用
---
>“一个时代结束了，另一个时代开始了。”
Framework Interoperability Group（框架可互用性小组），简称 FIG，成立于 2009 年。FIG 最初由几位知名 PHP 框架开发者发起，在吸纳了许多优秀的大脑和强健的体魄后，提出了 PSR-0 到 PSR-4 五套 PHP 非官方规范：

~~~
* PSR-0 (Autoloading Standard) 自动加载标准

* PSR-1 (Basic Coding Standard) 基础编码标准

* PSR-2 (Coding Style Guide) 编码风格向导

* PSR-3 (Logger Interface) 日志接口

* PSR-4 (Improved Autoloading) 自动加载优化标准
~~~

### 创建composer.json
在网站根目录下创建新文件夹 `my-framework` （My First Framework based on Composer），然后在此文件夹内创建 `composer.json` 文件
~~~
{
	"require":{
		
	}
}
~~~
### 初始化composer
打开命令行cmd，进入刚刚新建的目录 `my_framework`，执行命令
~~~
composer update
~~~
执行成功后生成了扩展包` vendor`
<center>
![](/img/composer-2-1.png)
</center>
此时目录为：
<center>
![](/img/composer-2-2.png)
</center>

到此，composer初始化完成！