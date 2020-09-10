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

MySQL基本操作

<!-- more -->



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

​		然后在文件中添加如下信息

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
insert into titles_only select distinct title from titles;
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

假设想了解每个部门的平均工资，你可以使用 AVG 函数并按照dept_no进行分组。要找出部门名称，可以将结果与departments 表通过dept_no 列进行关联

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

### 存储过程

存储过程处理的事一组SQL语句，且没有返回值

#### 如何操作



```mysql
/* 删除已存在的存储过程 */
drop procedure if exists create_employee;
/* 分隔符修改为 $$ */
delimiter $$

/* IN 指定作为参数的变量，INOUT指定输出的变量 */
create procedure create_employee (
	Out new_emp_no INT,
	IN first_name varchar(20),
	IN last_name varchar(20),
	IN gender enum('M', 'F'),
	IN birth_date date,
	IN emp_dept_name varchar(40),
	IN title varchar(50)
)
BEGIN
	/* 为emp_dept_no 和 salary 声明变量 */
		declare emp_dept_no char(4);
		declare salary int default 60000;

	/* 查询employees表中emp_no的最大值，赋值给变量 new_emp_no */
	select max(emp_no) into new_emp_no from employees;

	/* 增加 new_emp_no */
	set new_emp_no = new_emp_no + 1;

	/* 插入数据到 employees 表中 */
	/* curdate() 函数给出当前日期 */
	insert into employees values(new_emp_no, birth_date, first_name, last_name, gender, curdate());

	/* 找到 dept_name对应的dept_no */
	select emp_dept_name;

	select dept_no into emp_dept_no from departments where dept_name=emp_dept_name;

	select emp_dept_no;

	/* 插入dept_emp */
	insert into dept_emp values(new_emp_no, emp_dept_no, curdate(), '9999-01-01');

	/* 插入 title */
	insert into titles values(new_emp_no, title, curdate(), '9999-01-01');
	
	/* 以title为条件查询的薪水 */
	if title = 'Staff'
		then set salary = 100000;
	elseif title = 'Senior Staff'
		then set salary = 120000;

	end if;

	/* 插入 salaries */
	insert into salaries values(new_emp_no, salary, curdate(), '9999-01-01');

END
$$

/* 将分隔符改回 ; */
DELIMITER ;
```

- 在mysql命令行终端执行代码

- 保存为文件，使用命令 mysql -u root -p employees < stored_procedure.sql

- 使用 source 从文件加载 mysql> source stored_procedure.sql

  

要使用存储过程，需要将 execute 权限授予 emp_read_only 用户：

```mysql
grant execute on employees.* to 'emp_read_only'@'%';
```

使用CALL stored_procedure(OUT 变量， IN值) 语句和例程的名称调用存储过程。

使用emp_read_only 账户链接到MySQL

```bash
mysql -u emp_read_only -p123 employees -A
```

把想要传递的输出值存储在@new_emp_no变量中的emp_no的值：

```mysql
select @new_emp_no;
```

检查是否在employees表、salaries表和tiltes表创建了行：

```mysql
mysql> select * from employees where emp_no=500001;
+--------+------------+------------+-----------+--------+------------+
| emp_no | birth_date | first_name | last_name | gender | hire_date  |
+--------+------------+------------+-----------+--------+------------+
| 500001 | 1984-06-19 | John       | Smith     | M      | 2020-07-15 |
+--------+------------+------------+-----------+--------+------------+
1 row in set (0.01 sec)

mysql> select * from salaries where emp_no=500001;
+--------+--------+------------+------------+
| emp_no | salary | from_date  | to_date    |
+--------+--------+------------+------------+
| 500001 | 100000 | 2020-07-15 | 9999-01-01 |
+--------+--------+------------+------------+
1 row in set (0.01 sec)

mysql> select * from titles where emp_no=500001;
+--------+-------+------------+------------+
| emp_no | title | from_date  | to_date    |
+--------+-------+------------+------------+
| 500001 | Staff | 2020-07-15 | 9999-01-01 |
+--------+-------+------------+------------+
1 row in set (0.00 sec)
```



