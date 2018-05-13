---
title: js 比较几种遍历数组及对象的方式
tags: Web前端
categories: jQuery
---
>通常我们会用循环的方式来遍历数组。但是循环是 导致js 性能问题的原因之一。
>一般我们会采用下几种方式来进行数组的遍历：

### 方式1：for in 循环
```
var arr = [1,2,3,4,5];
var obj = { a : 1, b : 2, c : 3 };
for( var item in arr|obj ){
        fn(item){
                // do sth with arr[item];
                //do sth wtih obj[item];
        };
}
这里的 item：
        array 的索引值，对应于 arr 的下标值；
        object 的 key 值，对应于 obj 的 a,b,c；

```

### 方式2：for 循环
```
for (var i=0; i<arr.length; i++){
        //do sth with arr[i];
}
```

> 这两种方法应该非常常见且使用很频繁。但实际上，这两种方法都存在性能问题。
> 在方式1中，for-in 需要分析出array 的每个属性，这个操作性能开销很大。用在 key 已知的数组上是非常不划算的。所以尽量不要用 for-in，除非你不清楚要处理哪些属性，例如 JSON 对象这样的情况。
> 在方式2中，循环每进行一次，就要检查一下数组长度。读取属性（数组长度）要比读局部变量慢，尤其是当 array 里存放的都是 DOM 元素，因为每次读取都会扫描一遍页面上的选择器相关元素，速度会大大降低。
所以这时候我们就有必要对方式2进行优化。

#### 加速的
```
var arr = [1,2,3,4,5];
var length =arr.length;
for(var i=0; i<length; i++){
　　fn(arr[i]);
}
```
> 现在只需要读取一次 array 的 length 属性，速度已经加快了。但是还能不能更快呢？
事实是，如果循环终止条件不进行比较运算，那么循环的速度还可以更快。

#### 加速且优雅的
```
var arr = [1,2,3,4,5];
var i = arr.length;
while(i--){
        fn(arr[i]);
}
```

### 方式 3：forEach
```
var arr = [1,2,3,4,5];
arr.forEach(
        fn(value,index){
                //Do sth with value ;
        }
)
```
> 注意：
> 这里的 forEach回调中两个参数分别为 value，index，其位置刚好和 jQuery 的$.each 相反；
forEach 无法遍历对象；
IE不支持该方法；Firefox 和 chrome 支持；
forEach 无法使用 break，continue 跳出循环，且使用 return 是跳过本次循环；
可以添加第二个参数，为一个数组，回调中的 this 会指向这个数组，若没有添加，则是指向 window；

关于跳出循环的几种方式：
1. return ==》结束循环并中断函数执行；
2. break ==》结束循环函数继续执行；
3. continue ==》跳过本次循环；

> for 循环中的变量 i，由于 ES5并没有块级作用域的存在，它在循环结束以后仍然存在于内存中，所以建议使用函数自执行的方式来避免。
