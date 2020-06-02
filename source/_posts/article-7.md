---
title: MySQL基本操作
date: 2020-05-19 14:18:10
tags:
- MySQL
categories:
- 技术
- 数据库
---

version：8.0.17



## 常用基础命令

### 使用帮助

```mysql
help create
```

```bash
Many help items for your request exist.
To make a more specific request, please type 'help <item>',
where <item> is one of the following
topics:
   CREATE DATABASE
   CREATE EVENT
   CREATE FUNCTION
   CREATE FUNCTION UDF
   CREATE INDEX
   CREATE LOGFILE GROUP
   CREATE PROCEDURE
   CREATE RESOURCE GROUP
   CREATE ROLE
   CREATE SCHEMA
   CREATE SERVER
   CREATE SPATIAL REFERENCE SYSTEM
   CREATE TABLE
   CREATE TABLESPACE
   CREATE TRIGGER
   CREATE USER
   CREATE VIEW
   SHOW
   SHOW CREATE DATABASE
   SHOW CREATE EVENT
   SHOW CREATE FUNCTION
   SHOW CREATE PROCEDURE
   SHOW CREATE SCHEMA
   SHOW CREATE TABLE
   SHOW CREATE USER
   SPATIAL INDEXES
```



### **创建、删除、查看数据库**

#### 创建默认字符集的数据库（默认是拉丁字符集）

```mysql
create database test_data;
```

```mysql
show databases like "test%";
```

#### **创建gbk字符集的数据库**

```mysql
create database test_gbk DEFAULT CHARACTER SET gbk COLLATE gbk_chinese_ci;
```

#### 查看创建数据库的语句

```mysql
show create database test_gbk;
```

#### 删除数据库

```mysql
drop database test_data;
```

### 连接数据库

#### 进入指定数据库操作

```mysql
use test_gbk;
```

#### **查看当前连接的数据库**

```mysql
select database();
```

#### **查看当前连接数据库的用户**

```mysql
select user();
```

### 创建用户

```mysql
create user if not exists 'testuser'@'localhost' identified by '123456';
```

```mysql
create user if not exists 'company_read_only'@'localhost' identified with mysql_native_password by 'company_pass' with max_queries_per_hour 500 max_updates_per_hour 100;
```

上述声明将为用户创建以下内容:

- 用户名 : company read only
- 仅从localhost访问
- 可以限制对 IP范围的访问，例如 10.148.%.%。 通过给出%，用户可以从任何主机访问
- 密码：company_pass
- 使用 mysql_native_password（默认）身份验证
- 还可以指定任何可选的身份验证，例如 sha256_password、LDAP 或 Kerberos
-  用户可以在一小时内执行的最大查询数为 500
- 用户可以在一小时内执行的最大更新次数为 100次

### 授予和撤销用户的访问权限

你可以限制用户访问特定数据库或表，或限制特定操作，如 SELECT 、 INSERT 和UPDATE。 你需要拥有 GRANT 权限，才能为其他用户授予权限 。

#### 授予权限

- 将READ ONLY (SELECT)权限授予testuser用户

```mysql
grant select on company.* to 'testuser'@'localhost';
```

- 限制查询指定的表。 将testuser用户限制为仅能查询employees 表 

```mysql
grant select on employees.employees to 'testuser'@'localhost';
```

- 将访问权限限制为仅能查询指定列。限制testuser用户仅能访问employees表的first_name列和last_name列

```mysql
grant select(first_name, last_name) on employees.employees to 'testuser'@'localhost';
```

- 扩展授权。可以通过执行新授权来扩展授权。

```mysql
grant select(salary) on employees.salaries to 'company_read_only'@'localhost';
```

- 创建 SUPER 用户 。 需要一个管理员账户来管理该服务器 。 ALL 表示除 GRANT 权限之外的所有权限 。

```mysql
create user 'super_admin'@'%' identified with mysql_native_password by 'super@admin';
```

```mysql
grant all on *.* to 'super_admin'@'%';
```