### 触发器

触发器用于在触发器事件之前或之后激活某些内容。

#### 如何操作

例如：假设你希望在将薪水插入 salaries 表之前对其进行四舍五入 。 NEW 指的是正在插入的新值 :

```mysql
drop trigger if exists salary_round;
delimiter $$
create trigger salary_round before insert on salaries
for each row
BEGIN
	set NEW.salary=ROUND(NEW.salary);
END
$$
delimiter ;
```

```mysql
source /tmp/before_insert_trigger.sql
```

```mysql
mysql> insert into salaries values(10002, 100000.79, curdate(), '9999-01-01');
Query OK, 1 row affected (0.01 sec)
mysql> select * from salaries where emp_no=10002 and from_date=curdate();
+--------+--------+------------+------------+
| emp_no | salary | from_date  | to_date    |
+--------+--------+------------+------------+
|  10002 | 100001 | 2020-07-15 | 9999-01-01 |
+--------+--------+------------+------------+
1 row in set (0.01 sec)
```

假设你要记录 salaries 表中新增的薪水记录：

创建审计表：

```mysql
create table salary_audit (emp_no int, user varchar(50), date_modified date);
```

请注意， 以下触发器 PRECEDES salary_round 指定在 salary_round 触发器之前执行:

```mysql
delimiter $$
create trigger salary_audit
BEFORE INSERT
	on salaries for each row precedes salary_round
BEGIN
	insert into salary_audit value(NEW.emp_no, USER(), curdate());
END;$$
delimiter ;
```

```mysql
mysql> insert into salaries values(10003, 100000.79, curdate(), '9999-01-01');
Query OK, 1 row affected (0.00 sec)

mysql> select * from salary_audit where emp_no=10003;
+--------+----------------+---------------+
| emp_no | user           | date_modified |
+--------+----------------+---------------+
|  10003 | root@localhost | 2020-07-15    |
+--------+----------------+---------------+
1 row in set (0.00 sec)
```

### 

### 视图

视图是一个基于 SQL语句的结果集的虚拟表。我们可以使用视图来限制用户对特定行的访问 。

#### 如何操作

创建只对salaries表的emp_no 和 salary 列，且from_date在 ‘2002-01-01’ 之后的数据的访问权限.

```mysql
create ALGORITHM=UNDEFINED
DEFINER=`root`@`localhost`
SQL security DEFINER view salary_view
as
select emp_no, salary from salaries where from_date > '2002-01-01';
```

```mysql
mysql> select emp_no, avg(salary) as avg from salary_view group by emp_no order by avg desc limit 5;
+--------+-------------+
| emp_no | avg         |
+--------+-------------+
|  43624 | 158220.0000 |
|  47978 | 155709.0000 |
| 253939 | 155513.0000 |
| 109334 | 155190.0000 |
|  80823 | 154459.0000 |
+--------+-------------+
5 rows in set (1.54 sec)
```

列出所有视图

```mysql
show full tables where table_type like 'VIEW';
```

检查视图的定义

```mysql
show create view salary_view\G
```

我们可以更新没有子查询、 JOINS 、 GROUP BY 子句、 union 等的简单视阁 。 如果基础表有默认值， 那么 salary_view 就是一个可以被更新或插入的简单视图 :

