# DQL - Data Query Language 

## key words: select

语法：

select

​			字段列表

from

​			表名列表

where

​			条件列表

group by

​			分组字段列表

having

​			分组后条件列表

limit

​			分页参数



### select

```sql
select column_name1, column_name2... from tablename;
select * from tablename;
#设置别名
select column_name1 [as name]... from tablename;
#去除重复记录
select distinct column_name from tablename;
#example
create table emp(
    id int,
    work_no varchar(10),
    name varchar(10),
    gender char(1),
    age tinyint unsigned,
    id_card char(18),
    work_address varchar(50),
    entry_date date
) comment '员工表';

insert into emp(id, work_no, name, gender, age, id_card, work_address, entry_date)
values (1, '1', 'A', 'F', 20, '123456789012345678', 'Madison', '2001-01-01'),
       (2, '2', 'B', 'F', 21, '123456789012345677', 'NewYork', '2001-01-02'),
       (3, '3', 'C', 'M', 23, '123456719012345678', 'Boston', '2002-01-01'),
       (4, '4', 'D', 'M', 26, '121456789012345678', 'LA', '2011-01-01'),
       (1, '1', null, 'F', 20, '123456789012345678', null, '2001-01-01');
select name, work_no, age from emp;

select * from emp;
select work_address '工作地址' from emp;
select distinct entry_date from emp; # 去重复

```

## condition

```sql
select column_name from tablename where condition;
/*
>
>=
<=
!= or <>
between...and...
in(...) 在in之后列表中的值，多选一
like(_one char, % any number of char)
is null
and, &&
or, ||
not, !
	*/
select * from emp where age=20;
select * from emp where age<23;
select * from emp where name is not null;
select * from emp where age >= 15 && age <=21;
select * from emp where age between 15 and 21;
select * from emp where gender='F' and age < 25;
select * from emp where age in (18, 20, 40);
select * from emp where emp.work_address like '__';
select * from emp where id_card like '%8';
```

### 聚合函数

| name  | function |
| ----- | -------- |
| count | 统计数量 |
| max   | 最s大值  |
| min   | 最小值   |
| avg   | 平均值   |
| sum   | 求和     |

```sql
select function(column_name) from tablename;
select count(name) from emp;
select avg(age) from emp;
select max(age) from emp;
select min(age) from emp;
select sum(age) from emp where entry_date like '2001-01-__';

```

### 分组查询

```sql
select column_name from tablename where condition group by column_name having condition;

```

where vs having

执行时机不同，where 是分组之前进行过滤， 不满足where条件，不参与分组，而having是分组之后对结果进行过滤

判断条件不同，where不能对聚合函数进行判断，而having可以

```sql
# group by gender, calculate the num of man and woman
select gender, count(*) from emp group by gender;
#group by gender, calculate the avg age of man and woman
select gender, avg(age) from emp group by gender;
#query age smaller than 45, then group by work address, get the employee num greater than 3
select address, count(*) from emp where age<45 group by address having count(*) >= 3;

```

执行顺序：where > function > having

```sql
select column_name from tablename order by column_name1, column_name2;
#asc: ascending
#desc descending
#example1: 根据年龄对公司的员工进行升序排序
select name from tablename order by age asc;
#example2: sort by descending age, when age same, order by entry time descending
select * from tablename order by age asc, entry_time desc;


```

## 分页查询

```sql
select column_name from tablename limit start, end;
#起始索引从0开始，起始索引=（查询页码-1） * 每页显示记录数
#分页查询不同数据库不同实现，mysql是limit
#如果查询的是第一页数据，起始索引可以省略，直接简写为limit 10
#example query second page data, every page show 10 info
select * from emp limit 10, 10;
```

### practice

```sql
#q1: quary age = 20,21,22,23 female 
select * from emp where age in (20,21,22,23) and gender='F';
#q2: quary gender is male age between 20 - 40, name char = 3
select * from emp where gender = 'M' and age between 20 and 40 and name like '___';
#q3: quary num of employer whose age smaller than 60
select gender, count(*) from emp where age < 60 group by gender;
#q4: quary all age <= 35 name and age, and ascending by age, if age same descending by entry time
select name, age from emp where age <= 35, order by age asc, entry_time by desc;
#q5: quary gender is male, age between 20 and 40, the top 5 people, ascending by age, ascending by entry time if the age is same
select * from emp where gender = 'male' and age between 20 and 40 order by age, entry, limit 0, 5;


```

### DQL 执行顺序

1. from
2. where
3. group by
4. select
5. order by
6. limit