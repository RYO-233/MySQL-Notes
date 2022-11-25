[toc]

# SQL 通用语法

1. SQL语句可以单行或者多行，以 “;” 结束。
2. SQL语句可以使用空格 / 缩进来增强语句的可读性。
3. SQL语句不区分大小写，建议关键字大写，表名、列名小写。
4. 注释：
    1. 单行注释：-- / # (MySQL特有)
    2. 多行注释：/* */

# SQL 数据类型

## 数值类型

| 类型         | 大小                                     | 范围（有符号）                                               | 范围（无符号）                                               | 用途            |
| :----------- | :--------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- | :-------------- |
| TINYINT      | 1 Bytes                                  | (-128，127)                                                  | (0，255)                                                     | 小整数值        |
| SMALLINT     | 2 Bytes                                  | (-32 768，32 767)                                            | (0，65 535)                                                  | 大整数值        |
| MEDIUMINT    | 3 Bytes                                  | (-8 388 608，8 388 607)                                      | (0，16 777 215)                                              | 大整数值        |
| INT或INTEGER | 4 Bytes                                  | (-2 147 483 648，2 147 483 647)                              | (0，4 294 967 295)                                           | 大整数值        |
| BIGINT       | 8 Bytes                                  | (-9,223,372,036,854,775,808，9 223 372 036 854 775 807)      | (0，18 446 744 073 709 551 615)                              | 极大整数值      |
| FLOAT        | 4 Bytes                                  | (-3.402 823 466 E+38，-1.175 494 351 E-38)，0，(1.175 494 351 E-38，3.402 823 466 351 E+38) | 0，(1.175 494 351 E-38，3.402 823 466 E+38)                  | 单精度 浮点数值 |
| DOUBLE       | 8 Bytes                                  | (-1.797 693 134 862 315 7 E+308，-2.225 073 858 507 201 4 E-308)，0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 双精度 浮点数值 |
| DECIMAL      | 对DECIMAL(M,D) ，如果M>D，为M+2否则为D+2 | 依赖于M和D的值                                               | 依赖于M和D的值                                               | 小数值          |

## 字符串类型

| 类型       | 大小                  | 用途                            |
| :--------- | :-------------------- | :------------------------------ |
| CHAR       | 0-255 bytes           | 定长字符串                      |
| VARCHAR    | 0-65535 bytes         | 变长字符串                      |
| TINYBLOB   | 0-255 bytes           | 不超过 255 个字符的二进制字符串 |
| TINYTEXT   | 0-255 bytes           | 短文本字符串                    |
| BLOB       | 0-65 535 bytes        | 二进制形式的长文本数据          |
| TEXT       | 0-65 535 bytes        | 长文本数据                      |
| MEDIUMBLOB | 0-16 777 215 bytes    | 二进制形式的中等长度文本数据    |
| MEDIUMTEXT | 0-16 777 215 bytes    | 中等长度文本数据                |
| LONGBLOB   | 0-4 294 967 295 bytes | 二进制形式的极大文本数据        |
| LONGTEXT   | 0-4 294 967 295 bytes | 极大文本数据                    |

## 时间日期类

| 类型      | 大小 ( bytes) | 范围                                                         | 格式                | 用途                     |
| :-------- | :------------ | :----------------------------------------------------------- | :------------------ | :----------------------- |
| DATE      | 3             | 1000-01-01/9999-12-31                                        | YYYY-MM-DD          | 日期值                   |
| TIME      | 3             | '-838:59:59'/'838:59:59'                                     | HH:MM:SS            | 时间值或持续时间         |
| YEAR      | 1             | 1901/2155                                                    | YYYY                | 年份值                   |
| DATETIME  | 8             | '1000-01-01 00:00:00' 到 '9999-12-31 23:59:59'               | YYYY-MM-DD hh:mm:ss | 混合日期和时间值         |
| TIMESTAMP | 4             | '1970-01-01 00:00:01' UTC 到 '2038-01-19 03:14:07' UTC结束时间是第 **2147483647** 秒，北京时间 **2038-1-19 11:14:07**，格林尼治时间 2038年1月19日 凌晨 03:14:07 | YYYY-MM-DD hh:mm:ss | 混合日期和时间值，时间戳 |

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

### 复制

复制表结构

```mysql
create table t_name like r_t_name;
```

复制表结构+数据

```mysql
create table t_name [as] select 字段,... from r_t_name [where 条件];
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

#### 说明

1. 主机名默认值为 %，表示这个用户可以从任何主机连接 MySQL 服务器。
2. 密码可以省略，表示无密码登录。

### 修改密码

```mysql
SET PASSWORD FOR '用户名'@'主机' = PASSWORD('密码');
```

```mysql
alter user 'username'@'hostname' identified with mysql_native_password by 'new_password';
```

```mysql
# 修改 mysql.user 表
use mysql;
update user set authentication_string = password('321') where user = 'test1' and host = '%';
flush privileges;
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
grant 权限列表 on db_name.t_name to 'username'@'hostname' [with grant option];
```

### 撤销权限

```
revoke 权限列表 on db_name.t_name from 'username'@'hostname';
```