```mysql
mysql> update salary_view set salary=100000 where emp_no=10001;
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

```mysql
mysql> insert into salary_view values(10001, 100001);
ERROR 1423 (HY000): Field of view 'employees.salary_view' underlying table doesn't have a default value
```

如果该表有 一个默认值，即使它不符合视图中的过滤器条件，你也可以向其中插入一行 。 为了避免这种情况，为了只允许插入符合视图条件的行，必须在定义里面提供 WITH CHECK OPTION。

VIEW算法:

- MERGE: MySQL 将输入查询和视图定义合并到一个查询中，然后执行组合查询。 仅允许在简单视图上使用 MERGE 算法 。
- TEMPTABLE: MySQL将结果存储到临时表中，然后对这个临时表执行输入查询。
- UNDEFINED: MySQL 自动选择MERGE 或 TEMPTABLE 算法。MySQL 把MERGE 算法作为首选的 TEMPTABLE 算法， 因为 MERGE 算法效率更高。

### 事件

就像 Linux 服务器上的 cron 一样， MySQL 的 EVENTS 是用来处理计划任务的 。MySQL 使用称为事件调度线程的特妹线程来执行所有预定事件。 默认情况下， 事件调度线程是未 启用(版本低于 8.0.3 )的状态，如果要启用它，执行以下命令 :

```mysql
mysql> set global event_scheduler = ON;
Query OK, 0 rows affected (0.01 sec)
```

#### 如何操作

假设你不再需要保留一个月之前的薪水审计记录， 则可以设定一个每日运行的事件，用它从 salary audit 表中删除一个月之前的记录。

```mysql
drop EVENT if exists purge_salary_audit;
delimiter $$
create EVENT if not exists purge_salary_audit
on SCHEDULE
	every 1 week
	starts current_date
	DO BEGIN
		delete from salary_audit where date_modified < date_add(curdate(), interval - 7 day);
	END;$$
delimiter ;
```

检查事件:

```mysql
show EVENTS\G
```

检查事件定义：

```mysql
show create EVENT purge_salary_audit\G
```

禁用/启用事件:

```mysql
ALTER EVENT purge_salary_audit DISABLE;
ALTER EVENT purge_salary_audit ENABLE;
```

### 获取有关数据库和表的信息

#### TABLES

例如，假设你想知道 employees 数据库中的 DATA LENGTH 、 INDEX LENGTH 和DATE FREE，代码如下:

```mysql
select 
	sum(data_length)/1024/1024 as data_size_mb, 
	sum(index_length)/1024/2014 as index_size_mb, 
	sum(data_free)/1024/1-24 as data_free_mb 
from 
	information_schema.tables 
where 
	table_schema='employees';
+--------------+---------------+----------------+
| data_size_mb | index_size_mb | data_free_mb   |
+--------------+---------------+----------------+
| 142.85937500 |    2.82025819 | 20456.00000000 |
+--------------+---------------+----------------+
1 row in set (0.00 sec)
```

#### COLUMNS

列出每个表的所有列及其定义:

```mysql
select * from columns where table_name='employees'\G
```

#### FILES

```mysql
select * from files where file_name like './employees/employees.ibd'\G
```

#### INNODB_TABLESPACES

```mysql
select * from innodb_tablespaces where name='employees/employees'\G
```

#### INNODB_TABLESTATS

```mysql
select * from innodb_tablestats where name='employees/employees'\G
```

#### PROCESSLIST

```mysql
mysql> select * from processlist\G
*************************** 1. row ***************************
     ID: 4
   USER: event_scheduler
   HOST: localhost
     DB: NULL
COMMAND: Daemon
   TIME: 3290
  STATE: Waiting for next activation
   INFO: NULL
*************************** 2. row ***************************
     ID: 21
   USER: root
   HOST: localhost
     DB: information_schema
COMMAND: Query
   TIME: 0
  STATE: executing
   INFO: select * from processlist
2 rows in set (0.00 sec)
```

```mysql
mysql> show processlist;
+----+-----------------+-----------+--------------------+---------+------+-----------------------------+------------------+
| Id | User            | Host      | db                 | Command | Time | State                       | Info             |
+----+-----------------+-----------+--------------------+---------+------+-----------------------------+------------------+
|  4 | event_scheduler | localhost | NULL               | Daemon  | 3337 | Waiting for next activation | NULL             |
| 21 | root            | localhost | information_schema | Query   |    0 | starting                    | show processlist |
+----+-----------------+-----------+--------------------+---------+------+-----------------------------+------------------+
2 rows in set (0.00 sec)
```





## 进阶使用

#### 使用JSON

使用JSON保存更多的员工信息

```mysql
create table emp_details(
	emp_no int primary key,
	details json
);
```

##### 插入JSON

```mysql
insert into emp_details(emp_no, details)
values (1,
	'{
		"location": "IN",
		"phone": "+86111111111",
		"email": "kiding@kid.com",
		"address": {
			"line1": "abc",
			"line2": "xyz street",
			"city": "Bangalore",
			"pin": "560103"
		}
	}'
);
```

##### 检索JSON

使用 -> 和 ->> 运算符检索JSON列的字段

```mysql
mysql> select emp_no, details->'$.address.pin' pin from emp_details;
+--------+----------+
| emp_no | pin      |
+--------+----------+
|      1 | "560103" |
+--------+----------+
1 row in set (0.01 sec)
```

```mysql
mysql> select emp_no, details->>'$.address.pin' pin from emp_details;
+--------+--------+
| emp_no | pin    |
+--------+--------+
|      1 | 560103 |
+--------+--------+
1 row in set (0.03 sec)
```

#### JSON函数

##### 优雅预览

```mysql
mysql> select emp_no, JSON_PRETTY(details) from emp_details\G
*************************** 1. row ***************************
              emp_no: 1
