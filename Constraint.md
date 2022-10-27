# Constraint

1. not null
2. unique
3. primary key
4. default
5. check
6. foreign key

```sql
#id id is unique int primary key auto increase 
#name not null, quniue
#age bigger than 0 and smaller than 120
#status if no specific, default 1
#gender




create table user (
    id int primary key auto_increment comment 'main',
    name varchar(10) not null unique comment 'name',
    age int check ( age > 0 && age <= 120 ) comment 'age',
    status varchar(1) default '1',
    gender char(1)
) comment 'user table';

#foreign key
create table table_name(
	.....
    [constraint] [foreign_key_name] foreign key [foreign_key_column_name] main_table_name
)
#condition 2
alter table table_name add constraint foreign_key_name foreign key foreign_key_column_name references main_table;

alter table emp add constraint fk_emp_dept_id foreign key (dept_id) references dept(id);
#delete foreign key
alter table table_name drop foreign key key_name;

alter table emp drop foreign key fk_emp_dept_id;


```

## delete/update

| 行为        | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| No Action   | 当在父表中删除/更新对应记录时，首先检查该记录是否对应有外键，如果有则不允许删除/更新。（与restrict一致） |
| restrict    |                                                              |
| cascade     | ......................................................, 首先检查该记录是否有对应外键，如果有，则也删除或更新外键在子表中记录 |
| set null    | 如果有则设置子表中外键值为null                               |
| set default | 子表将外键列设置成一个默                                     |

```sql
alter table emp add constraint fk_emp_dept_id foreign key (dept_id) references dept(id) on update cascade on delete cascade;

```

