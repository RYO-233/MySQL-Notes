[toc]

# SQL 通用语法

1. SQL语句可以单行或者多行，以“;”结束。
2. SQL语句可以使用空格/缩进来增强语句的可读性。
3. SQL语句不区分大小写。
4. 注释：
    1. 单行注释：-- / # (MySQL特有)
    2. 多行注释：/* */

# SQL 数据类型

## 数值类型



## 字符串类型



## 时间日期类



# SQL 分类

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

# 一旦为表起了别名，就不能再使用表名来指定对应的字段了
# 此时只能够使用别名来指定字段。
```

去除重复记录

```mysql
select distinct field_list from t_name;
```

### 条件查询

```mysql
select field_list from t_name where 条件;
```

**条件：**

| 条件运算符     | 说明                                   |
| -------------- | -------------------------------------- |
| >              | 大于                                   |
| >=             | 大于等于                               |
| <              | 小于                                   |
| <=             | 小于等于                               |
| =              | 等于                                   |
| != / <>        | 不等于                                 |
| between..and.. | 在某个范围内(包括边界)                 |
| in(..)         | 在 in 之后的列表中的值，多选一         |
| like 占位符    | 模糊匹配(_匹配单个字符; %匹配多个字符) |
| is null        | 是 null                                |
| is not null    | 不是 null                              |

| 逻辑运算符 | 说明                 |
| ---------- | -------------------- |
| and / &&   | 且(多个条件同时成立) |
| or / \|\|  | 或(任意一个条件成立) |
| not / !    | 非                   |

### 聚合函数

> ​	指的是将一列数据作为一个整体，进行纵向计算。
>
> ​	==注意：null 值不参与聚合函数运算的。==

| 聚合函数 | 说明   |
| -------- | ------ |
| count    | 统计   |
| max      | 最大值 |
| min      | 最小值 |
| avg      | 平均值 |
| sum      | 求和   |

```mysql
select count(name) from emp;
```

### 分组查询

```mysql
select field_list from t_name [where 条件] group by 分组字段列表 [having 分组后过滤条件];
```

#### where 与 having 的区别

- 执行时机不同：where 是分组之前进行过滤，不满足 where 条件，不参与分组；而 having 是分组之后对结果进行过滤。
- 判断条件不同：where 不能对聚合函数进行判断，而 having 可以。

#### 注意事项

> • 分组之后，查询的字段一般为聚合函数和分组字段，查询其他字段无任何意义。
>
> • 执行顺序: where > 聚合函数 > having 。
>
> • 支持多字段分组, 具体语法为 : group by columnA,columnB

### 排序查询

```mysql
select field_list from t_name order by f_name1 (asc / desc), f_name2 (asc / desc)...;
```

### 分页查询

```mysql
select field_list from t_name limit start_index, record;
```

注意事项:

• 起始索引从0开始，起始索引 = （查询页码 - 1）* 每页显示记录数。

• 分页查询是数据库的方言，不同的数据库有不同的实现，MySQL中是LIMIT。

• 如果查询的是第一页数据，起始索引可以省略，直接简写为 limit 10。

### SQL 执行顺序

![execute_order](img/execute_order.png)

# DCL

## 用户管理

> ​	主机名可以使用 % 通配符。

### 查询用户

```mysql
use mysql;
select * from user;
```

### 创建用户

```mysql
create user 'username'@'hostname' identified by 'password';
```

### 修改密码

```mysql
alter user 'username'@'hostname' identified with mysql_native_password by 'new_password';
```

### 删除用户

```mysql
drop user 'username'@'hostname'
```

## 权限管理

### 权限分类

| 权限                | 说明                   |
| ------------------- | ---------------------- |
| all, all privileges | 所有权限               |
| select              | 查询数据               |
| insert              | 插入数据               |
| update              | 修改数据               |
| delete              | 删除数据               |
| alter               | 修改表                 |
| drop                | 删除数据库 / 表 / 视图 |
| create              | 创建数据库 / 表        |

### 查询权限

```mysql
show grants for 'username'@'hostname';
```

### 授予权限

```mysql
grant 权限列表 on db_name.t_name to 'username'@'hostname';
```

### 撤销权限

```
revoke 权限列表 on db_name.t_name from 'username'@'hostname';
```