JSON_PRETTY(details): {
  "email": "kiding@kid.com",
  "phone": "+86111111111",
  "address": {
    "pin": "560103",
    "city": "Bangalore",
    "line1": "abc",
    "line2": "xyz street"
  },
  "location": "IN"
}
1 row in set (0.00 sec)
```

##### 查找

可以在 WHERE 子句中使用 col->>path 运算符来引用 JSON 的某一列 :

```mysql
select emp_no from emp_details where details->>'$.address.pin' = "560103";
```

也可以使用 ***JSON_CONTAINS*** 函数查询数据。如果找到了数据，则返回 1，否则返回 0:

```mysql
select JSON_CONTAINS(details->>'$.adress.pin', '560103') from emp_details;
```

如何查询一个 key? 假设要检查 address.line1 是否存在:

```mysql
mysql> select JSON_CONTAINS(details->>'$.adress.pin', '560103') from emp_details;
+---------------------------------------------------+
| JSON_CONTAINS(details->>'$.adress.pin', '560103') |
+---------------------------------------------------+
|                                              NULL |
+---------------------------------------------------+
1 row in set (0.01 sec)
```

这里，one表示至少应该存在一个键。

```mysql
mysql> select JSON_CONTAINS_PATH(details, 'one', "$.address.line1") from emp_details;
+-------------------------------------------------------+
| JSON_CONTAINS_PATH(details, 'one', "$.address.line1") |
+-------------------------------------------------------+
|                                                     1 |
+-------------------------------------------------------+
1 row in set (0.01 sec)
```

如果要检查 address.line1 和 address. Line5 是否同时存在，可以使用 all，而不是 one:

```mysql
mysql> select JSON_CONTAINS_PATH(details, 'all', '$.address.line1', '$.address.line5') from emp_details;
+--------------------------------------------------------------------------+
| JSON_CONTAINS_PATH(details, 'all', '$.address.line1', '$.address.line5') |
+--------------------------------------------------------------------------+
|                                                                        0 |
+--------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

##### 修改

- JSON_SET(): 天环现有值并添加不存在的值。

  假设要替换员工的pin码，并添加昵称的详细信息

  ```mysql
  update 
  	emp_details
  set
  	details = JSON_SET(details, "$.address.pin", "560100", "$.nickname", "kai")
  where
  	emp_no = 1;
  ```

- JSON_INSERT(): 插入值，但不替换现有值。

  假设你希望添加新列而不更新现有值

  ```mysql
  update
  	emp_details
  set 
  	details=JSON_INSERT(details, "$.address.pin", "560132", "$.address.line4", "A Wing")
  where
  	emp_no=1;
  ```

- JSON_REPLACE(): 仅替换现有值。

  假设只需要替换现有字段，不需要添加新字段

  ```mysql
  update 
  	emp_details
  set 
  	details=JSON_REPLACE(details, "$.address.pin", "560132", "$.adress.line5", "Landmark")
  where
  	emp_no=1;
  ```

##### 删除

JSON_REMOVE 能从 JSON文档中删除数据。

```mysql
update 
	emp_details
set
	details=JSON_REMOVE(details, "$.address.line5")
where
	emp_no=1;
```

