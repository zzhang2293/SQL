# Function - String

### concat();

```sql
select concat('a', 'b');
```



### lower();    upper()

```sql
select lower('Hello');
select upper("Hello");
```



### lpad();      rpad();

```sql
#5 is the final length
select lpad('01', 5, '-');
```



### trim();

```sql
select trim("         hello world            ");
```



### substring();

```sql
select substring('hello mysql',1,5);
#start from index 1, 5 is the length
```



# Function number

### 

```sql
#ceil() upper round
select ceil(1.5); #2

#floor() down round
select floor(1.5) #1

#mod(3, 4) 3 mod 4
select mod(7,4);

#rand() random 0-1
select rand();

#round 
select round(2.345, 2); #2.35

#get a 6 digit integer random
select lpad(round(rand()*1000000, 0),6);
```





# Date

```sql
#curdate() dcurrent date
select curdate();

#curtime() current time
select curtime();

#now() current date and time
select now()

#year(date) get a date's year
select year(now())

#month(date) get a date's month
select month(now());

#day(date) get a date's day
select day(now());

#date_add(date, interval expr type) return a date 
select date_add(now(), INTERVAL 70 DAYS);

#datediff(date1, date2)

#query all emploees entry date, and sort by descending
select name, datediff(current_date(), entry_date) as entry_time from emp order by entry_time desc;

```

### 流程函数

```sql
#if(value, t, f)
select if(true, 'ok', 'err'); #ok

#ifNull(value1, vlue2)
select ifnull('ok', 'default');

#case when [val1] then [res1] ... else [default] end
select ifnull('', 'default'); #''

#case query emp name and address, if beijing, shanghai ----> 1线, else----->二线
#ase [expr] when [val1] then [res1] ... else [default] end
select name, work_address when 'beiing' then 'yixian' when 'shanghai' then 'yixian' else 'erxian' as address from emp;
#get all students score and >= 85 is excellent, >= 60 is pass else is fail
select id, name, (case when math >= 85 then 'excellent' when math >= 60 then 'pass' else 'fail' end) math, ...... from score;

```

