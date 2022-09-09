[toc]

# SQL通用语法

1. SQL语句可以单行或者多行，以“;”结束。
2. SQL语句可以使用空格/缩进来增强语句的可读性。
3. SQL语句不区分大小写。
4. 注释：
   1. 单行注释：-- / # (MySQL特有)
   2. 多行注释：/* */

# SQL数据类型



# SQL分类

| 分类 | 全称                       | 说明                                                     |
| ---- | -------------------------- | -------------------------------------------------------- |
| DDL  | Data Definition Language   | 数据定义语言。用来定义数据库对象(数据库、表、字段)。     |
| DML  | Data Manipulation Language | 数据操作语言。用来对数据库表中数据增删改。               |
| DQL  | Data Query Language        | 数据查询语言。原来查询数据库中表的记录。                 |
| DCL  | Data Control Language      | 数据控制语言。原来创建数据库用户、控制数据库的访问权限。 |

# DDL

## 数据库操作

### 查询

查询所有数据库

```mysql
show databases;
```

查询当前数据库

```mysql
select database();
```

### 创建

```mysql
create database [if not exists] db_name [default charset 字符集(eg: utf8mb4)] [collate 排序规则(asc / desc)];
```

### 使用

```mysql
use db_name;
```

### 删除

```mysql
drop database [if exists] db_name;
```

## 表操作

### 查询

查询当前数据库中所有的表

```mysql
show tables;
```

查询表的结构

```mysql
desc t_name;
```

查询表的建表语句

```mysql
show create table t_name;
```

### 创建

```mysql
create table t_name(
	field1 field_type [comment f_desc],
    ...
    fieldn field_type [comment f_desc]
)[comment table_desc];
```

### 修改

添加字段

```mysql
alter table t_name add field type(length) [comment f_desc] [约束];
```

修改字段类型

```mysql
alter table t_name modify field new_type(length);
```

修改字段名和字段类型

```mysql
alter table t_name change old_field new_field type(length) [comment f_desc] [约束];
```

修改表名

```mysql
alter table t_name rename to new_name;
```

### 删除

删除字段

```mysql
alter table t_name drop field;
```

删除表

```mysql
drop table [if exists] t_name;
```

删除指定表，并重新创建该表

```mysql
truncate table t_name;
```

# DML

### 添加数据

给所有字段添加数据

```mysql
insert into t_name values(value_list);
```

给指定字段添加数据

```mysql
insert into t_name(field_list) values(value_list);
```

批量添加数据

```mysql
# 批量给所有字段添加数据
insert into t_name values(value_list), (value_list)...;
# 批量给指定字段添加数据
insert into t_name(field_list) values(value_list), (value_list)...;
```

### 修改数据

```mysql
update t_name set field1=value1, field2=value2... [where 条件];
```

### 删除数据

```mysql
delete from t_name [where 条件];
```

# DQL

### 查询数据

```mysql
select field_list
	from t_name
	where 条件列表
	group by 分组字段列表
	having 分组后条件列表
	order by 排序字段列表
	limit 分页参数
```

### 基本查询

查询多个字段

```mysql
select field_list from t_name;
# 查询所有字段
select * from t_name;
```

设置别名

```mysql
select field1 [[as] name1]... from t_name;
```

去除重复记录

```mysql
select distinct field_list from t_name;
```

### 条件查询

```mysql
select field_list from t_name where 条件;
```

#### 条件



### 聚合函数

> ​	指的是将一列数据作为一个整体，进行纵向计算。

count, max, min, avg, sum

```mysql
select count(name) from emp;
```

### 分组查询

```mysql
select count(id), workaddress from emp where age < 20 group by workaddress
```

### 排序查询



### 分页查询



# DCL

