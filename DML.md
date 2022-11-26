[toc]

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

# 