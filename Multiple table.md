# Multiple table

## onto(one to more)

## more to more



### more to one

```sql
create table student(
    id int auto_increment primary key comment 'zhujian id',
    name varchar(10),
    no varchar(10)
);
insert into student(id, name, no) VALUES (null, 'Jane', '2000100101'), (null, 'Peter', '2000100102'), (null, 'Frank', '2000100103'), (null, 'Alice', '2000100104'),
                                         (null, 'Adam', '2000100105');
create table course(
    id int auto_increment primary key,
    name varchar(10)
);
insert into course values (null, 'Java'), (null, 'PHP'), (null,'MySQL'),(null,'Hadoop');
#connection
create table student_course
(

    id         int auto_increment primary key,
    student_id int not null,
    course_id  int not null,
    constraint fk_course_id foreign key (course_id) references course (id),
    constraint fk_student_id foreign key (student_id) references student (id)
);
insert into student_course values (null, 1, 1), (null, 1, 2), (null, 1, 3), (null, 2, 2), (null, 2, 3), (null, 3, 4);
```

![image-20221026172655697](C:\Users\zzhan\AppData\Roaming\Typora\typora-user-images\image-20221026172655697.png)



## one to one



## Query

```sql
create table dept(
    id int auto_increment primary key ,
    name varchar(10) not null
);
create table emp(
    id int auto_increment primary key,
    name varchar(10) unique ,
    age tinyint check ( age between 0 and 120),
    entry_date date not null,
    no char(5) not null unique,
    dept_id int not null,
    constraint dept_no foreign key (dept_id) references dept (id)
);
insert into dept values (null, 'research'),(null, 'market'),(null, 'financial'), (null, 'sell'),(null, 'manage'),(null, 'HR');

insert into emp values (null, 'Adam', 20, '2001-01-30', '00001', 1);
insert into emp values (null, 'John', 21, '2000-03-29','00002', 4);
insert into emp values (null, 'Peter', 15, '2012-02-23', '00034', 2);
insert into emp values (null, 'Faker', 23, '2015-12-23', '01425', 5);
insert into emp values (null, 'Jack', 19, '2012-02-05', '20341', 6);


select * from emp, dept; # get 5 * 6 lines
select * from emp, dept where dept_id = dept.id; 



```





## Inner connection

内连接：查询A B 交集部分    

```sql
#隐式内连接
select column_name from table1, table2 where condition;
#显式内连接
select column_name from table1 [inner] join table2 condition;
#exampe: check everyone's name, and the dept's name (emp, dept)
select emp.name, dept.name from emp, dept where emp.dept_id = dept.id;
#inner join
select emp.name, d.name from emp inner join dept d on emp.dept_id = d.id;
```







外连接：

1. 左连接：查询左表所有数据，以及两张表交集部分数据

   ```sql
   select column_name from table1 left [outer] join table2 on condition;
   #example: check all emp data, and dept info
   select emp.*, d.name from emp left outer join dept on emp.dept_id = dept.id; 
   #difference outer connection can check all data, no metter if the data can match the other table
   
   #exmaple: check all dept info and the coorespond emp info
   select dept.*, emp.* from emp right outer join dept on emp.dept_id = dept.id; 
   #same
   select dept.*, emp.* from dept left outer join emp on emp.dept_id = dept.id;
   
   ```

   

2. 右连接：查询右表所有数据，以及两张表交集部分

```sql
select column_name from table1 right [outer] join table2 on condition;
```





自连接：当前表与自身的连接查询。自链接必须使用表别名

```sql
select column_name from table1 alter_name1 join table1 alter_name2 on condition;
#example: check employee, and the leader's name
select emp_name.name, leader_name.name from emp emp_name, emp leader_name where emp_name.manager_id = leader_name.id;
select emp_name.name, leader_name.name, from emp emp_name left join emp leader_name on emp_name.manager_id = leader_name.id;#include all the value from emp_name


```





## Union

对于union查询，就是把多次查询结果合并，形成一个新的查询结果集  

```sql
select column_name from table1.....
union[all]
select column_name from table2.....;

select * from emp where age < 26
union #去重
select * from emp where age > 20;

```



## 子查询

Conception: SQL语句中嵌套select语句，称为嵌套查询，又称子查询

```sql
select * from t1 where column1 = (select column1 from t2);
#子查询外部可以是insert/update/delete/select
#标量子查询:return a single value(num, string, date), it is the simplest form, and it is called 标量子查询
#example: check all employees info in seller
#1. check seller id
select id from dept where name = 'seller'; # return 4
#2. check employee info
select * from emp where dept_id = 4;
#3. combine 
select * from emp where dept_id = (select id from dept where name = 'seller');

#example: check all employees after peter join the company
select * from emp where entry_date > (select entry_date from emp where name = 'Peter');



#列子查询 return a column (multiple line)
#in/not in/any/some/all
#any 子查询返回列表中，有一个满足即可
#some = any
#all 子查询返回列表的所有值都必须满足

#example: check 'sell', 'market' all employee info
select * from emp where dept_id in (select id from dept where name = 'sell' union select id from dept where name = 'market');

#example: check all employees whose salary is greater than anyother people in financial 
select * from emp where salary > all (select salary from emp where dept_id = (select id from dept where name = 'financial')); # use max() works

#example: check all employees whose salary is greater than any one in research
select * from emp where salary > any (select salary from emp where dept_id = (select id from dept where name = 'research'));



```



## 行子查询

子查询返回的结果是一行（可以是多列），这种子查询称为行子查询。

```sql
#example: check employee whose salary = john's , same leader
select * from emp where (salary, leader) = (select salary, leader from emp where name = 'John')
```



## 表子查询 - in

```sql
#check the employee whose salary and position are the same with John and Peter
select * from emp where (job, salary) in (select job, salary from emp where name = 'John' or name = 'Peter');
#check the employee entry date = '2006-01-01'
select emp.*, dept.name from emp left outer join dept on emp.dept_id = dept.id where entry_date > '2001-01-01';
#check name, age, position, dept info
select emp.name, emp.age, emp.position, dept.name from emp left join dept on emp.dept_id = dept.id;
#chech age < 30, name age, position, dept info
select e.name, e.age, e.position, dept.name from (select * from emp where age < 30) e left join dept on e.dept_id = dept.id;
#check departments id and name which have employees
select distinct dept_id, name from dept where dept_id = any (select dept_id from emp); 
select distinct d.id, d.name from emp e, dept d where e.dept_id = d.id;
#check age bigger than 40, dept, show null if dept = null
select emp.*, dept.name from emp left join dept on emp.dept_id = dept.id where emp.age > 40;
#check research employee salary and salary level
select salary, salgrade.level from emp, dept where dept_id = dept.id and dept.name = 'research' and emp.salgrade between salgrade.lower and salgrade.upper;
#check avg salary in research
select avg(salary) from emp where dept_id = (select id from dept where name = 'research');
#check all employees whose salary greater than John;
select * from emp where salary > (select salary from emp where name = 'John');
#check info of employees whose salary is lower than the avg salary of his dept
select * from emp, (select avg(salary) average , dept_id from emp group by dept_id) e where e.dept_id = emp.dept_id and e.salary > emp.salary; 
#check all dept info and num of employee in the dept
select *, e.c from dept, (select count(*) c, dept_id from emp group by dept_id) e where e.dept_id = id;
select d.id, d.name, (select count(*)from emp e where e.dept_id = d.id) from dept d;

```

