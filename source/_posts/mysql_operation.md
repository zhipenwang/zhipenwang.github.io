---
title: MYSQL之操作数据表
date: 2018-07-07 17:21:01
tags: 数据库
categories: MySQL
---
> 文中均使用 `table_test` 作为表名

### 创建数据表
```
create table if not exists table_test(
    id int(11) auto_increment primary key,
    name varchar(30) not null comment '名称',
    password varchar(30) not null comment '密码'
) comment="表名";
```

### 查看表结构
```
show columns from table_test;
或者
describe table_test;
或者
desc table_test;
```

### 修改表结构
#### 1. 添加新字段及修改存在字段
```
alter table table_test
    add sex enum('male', 'female') not null default 'male' comment '性别',
    modify name varchar(40);
```
#### 2. 修改表字段名
```
alter table table_test
    change column date create_time date null default '0000-00-00';
```
#### 3. 删除字段
```
alter table table_test 
    drop create_time;
```
#### 4. 修改表名
```
alter table table_test 
    rename as table_test_new;
```

### 重命名表
```
rename table table_test_new to table_test;
```

### 复制表
#### 1. 复制表结构
```
create table table_test_copy1 like table_test;
```
#### 2. 复制表结构及数据
```
create table table_test_copy2 as select * from table_test;
```
#### 3. 复制部分表结构及数据
>只复制了`id`,`name`两个字段的表结构
```
create table table_test_copy3 as select id,name from table_test'
```

### 删除表
```
drop table if not exists table_test;
```