# mysql 数据库

1. 关系型数据库 （RDBMS）

   特点：

   1. 使用表存储，格式统一，便于维护
   2. 使用SQL语言操作

2. MYSQL start/stop

​	net start mysql80

​	net stop mysql80



1. comment: 
   1. one line -- or # 
   2. multiple line: /* info */

| type | full name                  | comment                        |
| ---- | -------------------------- | ------------------------------ |
| DDL  | Data Definition Language   | 数据定义语言，用来定义数据对象 |
| DML  | Data Manipulation Language | 数据操作                       |
| DQL  | Data Query Language        | 数据查询语言                   |
| DCL  | Data Control Language      | 数据控制语言                   |

#### DDL

1. 查询

```sql
# 查询所有数据库
SHOW DATABASES; 
#查询当前数据库
SELECT DATABASE();
#创建
CREATE DATABASE [IF NOT EXISTS] NAME [DEFAULT CHARSET][COLLATE]
create database itheima default charset utf8mb4
#删除
DROP DATABASE[IF EXISTS] NAME
#查询
show tables;
#查询表结构
desc tablename;
#查询定表的建表语句
show create table tablename;
```

2. 创建

   ```sql
   create table tablename(
   	name1: name1 type comment "", # int, varchar
   	name2: name2 type
   )comment "";
   ```

![image-20221024154608197](C:\Users\zzhan\AppData\Roaming\Typora\typora-user-images\image-20221024154608197.png)



## 数据类型

tinyint

smallint

mediumint

int, integer

bigint

float

double

decimal

decimal: depend on M and D 精度/标度

```sql
age tinyint unsigned
score double(4, 1) # 4 = total length, 1 = 1 th accurate
```

#### 字符串类型

char :           char(10)  ----------------------------------------------> 性能高

varchar:      varchar(10) ------------------------------------------->性能较差

tinyblob

tinytext

blob

text

mediumblob

mediumtext

longblob 

longtext



username varchar(50)

gender char(1)

#### 时间类型

date    yyyy-mm-dd

time   hh:mm:ss

year    yyyy

datetime yyyy-mm-dd hh:mm:ss

timestamp

ex: birthday use date

```sql
create table emp(
     id int,
     workNo varchar(10),
     name varchar(10) comment "name",
     gender char(1) comment "gender",
     age tinyint unsigned,
     idcard char(18) comment "身份证",
     entrydate date comment "入职时间"
);
```

### DDL-表操作-修改

```sql
#添加字段
alter table tablename add name type comment "";

#修改数据类型
alter table tablename modify name newtype(length)
#修改字段名和类型
alter table tablename change oldname newname type(length);

#example
#change nickname to username, and change the length of varchar to 30
alter table emp change nickname username varchar(30) comment "usernmae";
#删除 字段
alter table tablename drop name;
```

![image-20221024161605519](C:\Users\zzhan\AppData\Roaming\Typora\typora-user-images\image-20221024161605519.png)



修改表名

```sql
alter table tablename rename to new_tablename;
```



删除表

```sql
# 删除表
drop table if exists tablename;
#删除指定表，并重新创建该表
truncate table tablename;
```

#### summary

1. DDL - 数据库操作：

   	1. show databases;

   2. create database

   3. use 

   4. select database()

2. DDL -  table

   1. show tables;
   2. create table;
   3. desc tablename;
   4. alter table tablename add/modify/change/drop/rename;
   5. drop table tablename;

   