##### 其他函数

- JSON_KEYS(): 获取JSON文档中的所有键。

  ```mysql
  select 
  	JSON_KEYS(details) 
  from 
  	emp_details
  where
  	emp_no=1;
  ```

- JSON_LENGTH(): 给出 JSON文档中的元素数。

  ```mysql
  select 
  	JSON_LENGTH(details) 
  from 
  	emp_details 
  where
  	emp_no=1;
  
  ```

### 公用表表达式(CTE)

MySQL8 支持公用表表达式 ，包括非递归和递归两种 。
公用表表达式允许使用命名的临时结果集， 这是通过允许在 SELECT 语句和某些其他语句前面使用 WITH 子句来实现的。

#### 非递归（CTE）

**公用表表达式（CTE）**与派生表类似，但它的声明会放在查询块之前，而不是FROM子句中。

##### 派生表





## 事务

### ACID属性

#### 原子性（Atomicity）

所有的SQL语句要么全部成功，要么全部失败，不会存在部分更新。

#### 一致性（Coonsistency）

事务只能以允许的方式改变受其影响的数据 。

#### 隔离性（Isolation）

同时发生的事务(并发事务)不应该导致数据库处于不一致的状态中。系统中每个事务都应该像唯一事务一样执行。 任何事务都不应影响其他事务的存在。

#### 持久性（Durability）

无论数据库或系统是否发生故障，数据都会永久保存在磁盘上，并且不会丢失。

### 执行事务

创建操作表

```mysql
create database bank;
use bank;

create table account(
	account_number varchar(10) primary key,
	balance int
);

insert into account values('A', 600), ('B', 400);
```

启动事务 ```start TRANSACTION``` 或者 ```BEGIN```

```mysql
BEGIN

select 
	balance INTO @a.bal 
from 
	account 
where 
	account_number='A';

update
	account
set
	balance=@a.bal-100 
where
	account_number='A';

select
	balance INTO @b.bal
from 
	account
where
	account_number='B';

update
	account
set
	balance=@b.bal+100
where
	account_number='B';

COMMIT;
```

如果遇到错误并希望中止事务， 可以发送 **ROLLBACK** 语句而非 **COMMIT** 语句 。

```mysql
start TRANSACTION
select balance into @a.bal from account where account_number='A';
update account set balance=@a.bal-100 where account_number='A';
select balance into @c.bal from account where account_number='C';
show warnings;
select @c.bal;
rollback;
```

**autocommit**

默认情况下，autocommit 的状态是ON，这意味着所有单独的语句一旦被执行就会 被提交，除非该语句在 BEGIN... COMMIT 块中 。 如果 autocommit 的状态为 OFF， 则需要明确发出 COMMIT 语句来提交事务 。 要禁用 autocommit ，请执行 :

```mysql
set autocommit=0;
```

DDL 语句，如数据库的 CREATE 或 DROP 语句，以及表或存储例程的 CREATE、DROP或 ALTER 语句，都是无法回滚的 。

### 使用保存点

使用保存点可以回滚到事务中的某些点，而且无须中止事务。你可以使用 SAVEPOINT 标识符为 事务设置名称，并使用 ROLLBACK TO 标识语句将事务回滚到指定的保存点而不中止事务。

```mysql
BEGIN;
select balance INTO @a.bal from account where account_number='A';
update account set balance=@a.bal-100 where account_number='A';
update account set balance=balance+100 where account_number='B';
SAVEPOINT transfer_to_b;
select balance INTO @a.bal from account where account_number='A';
update account set balance=balance+100 where account_number='C';
ROLLBACK TO transfer_to_b;
commit;
```

### 隔离级别

当两个或多个事务同时发生时，隔离级别定义了一个事务与其他事务在资源或者数据修改方面的隔离程度 。有 4 种类型的隔离级别，要更改隔离级别，需要设置 tx_isolatio口变量，该变量是动态的并具有会话级别的作用范围。

更改隔离级别 ```SET @@ transaction_islocation='READ-COMMITTED';``

#### 读取未提交（read uncommitted）





