---
title: MYSQL之操作数据（增删改查）
date: 2018-07-07 18:21:01
tags: 数据库
categories: MySQL
---
### 数据表结构
```
$sql = `
	create table if not exists table_test(
		id int(11) not null auto_increment primary key,
		name varchar(30) not null,
		sex tinyint(1) null
	)
`;
```

### 插入操作
```
// insert value 插入
$sql = "insert into table_test values(1, 'name1', 1)";
// 插入部分字段
$sql = "insert into table_test (name,sex) values('name2', 2)";
// 插入多条语句
$sql = "insert into table_test (name,sex) values('name1', 1), ('name2', 2)";
// insert set 插入
$sql = "insert into table_test set name='name1',sex=1";
// insert select插入
$sql = "insert into table_test (name,sex) select name,sex from table_test where id=1";
```

###  修改操作
```
// update set修改
$sql = "update table_test set name='name2' where id=1";
```

### 删除操作
```
// delete where删除
$sql = "delete from table_test where id=1";
// truncate table删除表所有行数据,且auto_increment 重新计数
$sql = "truncate table table_test";
```

### 查询操作
> 条件顺序如下： group by ... having ... order by ... limit ...

```
// 普通查询
$sql = "select * from table_test";
// in 查询
$sql = "select * from table_test where name in ('name1', 'name2')";
// distinct 查询某个字段去重(多字段表示多字段合并的去重 distinct name,sex)
$sql = "select distinct name from table_test";
// group_concat(field1) group by field2(按field2分组后，field1的值用，隔开)
$sql = "select group_concat(money),order_id from table_test group by order_id";
```

### 聚合查询
> count(*) 查询结果总数集  
sum(field) 计算字段求和总值  
avg(field) 计算字段平均值  
max(field) 计算字段最大值
min(field) 计算字段最小值