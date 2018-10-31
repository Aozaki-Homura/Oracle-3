# 实验二
- 第1步：以system登录到pdborcl，创建角色con_res_view和用户new_user，并授权和分配空间：

```sql
$ sqlplus system/123@pdborcl
SQL> CREATE ROLE ab;
Role created.
SQL> GRANT connect,resource,CREATE VIEW TO ab;
Grant succeeded.
SQL> CREATE USER abc IDENTIFIED BY 123 DEFAULT TABLESPACE users TEMPORARY TABLESPACE temp;
User created.
SQL> ALTER USER abc QUOTA 50M ON users;
User altered.
SQL> GRANT ab TO abc;
Grant succeeded.
SQL> exit
```
> 语句“ALTER USER new_user QUOTA 50M ON users;”是指授权new_user用户访问users表空间，空间限额是50M。

- 第2步：新用户new_user连接到pdborcl，创建表mytable和视图myview，插入数据，最后将myview的SELECT对象权限授予hr用户。

```sql
$ sqlplus account/123@pdborcl
SQL> show user;
USER is "abc"
SQL> CREATE TABLE mytable (id number,name varchar(50));
Table created.
SQL> INSERT INTO mytable(id,name)VALUES(1,'zhang');
1 row created.
SQL> INSERT INTO mytable(id,name)VALUES (2,'wang');
1 row created.
SQL> CREATE VIEW myview AS SELECT name FROM mytable;
View created.
SQL> SELECT * FROM myview;
NAME
--------------------------------------------------
zhang
wang
SQL> GRANT SELECT ON myview TO hr;
Grant succeeded.
SQL>exit
```

- 第3步：用户hr连接到pdborcl，查询new_user授予它的视图myview

```sql
$ sqlplus hr/123@pdborcl
SQL> SELECT * FROM abc.myview;
NAME
--------------------------------------------------
zhang
wang
SQL> exit
```
- 第4步：以下样例查看表空间的数据库文件，以及每个文件的磁盘占用情况。

$ sqlplus system/123@pdborcl
```sql
SQL> SELECT tablespace_name,FILE_NAME,BYTES/1024/1024 MB,MAXBYTES/1024/1024 MAX_MB,autoextensible FROM dba_data_files  WHERE  tablespace_name='USERS';

TABLESPACE_NAME
--------------------------------------------------------------------------------
FILE_NAME
--------------------------------------------------------------------------------
        MB     MAX_MB AUTOEXTEN
---------- ---------- ---------
USERS
/home/oracle/app/oracle/oradata/orcl/pdborcl/SAMPLE_SCHEMA_users01.dbf
       7.5 32767.9844 YES


SQL>

```
