---
title: Hexo NexT主题内给每篇文章后添加结束标语
date: 2017-04-18 19:21:01
tags: [hexo,next]
categories: 搭建博客
---
给文章后面添加结束标语

### 新建文件
在`\themes\next\layout\_macro`中新建`passage-end-tag.swig`文件，添加代码至该文件中：
```
<div>
    {% if not is_index %}
        <div style="text-align:center;color: #ccc;font-size:22px;">-------------本文结束<i class="fa fa-paw"></i>感谢您的阅读-------------</div>
    {% endif %}
</div>
```
### 修改post.swig
打开\themes\next\layout\_macro\post.swig文件，在post-body后，post-footer前，添加下面内容：
```
<div>
  {% if not is_index %}
    {% include 'passage-end-tag.swig' %}
  {% endif %}
</div>
```
### 修改_config
打开主题配置文件（_config.yml),在末尾添加：
```
# 文章末尾添加“本文结束”标记
passage_end_tag:
  enabled: true
```
至此，就完成了关于添加文章结束标语的功能，具体的效果，此刻，想必你也看到了，就在下边。
