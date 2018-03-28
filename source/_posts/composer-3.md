---
title: composer构建PHP框架——构建路由
date: 2017-04-21 19:21:01
tags: Composer
categories: composer使用
---
## 路由选择安装
本节开始构建路由，先去 GitHub 搜一下：[点此查看搜索结果](https://github.com/search?l=PHP&o=desc&q=router&ref=searchresults&s=stars&type=Repositories&utf8=%E2%9C%93)

推荐 [https://github.com/NoahBuscher/Macaw](https://github.com/NoahBuscher/Macaw)，对应的 Composer 包为 noahbuscher/macaw 。

下面开始安装它，更改 composer.json：
~~~
{
	"require":{
		"noahbuscher/macaw": "dev-master"
	}
}
~~~
运行 composer update，成功之后将得到以下目录：
<center>
![](/img/composer-3-1.png)
</center>
至此，Macaw安装成功！

## 站点入口文件与环境
在项目目录下新建public 文件夹，这个文件夹将是用户唯一可见的部分。在文件夹下新建 index.php 文件：
~~~
<?php

// Autoload 自动载入
require '../vendor/autoload.php';

// 路由配置
require '../config/routes.php';
~~~
上面一行表示引入 Composer 的自动载入功能，下面一行表示载入路由配置文件。
然后继续在项目目录下新建config文件夹，在config文件夹内新建 routs.php 文件，内容如下：
~~~
<?php

use NoahBuscher\Macaw\Macaw;

Macaw::get('fuck', function() {
  echo "成功！";
});

Macaw::get('(:all)', function($fu) {
  echo '未匹配到路由<br>'.$fu;
});

Macaw::dispatch();
~~~

然后访问你的地址即可：http://127.0.0.66/index.php/fuck

注意：如果要配置域名地址进行映射要指向 `public/index.php` 文件
![](/img/composer-3-2.png)