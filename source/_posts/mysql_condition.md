---
title: MYSQL之流程控制语句
tags: 数据库
categories: MySQL
---
>if、case、loop、while、iterate、leave

### if
> 注意 分号不能省略

```
IF CONDITION THEN
	...;
ELSEIF CONDITION THEN
	...;
ELSE
	...;
END IF;
```

```
调用：call do_if(10)

CREATE PROCEDURE do_if(in x int)
BEGIN
IF x IS NULL THEN
	select 100;
ELSEIF x=0 THEN
	select 101;
ELSE
	select 102;
END IF;
END
```

### case
> 注意 分号不能省略

```
CASE value
WHEN value THEN ...;
WHEN value THEN ...;
ELSE ...;
END CASE;
```

```
调用：call do_case(10)

CREATE PROCEDURE do_case(in x int)
BEGIN
CASE x
WHEN x IS NULL THEN SELECT 100;
WHEN x = 0 THEN SELECT 101;
ELSE SELECT 102;
END CASE;
END
```

### while
> 注意 分号不能省略

```
WHILE CONDITION DO
	...;
END WHILE;
```

```
调用：
	call do_while(@sum)
	select @sum

CREATE PROCEDURE do_while(out x int)
BEGIN
DECLARE i int DEFAULT 1;
DECLARE j int DEFAULT 0;
WHILE i<100 DO
	set j=j+i;
	set i=i+1;
END WHILE;
SET x=j;
END
```

### loop
> 注意 分号不能省略  
LEAVE loop_label 退出 定义名称为loop_label的loop退出循环

```
LOOP
	...
END LOOP;
```

```
调用：
	call do_loop(@sum)
	select @sum

CREATE PROCEDURE do_loop(out x int)
BEGIN
DECLARE i int DEFAULT 1;
DECLARE j int DEFAULT 0;
loop_label:LOOP
	set j=j+i;
	set i=i+1;
	IF i>10 THEN
		LEAVE loop_label;
	END IF;
END LOOP;
set x=j;
END
```

### repeat
> 注意 分号不能省略  

```
REPEAT
	...
UNTIL CONDITION
END REPEAT;
```

```
调用：
	call do_repeat(@sum)
	select @sum

CREATE PROCEDURE do_repeat(out x int)
BEGIN
DECLARE i int DEFAULT 1;
DECLARE j int DEFAULT 0;
REPEAT
set j=j+i;
set i=i+1;
UNTIL i>10
END REPEAT;
set x=j;
END
```