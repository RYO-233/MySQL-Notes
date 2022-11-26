[toc]

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

```mysql
revoke 权限列表 on db_name.t_name from 'username'@'hostname';
```

