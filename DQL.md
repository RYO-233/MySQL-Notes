[toc]

# DQL

## 基本查询

### 查询多个字段

```mysql
select field_list from t_name;

# 查询所有字段
select * from t_name;
```

### 设置别名

```mysql
# 一旦为表起了别名，就不能再使用表名来指定对应的字段了
# 此时只能够使用别名来指定字段。
select field1 [[as] name1]... from t_name;
```

### 去除重复记录

```mysql
select distinct field_list from t_name;
```

## 条件查询

```mysql
select field_list from t_name where 条件;
```

### 条件运算符

| 条件运算符         | 说明                                                         |
| ------------------ | ------------------------------------------------------------ |
| >                  | 大于                                                         |
| >=                 | 大于等于                                                     |
| <                  | 小于                                                         |
| <=                 | 小于等于                                                     |
| =                  | 等于                                                         |
| != / <>            | 不等于                                                       |
| <=>                | 即可判断 null，也可判断普通数值。<br />可读性较低，较少使用。 |
| between..and..     | 在某个范围内(闭区间)                                         |
| in(..)，not in(..) | 在 in 之后的列表中的值 / not in 列表中的值，多选一           |
| like 占位符        | 模糊匹配(_ 匹配单个字符; % 匹配多个字符)                     |
| is null            | 是 null                                                      |
| is not null        | 不是 null                                                    |

### 逻辑运算符

| 逻辑运算符 | 说明                 |
| ---------- | -------------------- |
| and / &&   | 且(多个条件同时成立) |
| or / \|\|  | 或(任意一个条件成立) |
| not / !    | 非                   |

## 聚合函数

```mysql
select count(name) from emp;
```

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

## 分组查询

```mysql
select field_list from t_name [where 条件] group by 分组字段列表 [having 分组后过滤条件];
```

> 分组中，select后面只能有两种类型的列：
>
> 1. 出现在 group by 后的列
> 2. 或者使用聚合函数的列

### where 与 having 的区别

- 执行时机不同：where 是分组之前进行过滤，不满足 where 条件，不参与分组；而 having 是分组之后对结果进行过滤。
- 判断条件不同：where 不能对聚合函数进行判断，而 having 可以。

### 注意事项

> • 分组之后，查询的字段一般为聚合函数和分组字段，查询其他字段无任何意义。
>
> • 执行顺序: where > 聚合函数 > having 。
>
> • 支持多字段分组, 具体语法为 : group by columnA,columnB

## 排序查询

```mysql
select field_list from t_name order by f_name1 (asc / desc), f_name2 (asc / desc)...;
```

> 需要排序的字段跟在`order by`之后；
>
> asc|desc表示排序的规则，asc：升序，desc：降序，默认为asc；
>
> 支持多个字段进行排序，多字段排序之间用逗号隔开。

## 分页查询

```mysql
select field_list from t_name limit [start_index,] count;
```

> __说明:__
>
> - start_index: 表示偏移量。通俗点讲就是跳过多少行，start_index 可以省略。默认为 0，表示跳过 0 行。范围：[0, +∞)。
> - count：跳过 start_index 行之后，开始取数据，取 count 行记录。范围：[0, +∞)。
> - __<span style="color: red">limit 中 start_index 和 count 的值不能用表达式。</span>__
>
> - 起始索引从 0 开始，起始索引 = （查询页码 - 1）* 每页显示记录数。
> - 分页查询是数据库的方言，不同的数据库有不同的实现，MySQL 中是LIMIT。
> - 如果查询的是第一页数据，起始索引可以省略，直接简写为 limit 10。
> - 分页排序时，不要有*二义性*。*二义性*可能会导致分页结果乱序。可以追加一个主键排序。

### 注意事项

> 开发过程中，分页我们经常使用，分页一般有2个参数:
>
> page: 表示第几页。从1开始，范围 [1, +∞)
>
> pageSize: 每页显示多少条记录。范围 [1, +∞)
>
> 如: page = 2，pageSize = 10，表示获取第 2 页的 10 条数据。
>
> 我们使用 limit 实现分页，语法如下：

```mysql
select f_list from t_name limit (page - 1) * pageSize, pageSize;
```

## SQL 查询顺序

```mysql
select field_list
	from t_name
	where 条件列表
	group by 分组字段列表
	having 分组后条件列表
	order by 排序字段列表
	limit [start_index,] count;
```

> __说明: __
>
> ​	`where` 后面接一个 / 多个条件，条件是对前面数据的过滤。只有满足 `where` 后面条件的数据，才会被返回。

## SQL 执行顺序

![execute_order](./img/execute_order.png)
