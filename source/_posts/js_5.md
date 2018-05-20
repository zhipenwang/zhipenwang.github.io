---
title: jQuery的三种bind/One/Live/On事件绑定使用方法
tags: Web前端
categories: jQuery
---
### on(events,[selector],[data],fn)
> events:一个或多个用空格分隔的事件类型和可选的命名空间，如"click"或"keydown.myPlugin" 。
> selector:一个选择器字符串用于过滤器的触发事件的选择器元素的后代。如果选择器为null或省略，当它到达选定的元素，事件总是触发。
> data:当一个事件被触发时要传递event.data给事件处理函数。
> fn:该事件被触发时执行的函数。 false 值也可以做一个函数的简写，返回false

#### 替换bind()
> 当第二个参数'selector'为null时，on()和bind()其实在用法上基本上没有任何区别了，所以我们可以认为on()只是比bind()多了一个可选的'selector'参数，所以on()可以非常方便的换掉bind()

#### 替换live()
> 在1.4之前相信大家非常喜欢使用live(),因为它可以把事件绑定到当前以及以后添加的元素上面，当然在1.4之后delegate()也可以做类似的事情了。live()的原理很简单，它是通过document进行事件委派的，因此我们也可以使用on()通过将事件绑定到document来达到live()一样的效果。

#### live()写法
```
$('#list li').live('click', '#list li', 
	function() {
	//function code here.
});
```

#### on()写法
```
$(document).on('click', '#list li', 
function() {
 //function code 
here.
});
```
> 这里的关键就是第二个参数'selector'在起作用了。它是一个过滤器的作用，只有被选中元素的后代元素才会触发事件。

#### 替换delegate()
> delegate()是1.4引入的，目的是通过祖先元素来代理委派后代元素的事件绑定问题，某种程度上和live()优点相似。只不过live()是通过document元素委派，而delegate则可以是任意的祖先节点。使用on()实现代理的写法和delegate()基本一致。

#### delegate()的写法
```
$('#list').delegate('li', 'click', 
function() {
 //function code here.
});
```
#### on()写法
```
$('#list').on('click', 'li', function() 
{
 //function code here.
});
```
> 貌似第一个和第二个参数的顺序颠倒了一下，别的基本一样。

### 总结
> jQuery推出on()的目的有2个，一是为了统一接口，二是为了提高性能，所以从现在开始用on()替换bind(), 
live(), 
delegate吧。尤其是不要再用live()了，因为它已经处于不推荐使用列表了，随时会被干掉。如果只绑定一次事件，那接着用one()吧，这个没有变化。
jQuery是 一款优秀的JavaScript框架,在旧版里主要用bind()方法，在新版里又多了两种One(),Live()
>下面介绍这几种方法的使用：

#### 1. bind/Unbind
> 在jquery的事件模型中，有两个基本的事件绑 定函数，bind与unbind，这两个函数的含义就是匹配页面元素进行相关事件的处理。比如我们在JS中经常使用到的 onfocus，onblur，onmouseover，onmousedown等事件都可以作为bind的参数进行传递。
$("#id").bind('click',function(){alert('tt!')});
其中bind的第一个参数代表的含义是：事件类型(注意不需要加on)，function中的代码就是你要执行的逻辑 代码
多个事件绑定：bind还允许你绑定多个事件，事件名字之间用空格隔开，例如：
$('a').bind('click mouseover',function(){
在最新的jquery1.4版本中，对bind方法进行了改进，你可以在bind方法传入一个类JSON对象来一次绑定多 个事件处理函数。
```
$('a').bind({
	click:function(){
		alert('a');
	},
	mouseover:function(){
		alert('a again!')
	}
})
```
> 在function函数中，你还可以通过传递一个javaScript对 象给function方法，这个事件对象通常是可以省略的。
bind中还有一个参数data， 该参数一般情况下很少使用，通常为了解决在同一个方法中处理同一个变量时有很好的处理。
```
var productname="Sports Shoes";
	$('#Area').bind('click',function(){
	alert(productname);
});
productname="necklace",
$('#Area').bind('click',function(){
	alert(productname);
});
```
> 由于变量productname被重新赋值，所以输出的消息都是”necklace”,这里不了解可以去查阅下关于JavaScript的变量作用域,要 解决这个问题就必须使用到data参数，
```
var productname="Sports Shoes";
$('#Area').bind('click',{pn:productname},function(){
	alert(event.data.pn);
});
productname="necklace",
$('#Area').bind('click',{pn:productname},function(){
	alert(event.data.pn);
});
```

#### 2. One
为每一个匹配元素的特定事件（像click）绑定一个一次性的事件处理函数。该方法与bind方法的参数一样，与bind方法的区别就是只对匹配元素的事 件处理执行一次，执行完之后，以后再也不会执行,当然重新发起web请求时它又会执行一次。
```
$('a').one('click',function(){
	alert('a');
})
```
> 单击页面上的a元素后，弹出消息，除非用户发起第二次请求，否则再次点击a元素不会弹出消息对话框。

#### 3. live
该方法主要是能处理动态添加的元素，给那些后添加的元素也一样绑定事件。
```
$('a').live('click,function(){
	alert('show message!');
})
```
> 然后如果我添加一个元素，
$('body').appnend('Another Element');
那么该元素也会被触发事件处理函数alert。
另外，jQuery还提供了一些绑定这些标准事件类型的简单方式，比如.click()用于简化.bind(‘click')。
一共有以下这些事件名称：blur, focus, focusin, focusout, load, resize, scroll, unload, click, dblclick, mousedown, mouseup, mousemove, mouseover, mouseout, mouseenter, mouseleave, change, select, submit, keydown, keypress, keyup, error 等。
下面看下jQuery中绑定事件bind() on() live() one()的异同
jQuery中绑定事件的四种方法，他们可以同时绑定一个或多个事件
```
bind()----------版本号小于3.0（在Jquery3.0中已经移除，相应unbind()也移除）
live()----------版本号小于1.7（在Jquery1.7中已经移除，相应die()也移除）
delegate()------版本号小于1.7（在Jquery1.7中已经移除）
on()------------版本号大于1.7（在Jquery1.7中添加，相应off()也添加）
```

> A：bind()事件的用法
```
<title>绑定事件</title>
 <script src="js/jQuery1.11.1.js" type="text/javascript"></script>
 <script>
  $(function () {
   $("p").bind({
    "mouseover": function () {
     $("p").css("background-color", "red");
    },
    "mouseout": function () {
     $("p").css("background-color", "");
    }
   });
  });
 </script>
</head>
<body>
 <p>what are you doing?</p>
</body>
</html>
```
>第一个最大的区别就是：bind()的事件绑定是只对当前页面选中的元素有效。如果你想对动态创建的元素bind()事件，是没有办法达到效果的。
在后面的动态生成DOM元素绑定事件就要使用on();