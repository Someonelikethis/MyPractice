```sh
// 查看系统用户
more /etc/passwd
// 登录安装oracle的linux用户，登录root再登录oracle用户
su - oracle
// 登录dba
sqlplus / as sysdba
// 登录数据库用户
sqlplus cw/cw19730207
// 导出用户全部数据
exp user/password  file=xxx.dmp full=y
// 停用oracle
shutdown immediate
// 启用oracle
startup
```



授权

```sql
// 授权用户dba权限
grant dba to cw;
// 授权某表查询权限给某用户
grant select on fschat.sys_user to qyyp;
// 授权取别名
grant create synonym to qyyp;
// 解除用户表空间限额
grant unlimited tablespace to fschat;
// 授权用户权限
grant connect,resource to fschat;
// 收回用户权限
revoke dba from fschat;
```



取别名

```sql
create synonym sys_user from fschat.sys_user;
```



查询用户表空间名称

```sql
select default_tablespace from dba_users where username='登录用户';
```



修改用户密码

```sql
// 登录dba
sqlplus / as sysdba;
alter user 用户名 identified by 新密码;
```



创建用户

```sql
create user test identified by test;
grant connect,resource to test;
```



授权并创建视图

```sql
grant select on sys_dept to qyyp;

create view sys_user as select * from fschat.sys_user;
create view sys_dept as select * from fschat.sys_dept;
```



查看当前机器及连接数

```sql
select machine,count(machine) from v$session group by machine;
```



查看最大连接数配置

```sql
select value from v$parameter where name ='processes';
默认150
```



imp导入

```
imp JNPFINIT/JNPFINIT fromuser=JNPFINIT touser=JNPFINIT rows=y indexes=y commit=y buffer=50000000 ignore=n file=/home/oracle/exp_JNPFINIT.dmp log=/home/oracle/exp_JNPFINIT.log
```



查询当前用户表空间

```
SELECT USERNAME, DEFAULT_TABLESPACE FROM DBA_USERS WHERE USERNAME = USER;
SELECT USERNAME, DEFAULT_TABLESPACE FROM USER_USERS;
```



查询oracle版本

```
SELECT * FROM v$version;
```



查找所有drop table

(DROP\s+TABLE\s+[^;]*);
