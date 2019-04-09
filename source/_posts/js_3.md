---
title: js与jquery关于验证页面元素存在的方法及不同之处
date: 2018-05-05 20:21:01
tags: Web前端
categories: jQuery
---
>在javascript中我们可以通过以下代码判断页面中是否存在某个元素

```
obj = document.getElementById("someID");
if (obj) {
     obj.innerText("Extsts");
}
```

>那么在jQuery，我们如何判断页面元素存在与否呢？如果参照上面的传统Javascript的写法，我们第一个想到的办法会是：

```
if ($("#someID")){
      $("#someID").text("hi");
}
```

可是这么写是不对的！因为<strong>jQuery对象永远都有返回值</strong>，所以$("someID") 总是TRUE ，IF语句没有起到任何判断作用。
我们知道，jQuery选择器获取页面的element时，无论element是否存在，都会返回一个对象。例如：
```
var my_element = $("#element_Id" );
```
此时的变量my_element就是一个对象，既然是一个对象，这个对象就具有length的属性，因此，用以下代码可以判断元素（对象）是否存在

正确的写法应该是：
```
if ( $("#someID").length > 0 ) {
     $("#someID").text("Extsts");
}
```


>注意 ：判断某个页面元素存在与否在jQuery实际上是没有必要的，jQuery本身会忽略 对一个不存在的元素进行操作，并且不会报错, 所以这么写代码会存在bug。
假如不存在someID这个元素，我们照样可以执行一下代码，并不会报错。

```
var value=$('#someID').length;
if(value>0){
     alert('Extsts');
}else{
      alert('not Extsts');
}
```

>JS判断变量是否为空或是否null
```
/** 
* 判断是否null 
* @param data 
*/ 
function isNull(data){ 
	return (data == "" || data == undefined || data == null) ? "暂无" : data; 
}
```