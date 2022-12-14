[toc]

# SQL 优化

## 添加数据

### insert 优化

#### 1) 批量插入数据

> ​	每次执行 `insert` 语句，都需要与数据库建立连接，进行网络传输(JDBC)。
>
> ​	使用*批量插入*在一次连接中插入多条数据，减少数据库的连接资源消耗。

```mysql
insert into t_name values(field_list), (field_list)...;
```

#### 2) 手动控制事务

> ​	MySQL 的事务是自动提交的。每次执行数据库语句，都要涉及事务的开启与关闭。
>
> ​	使用*手动开启事务*在一次事务中，完成多条啥数据库语句的执行。

```mysql
start transaction;
insert into t_name values(field_list), (field_list)...;
insert into t_name values(field_list), (field_list)...;
...
commit;
```

#### 3) 主键顺序插入

> ​	根据 MySQL 的组织结构，主键顺序插入性能要高于乱序插入。

### 大批量插入数据

> ​	如果一次性需要插入大批量数据(比如: 几百万的记录)，使用 `insert` 语句插入性能较低，此时可以使用 MySQL 数据库提供的 `load` 指令进行插入。

#### load 指令

```mysql
-- 客户端连接服务端时，加上参数 -–local-infile
mysql –-local-infile -u root -p

-- 设置全局参数local_infile为1，开启从本地加载文件导入数据的开关
set global local_infile = 1;

-- 执行load指令将准备好的数据，加载到表结构中
load data local infile 'sql_addr' into table t_name fields terminated by ',' lines terminated by '\n' ;
```

## 主键优化

### 数据组织方式

> ​	在 InnoDB 存储引擎中，表数据都是根据主键顺序组织存放的，这种存储方式的表称为索引组织表(index organized table IOT)。
>
> ​	数据行是记录在逻辑结构 page 页中的，而每一个页的大小是固定的，默认16K。
>
> ​	那也就意味着 一个页中所存储的行也是有限的，如果插入的数据行row在该页存储不小，将会存储到下一个页中，页与页之间会通过指针连接。

### 页分裂

> ​	页可以为空，也可以填充一半，也可以填充100%。每个页包含了 2 - N 行数据(如果一行数据过大，会行溢出)，根据主键排列。
>
> ​	当进行插入数据时，原本插入位置存储不了该数据时，这页即会发生页分裂。页分裂是一个比较耗费性能的操作。

### 页合并

> ​	当页进行了删除，达到了阈值，即会发现页合并。（默认为页的 50%）
>
> MERGE_THRESHOLD：
> 	页合并的阈值，可以在创建表或者创建索引时指定。

### 索引设计原则:

- 满足业务需求的情况下，尽量降低主键的长度。

- 插入数据时，尽量选择顺序插入，选择使用 AUTO_INCREMENT 自增主键。
- 尽量不要使用 UUID 做主键或者是其他自然主键，如身份证号。
- 业务操作时，避免对主键的修改。

## order by 优化

> MySQL 的排序，有两种方式：
>
> ​	Using Filesort: 通过表的索引或全表扫描，读取满足条件的数据行，然后在排序缓冲区 Sort Buffer 中完成排序操作，所有不是通过索引直接返回排序结果的排序都叫 Filesort 排序。
>
> ​	Using Index: 通过有序索引顺序扫描直接返回有序数据，这种情况即为 Using Index，不需要额外排序，操作效率高。
>
> 对于以上的两种排序方式，Using Index 的性能高，而 Using Filesort 的性能低，我们在优化排序操作时，尽量要优化为 Using Index。

### order by 优化原则:

- 根据排序字段建立合适的索引。多字段排序时，也遵循[最左前缀法则](索引.md)。

- 尽量使用[覆盖索引](./索引.md/##索引的使用)。

- 多字段排序, 一个升序一个降序时，此时需要注意联合索引在创建时的规则(ASC / DESC)。

- 如果不可避免的出现 Filesort，大数据量排序时，可以适当增大排序缓冲区大小(sort_buffer_size。默认 256k)。 

## group by 优化

> ​	对于分组操作，在联合索引中，也符合[最左前缀法则](索引.md)。

### group by 优化原则：

- 分组操作时，可以通过索引来提高效率。

- 分组操作时，索引的使用也是满足[最左前缀法则](索引.md)的。

## limit 优化

> ​	在数据量比较大时，如果进行 limit 分页查询，在查询时，越往后，分页查询效率越低。

### limit 优化思路: 

- 一般分页查询时，通过创建[覆盖索引](索引.md)能够比较好地提高性能，可以通过[覆盖索引](索引.md)加[子查询](多表查询.md)形式进行优化。

## count 优化

> ​	`select count(*) from tb_user;`
>
> ​	当数据量很大，执行 `count` 操作时，非常耗时。

### count 优化思路：

- 自己计数(可以借助于 Redis 这样的数据库进行，但如果是带条件的 count 又比较麻烦)。

### count 的语法

> ​	count() 是一个聚合函数，对于返回的结果集，一行行地判断，如果 count 函数的参数不是 NULL，累计值就加 1，否则不加，最后返回累计值。
>
> ​	按照效率排序的话，count(字段) < count(主键 id) < count(1) ≈ count(\*)，所以尽量使用 count(*)。

| count 用法  | 含义                                                         |
| ----------- | ------------------------------------------------------------ |
| count(主键) | InnoDB 引擎会遍历整张表，把每一行的 主键id 值都取出来，返回给服务层。服务层拿到主键后，直接按行进行累加(主键不可能为null)。 |
| count(字段) | 没有 not null 约束 : InnoDB 引擎会遍历整张表把每一行的字段值都取出来，返回给服务层。服务层判断是否为 null，不为 null，计数累加。<br />有not null 约束：InnoDB 引擎会遍历整张表把每一行的字段值都取出来，返回给服务层，直接按行进行累加。 |
| count(数字) | InnoDB 引擎遍历整张表，但不取值。服务层对于返回的每一行，放一个数字“1”进去，直接按行进行累加。 |
| count(*)    | InnoDB引擎并不会把全部字段取出来，而是专门做了优化，不取值，服务层直接按行进行累加。 |

## update 优化

我们主要需要注意一下 update 语句执行时的注意事项。

```mysql
update course set name = 'javaEE' where id = 1 ; 
```

当我们在执行修改的 SQL 语句时，会锁定id为 1 这一行的数据，然后事务提交之后，行锁释放。

但是当我们在执行如下 SQL 时。

```mysql
update course set name = 'SpringBoot' where name = 'PHP' ; 
```

当我们开启多个事务，在执行上述的SQL时，我们发现行锁升级为了表锁。导致该update语句的性能大大降低。

InnoDB 的行锁是针对索引加的锁，不是针对记录加的锁 ,并且该索引不能失效，否则会从**行锁**升级为**表锁**。