# DCL - data control language

1. 查询用户

   ```sql
   use mysql;
   select * from user;
   ```

2. create user

```sql
create user 'user_name'@'pc_name' identified by 'password';
#example create user itccast, only access fro local host, password 123456
create user 'itcast'@'localhost' identified by '12345678'; #cannot use database
#example create user any pc
create user 'heima'@'%' identified by '123456';
```

3. change password

```sql
alter user 'user_name'@'pc_name' identified with mysql_native_password by 'new_password';

```

4. delete user

```sql
drop user 'user_name'@'pc_name';
```



### DCL 权限控制

1. all, all privilege ---------------------------------->all privilege
2. select------------------------------------------------>query data
3. insert------------------------------------------------>insert data
4. update----------------------------------------------->change data
5. delete------------------------------------------------->delete data
6. alter---------------------------------------------------->change table
7. drop---------------------------------------------------->delete database, table
8. create-------------------------------------------------->create database, table



#### control previlege

```sql
#query xxx previlege
show grants for 'user_name'@'pcname';
#grant previlege
grant all on itcanst.* to 'user_name'@'pc_name';
#revoke previlege
revoke all on itcast.* from 'heima'@'%';

```

