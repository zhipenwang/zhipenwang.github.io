---
title: Hexo NexT主题内接入网页在线联系功能
date: 2017-04-20 19:12:49
tags: [hexo,next]
categories: 搭建博客
---
Hexo博客如何添加在线联系功能呢,发现了一个不错的网站可以提供在线联系的服务，当有用户在网页上给你留言后会通过邮件或者微信通知你，可以及时的解答用户的疑问。

### 注册
首先在(DaoVoice)[http://www.daovoice.io/]注册个账号。
登录上去之后进行配置，配置方法如下:
首先到DaoVoice上注册一个账号,注册完成后会得到一个app_id，获取appid的步骤如下图所示:
![](/img/daovoice.png)

以next主题为例,打开/themes/next/layout/_partials/head.swig文件添加如下代码：
```
{% if theme.daovoice %}
  <script>
  (function(i,s,o,g,r,a,m){i["DaoVoiceObject"]=r;i[r]=i[r]||function(){(i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;a.charset="utf-8";m.parentNode.insertBefore(a,m)})(window,document,"script",('https:' == document.location.protocol ? 'https:' : 'http:') + "//widget.daovoice.io/widget/0f81ff2f.js","daovoice")
  daovoice('init', {
      app_id: "{{theme.daovoice_app_id}}"
    });
  daovoice('update');
  </script>
{% endif %}
```
接着打开主题配置文件_config.yml，添加如下代码：
```
# Online contact 
daovoice: true
daovoice_app_id: 这里输入前面获取的app_id
```
需要注意的是,next主题下聊天的按钮会和其他按钮重叠到一起，可以到聊天设置，修改下按钮的位置:
![](/img/chat.png)
最后到右上角选择管理员，微信绑定,可以绑定你的微信号，关注公众号后打开小程序，就可以实时收发消息，有新的消息也会通过微信通知，设置页面如下:
![](/img/wechat.png)