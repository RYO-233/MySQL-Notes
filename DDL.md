[TOC]

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
alter table t_name add f_name type(length) [comment f_desc] [约束];
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

添加列

```mysql
alter table 表名 add column 列名 类型 [列约束];
```

修改列

```mysql
alter table 表名 modify column 列名 新类型 [约束];
# modify不能修改列名，change可以修改列名
alter table 表名 change column 列名 新列名 新类型 [约束];
```

删除列

```mysql
alter table 表名 drop column 列名;
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

### 复制

复制表结构

```mysql
create table t_name like r_t_name;
```

复制表结构+数据

```mysql
create table t_name [as] select 字段,... from r_t_name [where 条件];
```
