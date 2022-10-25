# DML - data manipulation language

##  添加数据 (insert) 修改数据 (update) 修改数据 (delete)



### 1. insert

```sql
#给指定字段添加数据
insert into tablename (字段1， 字段2, ...) values (value1, value2);

#给全部字段添加数据
insert into tablename values (value1, value2, ...);
#批量添加数据
insert into tablename (column_name1, column_name2,...) values (value1, value2,...),(value1, value2,...)...;
insert into tablename values (value1, value2,...),(value1, value2,...);

```

​	插入数据，指定的字段顺序需要与值的顺序一一对应

​	字符串和日期类型应该包含在“”内

​	插入数据的大小，应该在字段的规定范围内

### 2. Change

```sql
Update tablename set column_name1 = value1, column_name2 = value2,...[where condition];
#example
update employee set name = 'itheima' where id = 1;
#change two values
update employee set name='fee', gender='F' where id = 1;
#change all values of one column
update employee set entrydate='2008-01-01';

```



### 3. Delete

```sql
delete from tablename [where condition];
#example
delete from employee where gender='F';
delete from employee; ## all values

```

