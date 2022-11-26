[toc]

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

| 条件运算符         | 说明                                   |
| ------------------ | -------------------------------------- |
| >                  | 大于                                   |
| >=                 | 大于等于                               |
| <                  | 小于                                   |
| <=                 | 小于等于                               |
| =                  | 等于                                   |
| != / <>            | 不等于                                 |
| between..and..     | 在某个范围内(包括边界)                 |
| in(..)，not in(..) | 在 in 之后的列表中的值，多选一         |
| like 占位符        | 模糊匹配(_匹配单个字符; %匹配多个字符) |
| is null            | 是 null                                |
| is not null        | 不是 null                              |

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

![execute_order](../Java/img/execute_order.png)

# 