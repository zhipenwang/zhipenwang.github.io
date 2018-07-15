---
title: MYSQL之运算符操作
tags: 数据库
categories: MySQL
---
```
create table if not exists test(
	id int(11) not null auto_increment primary key,
	num int(11) null
)
```
### 算术运算符
>加(+)、减(-)、乘(*)、除(/)、余(%)、除(div)、余(mod)

```
select id, num, id+num, id-num, id*num, id/num, id%num, id div num, id mod num from test;
结果：
id 	num 	id+num 	id-num 	id*num 	id/num 	id%num 	id div num 	id mod num
5  	2   	8	  	3      	10     	2.5    	1      	2			1
6   null	null	null	null	null	null	null		null
null 5		null	null	null	null	null	null		null
```

### 比较运算符
>大于(>)、小于(<)、等于(=)、不等于(!=或<>)、大于等于(>=)、小于等于(<=)

```
1. 符合条件的结果为1
2. 不符合条件的结果为0
3. 值为NULL的结果为null
```  

>是null(IS NULL)、不是null(IS NOT NULL)

```
判断是否为空值可以采用 <=> 进行判断，a<=>0，值为0的结果为1，其他都为0；a<=>null，值为null的结果为1，其他都为0
```  

>包含(between a and b)  
>包含(id IN (1,2,3))、不包含(id not in(1,2,3))  
>模式匹配(name like '%keyword%')、模式不匹配(name not like '%keyword%')  

```
1. like 'abc'  表示字符串=abc的匹配
2. like '%abc' 表示以abc结尾的字符串匹配
3. like 'abc%' 表示以abc开头的字符串匹配
4. like '%abc%'表示包含abc的字符串匹配

```

>正则匹配(a regexp '^a')

```
1. a regexp '^abc' 表示以abc开头的字符串匹配
2. a regexp 'abc'  表示包含abc的字符串匹配
3. a regexp 'abc$' 表示以abc结尾的字符串匹配
```

### 逻辑运算符
>与(&&或AND)、或(||或OR)、非(!或NOT)、异或(XOR)

```
1. &&  只要有一个值为0，结果为0；有值为null，其他都不为0，结果为null；值都不为null与0的结果为1；
2. ||  值都为0结果为0；值为0或者null结果为null；值存在不为0或者null的结果为1；
3. !   值为null结果为null；值为0的结果为1；值为非0或null的结果为1；
4. XOR 值存在null的结果为null；值都是非0或者都是0的结果为0；值存在0跟非0的结果为1；
```

### 位运算符
>按位与(&)、按位或(|)、按位取反(~)、按位异或(^)、按位左移(<<)、按位右移(>>)

```
1. &	将十进制数转换为二进制数，每个二进制数对应的位上进行与运算，最后转换为十进制数
	例：10&5 => 1010&0101 => 0000 => 0
	例：10&6 => 1010&0110 => 0010 => 2
2. |	将十进制数转换为二进制数，每个二进制数对应的位上进行或运算，最后转换为十进制数
	例：10&5 => 1010|0101 => 1111 => 15
	例：10&6 => 1010|0110 => 1110 => 14
3. ~	将十进制数转换为二进制数，每位都进行取反运算
	例：10 => 1010 => 0101 => 18446744073709551605(字节计算结果)
4. ^	将十进制数转换为二进制数，每个二进制数对应的位上进行异或运算，最后转换为十进制数
	例：10&5 => 1010^0101 => 1111 => 15
	例：10&6 => 1010^0110 => 1100 => 12
5. <<	m << n 将十进制数m转换为二进制数，按位左移n位，右边补上n个0，最后转换为十进制数
	例：10 << 1 => 1010 => 10100 => 20
5. >>	m >> n 将十进制数m转换为二进制数，按位右移n位，左边补上n个0，最后转换为十进制数
	例：10 >> 1 => 1010 => 0101 => 5
```

### 运算符优先级
![image](https://wenku.baidu.com/content/216d3e2926284b73f242336c1eb91a37f11132c9?m=26e2fd787393eaa2973d6c1f478627b5&type=pic&src=04452f9619b2848d6d760be569e87603.jpg)