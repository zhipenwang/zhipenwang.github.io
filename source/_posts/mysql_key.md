---
title: MYSQL之数据表级联操作-数据完整性约束
date: 2018-07-15 20:21:01
tags: 数据库
categories: MySQL
---
### 外键约束
>目前mysql只有InnoDB存储引擎支持外键约束
>1. column_name 当前表外键字段名
>2. table_name 外键表
>3. index_column_name 外键所在外键表中字段名
>4. ON DELETE 删除  ON UPDATE 更新
>5. reference_option语法格式为：（没设置时候，默认两个都指定 RESTRICT）  
	当删除或更新外键所在外键表的数据时候   
	RESTRICT      限制策略，系统不删除或更新外键当前表的数据  
	CASCADE    	  级联策略，自动删除或更新外键当前表的数据  
	SET FULL 	  置空策略，设置外键当前表的数据外键列数据为NULL，需要提前设置外键列未被限制 NOT NULL  
	NO ACTION 	  不采取实施策略，系统不删除或更新外键当前表的数据，与 RESTRICT 一致


```
FOREIGN KEY ( column_name [(length)] [ASC | DESC] ) 
REFERENCES table_name(index_column_name)
[MATCH FULL | MATCH PARTIAL | MATCH SIMPLE]
[ON DELETE reference_option]
[ON UPDATE reference_option]
```

#### 创建表，关联外键
```
// 创建 班级表
create table if not exists class(
	id int(11) not null auto_increment,
	name varchar(30) not null,
	primary key (id)
) ENGINE=InnoDB default charset=utf8;

// 创建学生表，外键 班级id class_id，级联删除及更新
create table if not exists student(
	id int(11) not null auto_increment,
	name varchar(30) not null,
	class_id int(11) not null,
	primary key(id),
	index(class_id),
	foreign key (class_id) references class(id) on delete cascade on update cascade
) ENGINE=InnoDB default charset=utf8;
```


#### 插入数据操作
```
//插入数据
insert into class(name) values('one'), ('two'), ('three');
// 生成 id 1,2,3

insert into student(class_id,name) values(1, 'lili');
insert into student(class_id,name) values(2, 'lili');
insert into student(class_id,name) values(2, 'lili');
insert into student(class_id,name) values(3, 'lili');

// 插入失败，因为班级表中没有id =10的数据
insert into student(class_id,name) values(10, 'lili');
```

#### 更新数据操作
```
//更新数据
update class set id=5 where id=2;
//此时 学生表中 sid=2的数据同步更新为了5
```

#### 删除数据操作
```
//删除数据
delete from class where id=1;
//此时 学生表中 sid=1的数据同步删除了
```


### 主键
> 一个表中有且只有一个主键

```
// 单字段主键
create table if not exists table_test(
	id int(11) not null auto_increment primary key,
	name varchar(30) not null
) engine=InnoDB default charset=utf8;

// 复合字段主键
create table if not exists table_test(
	id int(11) not null auto_increment,
	name varchar(30) not null,
	primary key(id, name)
) engine=InnoDB default charset=utf8;
```

### 候选键
> 一个表中可以存在多个候选键  
UNIQUE 来表示

```
create table if not exists table_test(
	id int(11) not null auto_increment UNIQUE,
	name varchar(30) not null UNIQUE,
) engine=InnoDB default charset=utf8;
```

### 字段条件约束
```
// null
id int(11) not null,
sid int(11) null

// check对列约束
age int(2) not null check(age>6 and age<18)

// check对表约束
primary key(id),
check(class_id in (select id from tb_class))
```

### 完整性约束
```
// 创建表的时候使用
constraint <symbol>
[primary ... | foreign ... | check ...]

// 更新表的完整性约束
alter table table_name add constraint primary
primary key(id)

// 删除表的完整性约束
alter table table_name drop [foreign key|index] <symbol> [primary key]
```