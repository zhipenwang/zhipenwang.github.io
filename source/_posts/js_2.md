---
title: jQuery对于父子、同级元素的处理方式
date: 2018-05-01 20:21:01
tags: Web前端
categories: jQuery
---
>上一节介绍了一些关于表单处理的元素内容
>本节介绍jquery对于父、同级、子元素的处理方式。

### 父窗口与子窗口处理
```
父窗口：
<input name='img' type='text' />
<img id='img' src='' />
layer.open({
    type: 2,
    title: 'title',
    area: ['500px', '350px'],
    content 'son.html'
})

子窗口：
var img_url;
parent.$("input[name='img']").val(img_url);
parent.$('#img').attr('src', img_url);
parent.layer.closeAll();
```

## jQuery获取父元素节点、子元素节点及兄弟元素节点的方法
```
<ul class="par">
	<li id="firstli">
		<h3 class="title">one</h3>
		<ul class="par">
			<li id="one">one_first</li>
			<li>two_first</li>
		</ul>
	</li>
</ul>
```
### jquery父节点的获取
#### 使用parent()获取父节点
```
$("#one").parent().parent();  // 获取 id为firstli的li标签节点
$("#one").parent().parent('.par'); // 获取最上面 class为par的ul节点
$("#one").parent('.par'); // 获取 id为one的上一级class为par的ul节点
```

#### 使用parents()和closest()获取父节点
>1. closest从当前元素开始匹配寻找，逐级向上寻找直到找到匹配的元素后就停止了，返回0或者1个元素
>2. parents从父元素开始匹配寻找，一直向上查找直到找到根元素，将所有元素放到另外一个集合中，返回0、1或者更多元素
```
$("#one").parnets('.par');  // 找出所有class为par的父节点/父父节点
$("one").closest('.par');  // 获取最近一层的父级class为par的ul节点
```

### jquery兄弟节点的获取
#### parent父节点再find子节点
```
$('.title').parent().find('ul');  // 找到class为title的兄弟节点ul，即class为par的ul
```
#### sibingls()获取兄弟节点
```
$('.title').sibings('ul');  // 找到class为title的兄弟节点ul，即class为par的ul
```

### jquery子节点的获取
####  :first方式
```
$('.par:first-child');  // 获取id为firstli的li节点
```
#### 选择器获取
```
$('#firstli h3.title');  // 获取class为title的h3节点
```
#### find()函数
```
$('#firstli').find('h3');  // 获取class为title的h3节点
```
#### children()函数
```
$('#firstli').children('h3.title');   // 获取class为title的h3节点
```