---
title: MYSQL之索引
tags: 数据库
categories: MySQL
---
>索引的两种存储类型：B树(BTree)索引、哈希(Hash)索引  
BTree为系统默认索引

### 创建表时建立索引
```
create table (
	属性名 长度 是否为空,
	[UNIQUE | FULLTEXT | SPATIAL ]  INDEX|KEY [别名]( 属性名1 [(长度)] [ASC | DESC])
)
```

### 已建立的表中创建索引
```
create [UNIQUE | FULLTEXT | SPATIAL ]  INDEX [别名] on table_name( 属性名1 [(长度)] [ASC | DESC])
```

### 修改表结构添加索引
```
alter table table_name add [UNIQUE | FULLTEXT | SPATIAL ]  INDEX [别名] on table_name( 属性名1 [(长度)] [ASC | DESC])
```

### 删除索引
```
drop INDEX 属性名 on table_name
```

>1. []是可选项  
>2. [UNIQUE | FULLTEXT | SPATIAL ] 可选项，分别代表唯一性索引、全文索引、空间索引
>3. INDEX和KEY参数其中一个即可，用于指定字段索引
>4. 别名为可选项，创建的索引取新名称  
	别名的参数如下：  
	属性名1 必选项，指索引对应的字段名称，该字段必须预选被定义到表中  
	长度	可选项，索引的长度，必须是字符串类型才可以使用  
	ASC/DESC可选项，ASC表示升序排列，DESC表示降序排列  
>5. table_name 表名称

### 普通索引
>在任何数据类型的字段中创建索引

```
// 创建表的时候创建索引
// 创建了索引列 id
create table if not exists table_test(
	id int(11) not null auto_increment primary key,
	name varchar(30) not null,
	index(id)
);
// 已存在表中创建索引
create index info on table_test (id)
// 修改表结构添加索引
alter table table_test add index info (id)
// 查看表结构
show create table table_test;
```

### 唯一性索引
>使用UNIQUE参数设置唯一索引，该索引的值必须唯一  
主键是一种特殊的唯一索引

```
// 创建表的时候创建索引
// 创建唯一索引 别名为 name 索引字段为 id，索引存储排序为 asc
create table if not exists table_test(
	id int(11) not null auto_increment primary key,
	name varchar(30) not null,
	UNIQUE INDEX name (id asc)
);
// 已存在表中创建索引
create UNIQUE INDEX name on table_test (id asc)
// 修改表结构添加索引
alter table table_test add UNIQUE INDEX name (id asc)
// 查看表结构
show create table table_test;
```

### 全文索引
>使用FULLTEXT参数设置全文索引  
只有myisam存储引擎的数据表支持fulltext全文索引  
只能创建在数据类型为 char、varchar、text的字段上  
默认情况下，应用全文索引大小写不敏感，索引的列使用二进制排序后，可以执行大小写敏感的全文索引

```
// 创建表的时候创建索引
// 创建全文索引 别名为 name_info 索引字段为 name
create table if not exists table_test(
	id int(11) not null auto_increment primary key,
	name varchar(30) not null,
	FULLTEXT KEY name_info (name)
) engine=MyISAM;
// 已存在表中创建索引
create FULLTEXT INDEX name_info on table_test (name)
// 修改表结构添加索引
alter table table_test add FULLTEXT INDEX name_info (name)
// 查看表结构
show create table table_test;
```

### 单列索引
>只对应一个字段的索引

```
// 创建表的时候创建索引
// 创建单列索引 别名为 name_info 索引字段为 name，索引字段长度为20
// 数据表中的字段长度为30，而创建的索引的字段长度为20，这样做的目的是为了提高查询效率，优化查询速度
create table if not exists table_test(
	id int(11) not null auto_increment primary key,
	name varchar(30) not null,
	INDEX name_info (name(20))
);
// 已存在表中创建索引
create INDEX name_info on table_test (name(20))
// 修改表结构添加索引
alter table table_test add INDEX name_info (name(20))
// 查看表结构
show create table table_test;
```

### 多列索引
>表的多个字段上创建索引  
应用此索引，必须使用这些字段的第一个字段

```
// 创建表的时候创建索引
// 创建多列索引 别名为 info 索引字段为 name、sex
create table if not exists table_test(
	id int(11) not null auto_increment primary key,
	name varchar(30) not null,
	sex bit(1) not null,
	INDEX info (name,sex)
);
// 已存在表中创建索引
create INDEX info on table_test (name,sex)
// 修改表结构添加索引
alter table table_test add INDEX info (name,sex)
// 查看表结构
show create table table_test;
```

### 空间索引
>使用SPATIAL参数可以设置空间索引  
只能建立在数据类型为空间类型的字段上  
mysql只有MyISAM存储引擎支持空间检索，且索引的字段不能为空值  
空间类型：geometry、point、linestring、polygon

```
// 创建表的时候创建索引
// 创建空间索引 别名为 good_info 索引字段为 good
create table if not exists table_test(
	id int(11) not null auto_increment primary key,
	good geometry not null,
	SPATIAL INDEX good_info (good)
);
// 已存在表中创建索引
create SPATIAL INDEX good_info on table_test (good)
// 修改表结构添加索引
alter table table_test add SPATIAL INDEX good_info (good)
// 查看表结构
show create table table_test;
```