- 授予 GRANT特权。 用户拥有 GRANT OPTION权限才能授予其他用户权限。 可以将 GRANT 特权扩展到 super_admin 超级用户

```mysql
grant grant option on *.* to 'super_admin'@'%';
```

#### **检查授权**

```mysql
show grants for 'super_admin'@'%';
```

```mysql
show grants for 'company_read_only'@'localhost';
```

```mysql
show grants for 'testuser'@'localhost';
```

#### 撤销权限

```mysql
revoke insert,update,select,delete on test_gbk.* from 'testuser'@'localhost';

# 回收后查看权限
mysql> show grants for 'testuser'@'localhost';
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Grants for testuser@localhost                                                                                                                                                                                |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| GRANT USAGE ON *.* TO `testuser`@`localhost`                                                                                                                                                                 |
| GRANT CREATE, DROP, REFERENCES, INDEX, ALTER, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, EVENT, TRIGGER ON `test_gbk`.* TO `testuser`@`localhost` |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
2 rows in set (0.00 sec)
```

撤销 company_read_only 用户对薪水列的访问权限

```mysql
revoke select(salary) on employees.salaries from 'company_read_only'@'localhost';
```

#### 修改 mysql.user 表

所有用户信息及权限都存储在 mysql.user表中 。如果你有权访问 mysql.user表， 则可以直接通过修改 mysql.user 表来创建用户并授予权限 。

如果你使用 GRANT 、 REVOKE 、 SET PASSWORD 或 RENAME USER 等账户管理语句间 接修改授权表， 则 服务器会通知这些更改，并立即再次将授权表加载到内存中 。

如果使用 INSERT、 UPDATE 或 DELETE 等语句直接修改授权表，则更改不会影响权限检查，除非你重新启动服务器或指示其重新加载表 。 如果直接更改授权表，但忘记了重新加载表，那么在重新启动服务器之前，这些更改无效 。

可以通过执行 FLUSH PRIVILEGES 语句来完成 GRANT 表的重新加载 。

```mysql
update mysql.user set host='localhost' where user='super_admin';
```

```mysql
flush privileges;
```

#### 锁定用户

MySQL支持使用 CREATE USER或 ALTER USER 锁定用户

```mysql
alter user 'company_read_only'@'localhost' account lock;
```

解锁

```mysql
alter user 'company_read_only'@'localhost' account unlock;
```

#### 为用户创建角色

MySQL 的角色是一个权限的集合。 与用户账户一样，角色的权限可以被授予和撤销。 用户账户被授予角色后， 该角色就会将其拥有的权限授予该账户 。 之前，我们为不同的用 户创建了读取 、写入和管理权限。对于写入权限，我们已授予用户 INSERT、 DELETE 和 UPDATE 权限 。 现在你可以将这些权限授予某个角色， 然后为用户分配该角色。通过这种方式，可以避免为许多用户账户单独授予权限的麻烦。

- 创建角色

```mysql
create role 'app_read_only', 'app_writes', 'app_developer';
```

- 使用 GRANT 语句为角色分自己权限

```mysql
grant select on employees.* to 'app_read_only';
```

```mysql
grant insert, update, delete on employees.* to 'app_writes';
```

```mysql
grant all on employees.* to 'app_developer';
```

- 创建用户。如果你不指定主机，则将采用 %(任意主机):

```mysql
create user emp_read_only identified by '123';
create user emp_write identified by '123';
create user emp_develpoer identified by '123';
create user emp_read_write identified by '123';
```

- 使用 GRANT 语句为用户分配角色 。你可以为用户分配多个角色 。

```mysql
grant 'app_read_only' to emp_read_only;
grant 'app_writes' to emp_write;
grant 'app_developer' to emp_develpoer;
grant 'app_read_only', 'app_writes' to emp_read_write;
```

### 创建、删除表

在表中定义列时，应该指定列的名称、数据类型(整型、浮点型、字符串等)和默认 值(如果有的话) 。 MySQL 支持各种数据类型。更多有关信息请参阅MySQL文档(https://dev.mysql.com/doc/refman/8.0/en/spatial-extensions.html )。下面是所有数据类型的概述，其中 JSON 数据类型是一个新的扩展类型  。

1. 数字 : TINYINT 、 SMALLINT 、 MEDIUMINT 、 INT 、 BIGINT 和 BIT。
2. 浮点数 : DECIMAL、 FLOAT 和 DOUBLE。

3. 字符串: CHAR、 VARCHAR、 BINARY、 VARBINARY、 BLOB、 TEXT、 ENUM 和 SET。

1. Spatial 数据类型，更多详细信息请参阅 https://dev.mysql.com/doc/refman/8.0/en/spatial-extensions.html 。
2. JSON 数据类型，将在后面单独讨论。

你可以在一个数据库中创建多张表 。

#### **建表，并且建立两个字段**

```mysql
mysql> create table test(
       id int(4) not null,
       name char(20) not null
       );
```

#### 另一种方式

```mysql
mysql> create table if not exists test_gbk.customers(
       id int unsigned AUTO_INCREMENT PRIMARY KEY,
       first_name varchar(20),
       last_name varchar(20),
       country varchar(20)
       ) engine=InnoDB;
```

其中的选项解释如下 。

• 句点符号: 表可以使用 database.table 引用。 如果已经连接到数据库， 则可 以简单地使用 customers 而不是 company . customers。

• IF NOT EXISTS :如果存在一个具有相同名字的表 ， 并且你指定了这个子句， MySQL 只会抛出一个警告，告知表已经存在 。 否则 ， MySQL 将抛出 一个错误 。

• id:它被声明为一个整型数，因为它只包含整型数。除此之外，还有两个关键字， AUTO_INCREMENT 和 PRIMARY KEY 。

• AUTO INCREMENT: 自动生成线性递增序列，因此不必担心为每一行的 id分配 值。

• PRIMARY KEY: 每行都Fl'!一个非空的UNIQUE列标识。 只有一列应该在表中定 义。 如果一个表包含 AUTO INCREMENT列， 则它会被视为 PRIMARY KEY。

• first_name、 last_name 和country: 它们包含字符串 ， 因此它们被定义为 varchar 。

• Engine:与列定义一起，还应该指定存储引擎。一些类型的存储引擎包括 InnoDB、 MyISAM、FEDERATED、BLACKHOLE、CSV和MEMORY。在所有引擎中， InnoDB 是唯一的事务引擎， 也是默认引擎。

#### 查看表

```MYSQL
show tables;
```

#### **查看表结构**

```mysql
desc test;
```

#### **删除表**

```mysql
drop table test;
```

#### **查看建表**

```mysql
show create table test1\G
```

#### 克隆表结构

```mysql
create table new_customers like customers;
```

### 插入、更新和删除行

#### INSERT 语句用于在表中创建新记录

```mysql
mysql> insert ignore into customers
       (first_name, last_name, country)
       values
       ('Mike', 'Galler', 'USA'),
       ('Andy', 'Hollands', 'Australia'),
       ('Ravi', 'Vadantam', 'India'),
       ('Rajiv', 'Perera', 'Sri Lanka');
```

或者可以明确地写出 id列，如果你想插入特定的 id:

```mysql
mysql> insert ignore into customers
       (id, first_name, last_name, country)
       values
       (1, 'Mike', 'Galler', 'USA'),
       (2, 'Andy', 'Hollands', 'Australia'),
       (3, 'Ravi', 'Vadantam', 'India'),
       (4, 'Rajiv', 'Perera', 'Sri Lanka');
```

IGNORE:如果该行已经存在，并给出了 IGNORE子句，则新数据将被忽略， INSERT 语句仍然会执行成功，同时生成一个警告和重复数据的数目。 反之，如果未给出 IGNORE 子句，则 INSERT 语句会生成一条错误信息 。 行的唯一性由主键标识。

#### UPDATE 语句用于修改表中的现有记录

```mysql
update customer set first_name='Rajiv', country='UK' where id=4;
```

WHERE : 这是用于过滤的子句 。 在 WHERE 子句后指定的任何条件都会用于过滤，被筛选出来的行都会被更新 。

#### DELETE 语句用于删除表中记录

```mysql
delete from customers where id=5 and first_name='Mike';
```

#### REPLACE 、INSERT 、ON DUPLICATE KEY UPDATE

```mysql
replace into customers values (1, 'Mike', 'Galler', 'America');
```

```mysql
insert into payments values ('Mike Galler', 200) on duplicate key update payment=payment+values(payment);
```

在很多情况下，我们需要处理重复项 。行的唯一性由主键标识 。如果行已经存在，则 REPLACE 会简单地删除行并插入新行;如果行不存在，则 REPLACE 等同于 INSERT。

如果你想在行已经存在的情况下处理重复项，则需要使用 ON DUPLICATE KEY UPDATE。 如果指定了 ON DUPLICATE KEY UPDATE 选项，并且 INSERT 语句在 PRIMARY KEY中引发了重复值， 则MySQL会用新值更新已有行。

区别：

（1）在没有主键或者唯一索引重复时，replace与insert .. on deplicate udpate相同。

（2）在主键或者唯一索引重复时，**replace是delete老记录，而录入新的记录，所以原有的所有记录会被清除，这个时候，如果replace语句的字段不全的话，有些原有的比如c字段的值会被自动填充为默认值。**而insert .. duplicate update则只执行update标记之后的sql，从表象上来看相当于一个简单的update语句。它保留了所有字段的旧值，只更新update后面的语句，而replace没有保留旧值，直接删除再insert新值。
从底层执行效率上来讲，replace要比insert .. on duplicate update效率要高，但是在写replace的时候，字段要写全，防止老的字段数据被删除。

#### TRUNCATING TABLE

删除整个表需要很长时间，因为 MySQL 需要逐行执行操作。 删除表的所有行(保留 表结构)的最快方法是使用TRUNCATE TABLE语句。

TRUNCATING TABLE 是 MySQL 中的 DDL 操作，也就是说一旦数据被清空 ，就不能被回滚。

```mysql
truncate table test1;
```

### 加载示例数据

#### 下载压缩文件

```bash
wget 'https://codeload.github.com/datacharmer/test_db/zip/master' -O master.zip
```

#### 解斥缩文件

```bash
unzip master.zip
```

#### 加载数据

```bash
cd test_db-master
```

```bash
mysql -u root -p < employees.sql
```

#### 验证数据

```bash
mysql -u root -p employees -A
```

```mysql
mysql> show tables;
+----------------------+
| Tables_in_employees  |
+----------------------+
| current_dept_emp     |
| departments          |
| dept_emp             |
| dept_emp_latest_date |
| dept_manager         |
| employees            |
| salaries             |
| titles               |
+----------------------+
8 rows in set (0.00 sec)
```

```msyql
mysql> desc employees\G
*************************** 1. row ***************************
  Field: emp_no
   Type: int(11)
   Null: NO
    Key: PRI
Default: NULL
  Extra:
*************************** 2. row ***************************
  Field: birth_date
   Type: date
   Null: NO
    Key:
Default: NULL
  Extra:
*************************** 3. row ***************************
  Field: first_name
   Type: varchar(14)
   Null: NO
    Key:
Default: NULL
  Extra:
*************************** 4. row ***************************
  Field: last_name
   Type: varchar(16)
   Null: NO
    Key:
Default: NULL
  Extra:
*************************** 5. row ***************************
  Field: gender
   Type: enum('M','F')
   Null: NO
    Key:
Default: NULL
  Extra:
*************************** 6. row ***************************
  Field: hire_date
   Type: date
   Null: NO
    Key:
Default: NULL
  Extra:
6 rows in set (0.00 sec)
```

### 查询数据

#### 查询所有列

```mysql
select * from departments;
```

#### 选择列

选择 dept_manager 的 emp_no 和 dept_no 列：

```mysql
select emp_no,dept_no from dept_manager;
```

#### 计数

从 employees表中查找员工的数量

```mysql
select count(*) from employees;
```

#### 条件过滤

```mysql
select emp_no from employees where first_name='Georgi' and last_name='Facello';
```

#### 操作符

- IN: 检查一个值是存在一组值中

  ```mysql
  select count(*) from employees where last_name in ('Christ', 'Lamba', 'Baba');
  ```

- BETWEEN ...AND:检查一个值是否在一个范围内

  ```mysql
  select count(*) from employees where hire_date between '1986-12-01' and '1986-12-31';
  ```

- NOT 你可以简单地用 NOT 运算符来否定结果

  ```mysql
  select count(*) from employees where hire_date not between '1986-12-01' and '1986-12-31';
  ```

#### 简单模式匹配

可以使用 LIKE 运算符来实现简单模式匹配。 使用下画线( _ )来精准匹配一个字符，使用( % ) 来匹配任意数量的字符 。

- 找出名字以 Christ开头的所有员工的人数

  ```mysql
  select count(*) from employees where first_name like 'christ%';
  ```

- 找出名字以 Christ开头并以 ed结尾的所有员工的人数

  ```mysql
  select count(*) from employees where first_name like 'christ%ed';
  ```

- 找出名字中包含 sri的所有员工的人数

  ```mysql
  select count(*) from employees where first_name like '%sri%';
  ```

- 找到名字以 er 结尾的所有员工的人数

  ```mysql
  select count(*) from employees where first_name like '%er';
  ```

- 找出名字以任意两个字符开头、后面跟随 ka、再后面跟随任意数量字符的所有员工的人数

  ```mysql
  select count(*) from employees where first_name like '__ka%';
  ```

#### 正则表达式

你可以利用 RLIKE 或 REGEXP 运算符在 WHERE 子句中使用正则表达式

- 找出名字以 Christ开头的所有员工的人数

  ```mysql
  select count(*) from employees where first_name rlike '^christ';
  ```

- 找出姓氏以 ba结尾的所有员工的人数

  ```mysql
  select count(*) from employees where last_name regexp 'ba$';
  ```

- 查找姓氏不包含元音 (a、 e、 i、 o和u)的所有员工的人数

  ```mysql
  select count(*) from employees where last_name not regexp '[aeiou]';
  ```

#### 限定结果

查询hire date在1986年之前的任何10名员工的姓名

```mysql
select first_name, last_name from employees where hire_date < '1986-01-01' limit 10;
```

#### 使用表别名

使用自别名来更改 COUNT (*)

```mysql
mysql> select count(*) as count from employees where hire_date between '1986-12-01' and '1986-12-31';
+-------+
| count |
+-------+
|  3081 |
+-------+
1 row in set (0.09 sec)
```

### 对结果排序

查找薪水最高的前 5名员工的员工编号

```mysql
select emp_no, salary from salaries order by salary desc limit 5;
```

你可以在 SELECT 语句中提及列的位置，而不是指定列名称

```mysql
select emp_no, salary from salaries order by 2 desc limit 5;
```

### 对结果分组(聚合函数)

#### COUNT

- 分别找出男性和女性员工的人数

  ```mysql
  select gender, count(*) as count from employees group by gender;
  ```

- 如果你希望查找员工名字中最常见的10个名字，可以使用 GROUP BY first_name 对所有名字分组，然后使用COUNT(first_name)在各组内计数，最后使用ORDER BY计数对结果进行排序 并将返回结果行数限制为前 10行

  ```mysql
  select first_name,count(first_name) as count from employees group by first_name order by count desc limit 10;
  ```

#### SUM

查找每年给予员工的薪水总额并按薪水高低对结果进行排序。 YEAR ()函数将返回给定日期所在的年份

```mysql
select year(from_date) as date, sum(salary) as sum from salaries group by date order by sum desc;
```

#### AVERAGE

查找平均工资最高的10名员工

```mysql
select emp_no, avg(salary) as avg from salaries group by emp_no order by avg desc limit 10;
```

#### DISTINCT

可以使用 DISTINCT 子句过滤出表中的不同条目

```mysql
select distinct title from titles;
```

#### 使用 HAVING 过滤

可以通过添加HAVING子句来过滤GROUP BY子句的结果。 例如，找到平均工资超过 140,000美元的员工

```mysql
select emp_no,avg(salary) as avg from salaries group by emp_no having avg > 140000 order by avg desc;
```

### 查询数据并保存到文件和表中

我们可以使用 SELECT INTO OUTFILE 语句将输出保存到文件中。

可以指定列和行分隔符，然后可以将数据导入其他数据平台 。

#### 另存为文件

- 要将输出结果保存到文件中，你需要拥有 FILE权限

```mysql
create user user_to_file identified by '123';
grant select on employees.* to user_to_file;
grant file on *.* to user_to_file;
```

- 在Ubuntu系统中， 默认情况下， MySQL不允许写人文件。 你应该在配置文件中 设置secure_file_priv并重新启动MySQL，配置方法如下

```bash
# 在 my.cnf配置文件中添加如下信息 /usr/local/mysql/support-files/my.cnf
[mysqld]
secure-file-priv="/tmp"
```

​		如果没有配置文件 my.cnf 需手动创建

```bash
sudo vim /etc/my.cnf
```

​		然后在文件中太监如下信息

```bash
[client]
default-character-set=utf8
[mysqld]
character-set-server=utf8

#sql_mode='NO_AUTO_VALUE_ON_ZERO,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION,PIPES_AS_CONCAT,ANSI_QUOTES'

sql_mode='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION'

[mysql]
default-character-set=utf8
```

​		最后重启mysql服务

```bash
sudo /usr/local/mysql/support-files/mysql.server restart
```

​		进入mysql查看配置

```mysql
mysql> show variables like '%secure%';
+--------------------------+---------------+
| Variable_name            | Value         |
+--------------------------+---------------+
| require_secure_transport | OFF           |
| secure_file_priv         | /private/tmp/ |
+--------------------------+---------------+
2 rows in set (0.01 sec)
```

以下语句会将输出结果保存为 csv 格式

```mysql
select first_name, last_name into outfile '/tmp/result.csv' fields terminated by ',' optionally enclosed by '"' lines terminated by  '\n' from employees where hire_date<'1986-01-01' limit 10;
```

#### 另存为表

我们也可以将 SELECT 语句的结果保存到表中 。 即使表不存在， 也可以使用 CREATE 和 SELECT 来创建表并加载数据 。 如果表己存在，则可以使用 INSERT 和 SELECT 加载数据。

可以将标题保存到新的 titles_only 表中

```mysql
create table titles_only as select distinct title from titles;
```

如果表已经存在，则可以使用 INSERT INTO SELECT 语句

```mysql
insert into select distinct title from titles;
```

### 将数据加载到表中

创建一个表来保存数据

```mysql
mysql> create table emplyee_names (
       first_name varchar(14) not null,
       last_name varchar(16) not null
       ) engine=innodb;
```

确保文件存在

```bash
sudo ls -lhtr /tmp/result.csv
```

使用 LOAD DATA INFILE 语句加载数据

```mysql
mysql> load data infile '/tmp/result.csv' into table emplyee_names
       fields terminated by ','
       optionally enclosed by '"'
       lines terminated by '\n';
```

该文件可以以完整路径名的形式给出，以指定其确切位置 。 如果以相对路径名的形式给出，则相对路径名将被解析为相对于客户机程序启动的目录 。

- 如果文件开头包含一些你想忽略的行，可以用 IGNORE n Lines 指定

```mysql
mysql> load data infile '/tmp/result.csv' into table emplyee_names
       fields terminated by ','
       optionally enclosed by '"'
       lines terminated by '\n'
       ignore 1 lines;
```

- 可以用 REPLACE或者 IGNORE来处理重复的行

```mysql
mysql> load data infile '/tmp/result.csv' replace into table emplyee_names
       fields terminated by ','
       optionally enclosed by '"'
       lines terminated by '\n'
       ignore 1 lines;
```

```mysql
mysql> load data infile '/tmp/result.csv' ignore into table emplyee_names
       fields terminated by ','
       optionally enclosed by '"'
       lines terminated by '\n'
       ignore 1 lines;
```

### 表关联

假设你想用 emp_no: 110022 找到员工的姓名和部门号码:

- 部门编号和名称存储在 departments表中
- 员工编号和其他详细信息(例如 first_name 和 last_name )存储在 employees 表中 
- 员工和部门的映射关系存储在 dept_manager表中

如果你不想使用 JOIN，可以这样做

1. 从 employee 表中查找 emp_no 为 110022 的员工姓名

   ```mysql
   select emp.emp_no, emp.first_name, emp.last_name from employees as emp where emp.emp_no=110022;
   ```

2. 从 dept_manager 表中查找部门编号

   ```mysql
   select dept_no from dept_manager where emp_no='110022';
   ```

3. 从 departments 表中查找部门名称

   ```mysql
   select dept_name from departments dept where dept.dept_no='d001';
   ```

#### 使用join操作

```mysql
mysql> select emp.emp_no, emp.first_name, emp.last_name, dept.dept_name from employees emp
       join dept_manager dept_mgr
           on emp.emp_no=dept_mgr.emp_no and emp.emp_no=110022
       join departments dept
           on dept_mgr.dept_no=dept.dept_no;
```

假设计、想了解每个部门的平均工资，你可以使用 AVG 函数并按照dept_no进行分组。要找出部门名称，可以将结果与departments 表通过dept_no 列进行关联

```mysql
select dept_name, avg(salary) as avg_salary
from
    salaries
join dept_emp
    on salaries.emp_no=dept_emp.emp_no
join departments as dept
    on dept_emp.dept_no=dept.dept_no
group by
    dept_emp.dept_no
order by 
	avg_salary 
desc
;
```

#### 通过与自己关联来识别重复项

```mysql
select empl1.*
from
    employees as empl1
join employees empl2
    on empl1.first_name=empl2.first_name
    and empl1.last_name=empl2.last_name
    and empl1.gender=empl2.gender
    and empl1.hire_date=empl2.hire_date
    and empl1.emp_no!=empl2.emp_no
order by
    first_name, last_name;
```

#### 使用子查询

```mysql
select
    first_name, last_name
from 
    employees
where
    emp_no
in
    (
        select 
            emp_no 
        from 
            titles
        where
            title="Senior Engineer" and from_date="1986-06-26"
    )
;
```

找到工资最高的员工

```mysql
select
    emp_no
from 
    salaries
where 
    salary=(
        select 
            max(salary) 
        from 
            salaries
    )
;
```

#### 查找表之间不匹配的行

```mysql
create table employees_list1 as select * from employees where first_name like 'aa%';
```

```mysql
create table employees_list2 as select * from employees where emp_no between 400000 and 500000 and gender='F';
```

我们已经知道如何找到两个列表中都存在的员工了 ，代码如下

```mysql
select
    l1.*
from 
    employees_list1 l1
join employees_list2 l2
    on l1.emp_no=l2.emp_no
;
```

现在要找出存在于 employees_listl 但不存在于 employees_list2 中的员工

```mysql
select 
    *
from 
    employees_list1 l1
where 
    l1.emp_no 
not in 
    (
        select
            l2.emp_no
        from 
            employees_list2 l2
    )
;
```

或者也可以使用 OUTER JOIN

```mysql
select
    l1.*
from 
    employees_list1 l1
left outer join employees_list2 l2
    on l1.emp_no=l2.emp_no
where
    l2.emp_no is null
;
```

outer join为第二个表中所有与第一个表中的行不匹配的行创建 NULL列。 如果使 用 RIGHT JOIN，则为第一个表中所有与第二个表中的行不匹配的行创建 NULL 列。

你也可以使用 OUTER JOIN 来查找民自己的行

```mysql
select
    l1.*
from 
    employees_list1 l1
left outer join employees_list2 l2
    on l1.emp_no=l2.emp_no
where 
    l2.emp_no is not null
;
```











