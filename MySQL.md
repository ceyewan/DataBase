[TOC]

### 相关概念

- 客户端连接

```shell
mysql [-h 127.0.0.1] [-p 3306] -u root -p
```

- 数据模型
  1. 关系型数据库
  2. 数据模型
- 图形化界面工具
  - DataGrip

### SQL 通用语法

- 单行注释 -- 或者 #
- 多行注释 /* xxx */
- DDL 数据定义语言
- DML 数据操作语言
- DQL 数据查询语言
- DCL 数据控制语言

#### DDL 数据定义语言

```sql
# 查询所有数据库
show databases;
# 查询当前数据库
select database();
# 创建数据库
create database [if not exists] 数据库名 [default charset 字符集] [collate 排序规制]；
# 删除数据库
drop database [if exists] databaseName；
# 切换数据库
use datanaseName;

# 查询当前数据库的所有表
show tables;
# 查看指定表的结构
desc tableName;
# 查询指定表的建表语句
show create table tableName;
# 创建表
create table tableName(
	xxx xxxtype [comment 注释],
    ...
    xxx xxxtype [comment 注释]
) [comment 注释]；
# type 主要包括数值类型、字符串类型和日期时间类型
 create table emp(
     id int comment '编号',
     workno varchar(10) comment '工号',
     name varchar(10) comment '姓名',
     gender char(1) comment '性别',
     age tinyint unsigned comment '年龄',
     idcard char(18) comment '身份证号',
     entrydate date comment '入职时间'
 ) comment '员工表';
 # 添加字段
 alter table 表名 add 字段名 类型（长度） [comment 注释] [约束];
 # 修改数据类型
 alter table 表名 modify 字段名 新数据类型（长度）;
 # 修改字段名和字段类型
 alter table 表名 change 旧字段名 新字段名 类型 [comment 注释] [约束];
 # 删除字段
 alter table 表名 drop 字段名;
 # 修改表名
 alter table 表名 rename to 新表名;
 # 删除表
 drop table [if exists] 表名;
 # 删除指定表并创建新表
 truncate table 表名;
```

#### DML 数据操作语言（增删改）

```sql
# 给指定字段添加数据
insert into 表名 (字段名1，字段名2，...) value(值1，值2，值3，...);
# 全部字段
INSERT INTO 表名 VALUES (值1, 值2, ...);
# 批量添加
insert into 表名 (字段名1，字段名2，...) value(值1，值2，值3，...),(值1，值2，值3，...)...;
# 修改数据
update 表名 set 字段名1 = 值1, 字段名2 = 值2，... [where 条件];
# 删除数据
delete from 表名 [where 条件];
```

#### DQL （查）

```sql
# 执行顺序
select xxx from xxx where xxx group by xxx having xxx order by xxx limit xxx;
# 基本查询
select 字段名1,字段名2 from 表名;
select * from 表名;
# 字段设置别名
select 字段1 [as 别名1], 字段2 [as 别名2] from 表名;
# 去除重复记录
select distinct 字段列表 from 表名;
# 条件查询
select 字段列表 from 表名 where 条件列表;
# in(...),like xxx,is null,is not null ...
# 聚合函数
count(),max(),min(),avg(),sum()
# 分组查询
select 字段列表 from 表名 [where 条件] group by 分组字段名 [having 分组后过滤条件];
# where 是分组前过滤，having 是分组后对结果过滤；where 不能对聚合函数进行判断。执行顺序: where > 聚合函数 > having 
# 排序查询
select 字段列表 from 表名 order by 字段x [asc],字段2 desc;
# asc 升序（默认）；desc 降序
# 分页查询
select 字段列表 from 表名 limit 起始索引，查询记录数;
```

#### DCL （数据控制语言）

```sql
# 查询用户
select * from mysql.user;
# 创建用户
create user '用户名'@'主机名' identified by '密码';
# 修改用户密码
alter user '用户名'@'主机名' identified with mysql_native_password by '新密码';
# 删除用户
drop user '用户名'@'主机名';
# 主机名为 localhost 表示只能够在当前主机访问，% 可以在任意主机访问该数据库
# 查询权限
show grants for '用户名'@'主机名';
# 授予权限
grant 权限列表 on 数据库.表名 to '用户名'@'主机名';
# 撤销权限
remove 权限列表 on 数据库.表名 to '用户名'@'主机名';
# rant 权限列表 on *.* to '用户名'@'主机名'; * 匹配所有
# 权限列表 all/all privileges, select, insert, update, delete, alter, drop, create...
```

### 函数

#### 字符串函数

```sql
# concat 字符串拼接
select concat('Hello', 'MySQL')；
# lower 全部转小写
select lower('Hello');
# upper 全部转大写
select upper('Hello');
# lpad 左填充(填充到 5 位，用 - 来填充)
select lpad('01', 5, '-');
# rpad 右填充
# trim 去除两端空格
select trim(' Hello MySQL ');
# 截取子字符串（从 1 开始，取 5 位）
select substring('Hello MySQL', 1, 5)
```

#### 数值函数

```sql
# ceil 向上取整
select ceil(1.1);
# floor 向下取整
# mod 取模
select mod(7, 4);
# rand 获取随机数(0 - 1 之间)
select rand();
# round 四舍五入（保留两位小数）
select round(2.344, 2);

# 获得 1000000 以内的随机数，保留 0 位小数，并且补齐到 6 位
select lpad(round(rand()*1000000, 0), 6, '0');
```

#### 日期函数

```sql
# curdate 当前日期
select curdate();
# curtime 当前时期
select curtime();
# now 当前日期和时间
select now();
select year(now()), month(now()), day(now());
# date_add 增加指定的时间间隔
select date_add(now(), interval 70 year);
# datediff 时间间隔
select datediff('2022-01-01', '2022-12-01')
```

#### 流程函数

```sql
# if(value, a, b) 如果 value 为 true 则 a ，否则 b
select if(value, 'ok', 'Error');
# ifnull(value1, value2) 如果 value1 为 null 才 value2
select ifnull('Ok', 'Default');
# case when then else end
select
	name,
	(case workaddress when '北京' then '一线城市' else '二级城市' end) as '工作地址'
from emp;
```

### 约束

- 约束是作用于表中字段上的规则，用于限制在表中的数据。
- 目的是保证数据库中数据的正确、有效性和完整型
- 包括非空约束(not null)、唯一约束(unique)、主键约束(primary key)、默认约束(default)、检查约束(check)和外键约束(foreign key)

一个小演示：

```sql
create table tb_user(
	id int AUTO_INCREMENT PRIMARY KEY COMMENT 'id唯一标识',
	name varchar(10) NOT NULL UNIQUE COMMENT '姓名',
    age int CHECK(age > 0 && age < 120) COMMENT '年龄',
    status char(1) DEFAULT '1' COMMENT '状态',
    gender char(1) COMMENT '姓名'
);
```

#### 外键约束

- 外键是用来让两张表的数据之间建立连接，从而保证数据的一致性和完整性。

我们看这个例子，创建了两张表，每个人都有一个部门 ID，每个 ID 都有一个名字。如果我们在第一个表里删除部门 ID = 1 的，那么表 2 里面的人就是“我被退学了？”。这就是没有外键约束导致无法保证一致性和完整性。

```sql
create table dept(
 	id int auto_increment comment 'ID' primary key,
 	name varchar(50) not null comment '部门名称'
)comment '部门表';
 
 INSERT INTO dept (id, name) VALUES (1, '研发部'), (2, '市场部'),(3, '财务部'), (4,
'销售部'), (5, '总经办');

create table emp(
 	id int auto_increment comment 'ID' primary key,
 	name varchar(50) not null comment '姓名',
 	age int comment '年龄',
 	job varchar(20) comment '职位',
 	salary int comment '薪资',
 	entrydate date comment '入职时间',
 	managerid int comment '直属领导ID',
 	dept_id int comment '部门ID'
)comment '员工表';

INSERT INTO emp (id, name, age, job,salary, entrydate, managerid, dept_id) VALUES (1, '金庸', 66, '总裁',20000, '2000-01-01', null,5),(2, '张无忌', 20, '项目经理',12500, '2005-12-05', 1,1), (3, '杨逍', 33, '开发', 8400,'2000-11-03', 2,1),(4, '韦一笑', 48, '开发',11000, '2002-02-05', 2,1), (5, '常遇春', 43, '开发',10500, '2004-09-07', 3,1), (6, '小昭', 19, '程序员鼓励师',6600, '2004-10-12', 2,1);
```

- 添加外键

可以在创建时就添加，也可以创建完成之后添加。

```sql
create table 表名(
	[CONSTRAINT] [外键名称] FOREIGN KEY （外键字段名） REFERENCES 主表(主表列名);
);
alter table 表名 add CONSTRAINT 外键名称 FOREIGN KEY （外键字段名） REFERENCES 主表(主表列名);
# 上面的例子
alter table emp add constraint fk_emp_dept_id foreign key (dept_id) references dept(id);
# 删除外键
alter table 表名 drop foreign key 外键名称;
```

- 删除/更新行为

| 行为        | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| NO ACTION   | 当在父表中删除/更新对应记录时,首先检查该记录是否有对应外键,如果有则不允许删除/更新。 (与 RESTRICT 一致) 默认行为 |
| RESTRICT    | 当在父表中删除/更新对应记录时,首先检查该记录是否有对应外键,如果有则不允许删除/更新。 (与 NO ACTION 一致) 默认行为 |
| CASCADE     | 当在父表中删除/更新对应记录时,首先检查该记录是否有对应外键,如果有,则也删除/更新外键在子表中的记录。 |
| SET NULL    | 当在父表中删除对应记录时,首先检查该记录是否有对应外键,如果有则设置子表中该外键值为null(这就要求该外键允许取null)。 |
| SET DEFAULT | 父表有变更时,子表将外键列设置成一个默认的值 (Innodb不支持)   |

```sql
# 用法 ON UPDATE xxx ON DELETE xxx;
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段) REFERENCES 主表名 (主表字段名) ON UPDATE CASCADE ON DELETE CASCADE;
```

### 多表查询

#### 多表关系

- 一对多
  - 部门与员工，员工建立外键指向部门
- 多对多
  - 学生与课程，建立第三张表，中间表至少两个外键，分别关联两方主键。
- 一对一
  - 用户与用户，任意一方加入外键，并且设置外键为唯一的（UNIQUE）

#### 多表查询概述

```sql
# 得到笛卡尔积结果
select * from emp, dept;
# 添加连接查询条件
select * from emp, dept where emp.dept_id = dept.id;

```

- 内连接

```sql
# 隐式内连接
select 字段列表 from 表1, 表2 where 条件 ... ;
# 显式内连接
select 字段列表 from 表1 [inner] JOIN 表2 ON 连接条件;
```

- 外连接

```sql
# 左外连接，查询表1的所有信息和两个表的交集部分（如果表1中的元素在表2中找不到对应的，那么就用null填充）
select 字段列表 from 表1 LEFT [OUTER] JOIN 表2 ON 条件 ...;
select e.*, d.name from emp e left outer join dept d on e.dept_id = d.id;
# 右外连接
select 字段列表 from 表1 RIGHT [OUTER] JOIN 表2 ON 条件 ...;
```

- 自连接

```sql
# 查询员工及其所属领导的名字
select a.name , b.name from emp a , emp b where a.managerid = b.id;
# 包含没有领导的员工
select a.name '员工', b.name '领导' from emp a left join emp b on a.managerid = b.id;
```

#### 联合查询

```sql
select 字段列表 from 表A ...
UNION [ALL] -- ALL 不会去重，没有就会去重
select 字段列表 from 表B ...;
# 要求列数和类型完全一致
```

#### 子查询

- 查询中套查询

```sql
SELECT * FROM t1 WHERE column1 = ( SELECT column1 FROM t2 );
```

- 标量子查询（子查询结果为单个值）

```sql
select * from emp where dept_id = (select id from dept where name = '销售部');
```

- 列子查询（返回结果一列）

```sql
# 常用操作符 in、not in、any、some、all
select * from emp where dept_id in (select id from dept where name = '销售部' or name = '市场部');
```

- 行子查询(返回结果是一行)

```sql
# 常用操作符 =、<>、in、not in
select * from emp where (salary,managerid) = (select salary, managerid from emp where name = '张无忌');
```

- 表子查询

```sql
# 常用操作符 in
select * from emp where (job,salary) in ( select job, salary from emp where name = '鹿杖客' or name = '宋远桥' );
```

### 事务

- 事务是一组操作的集合,它是一个不可分割的工作单位,事务会把所有的操作作为一个整体一起向系统提交或撤销操作请求,即这些操作要么同时成功,要么同时失败。

```sql
# 查看/设置事务提交方式
select @@autocommit;
set @@autocommit = 0; # 手动提交
# 提交事务
COMMIT;
# 回滚事务 (事务执行失败)
ROLLBACK;
# 开启事务（或者 set @@autocommit = 0; ）
start transaction 或 start;
# 查看事务隔离级别
SELECT @@TRANSACTION_ISOLATION;
# 设置事务隔离级别（当前会话或全局）
SET [ SESSION | GLOBAL ] TRANSACTION ISOLATION LEVEL { READ UNCOMMITTED |
READ COMMITTED | REPEATABLE READ | SERIALIZABLE };
```

#### 事务的四大特性

- 原子性：不可分割的最小操作单元
- 一致性：事务完成时，所有的数据保持一致状态
- 隔离性：事务在不受外部并发操作影响的独立环境下运行
- 持久性：事务一旦提交或回滚，数据会持久存在

#### 并发事务问题

- 脏读：一个事务读取到另一个事务还没有提交的数据
- 不可重复读：一个事务先后读取同一条记录，但是读到的数据不同
- 幻读：一个事务按照条件查询时，没有对应的数据行，但是在插入数据时，发现这行数据已经存在

#### 事务隔离级别

| 隔离级别              | 脏读 | 不可重复读 | 幻读 |
| --------------------- | ---- | ---------- | ---- |
| Read uncommitted      | √    | √          | √    |
| Read committed        | ×    | √          | √    |
| Repeatable Read(默认) | ×    | ×          | √    |
| Serializable          | ×    | ×          | ×    |

> 事务隔离级别越高,数据越安全,但是性能越低。归根结底是性能与安全的平衡。

### 存储引擎

#### 体系结构

- 连接层：最上层是一些客户端和链接服务,包含本地sock 通信和大多数基于客户端/服务端工具实现的类似于TCP/IP的通信。主要完成一些类似于连接处理、授权认证、及相关的安全方案。在该层上引入了线程池的概念,为通过认证安全接入的客户端提供线程。同样在该层上可以实现基于SSL的安全链接。服务器也会为安全接入的每个客户端验证它所具有的操作权限。
- 服务层：第二层架构主要完成大多数的核心服务功能,如SQL接口,并完成缓存的查询,SQL的分析和优化,部分内置函数的执行。所有跨存储引擎的功能也在这一层实现,如 过程、函数等。在该层,服务器会解析查询并创建相应的内部解析树,并对其完成相应的优化如确定表的查询的顺序,是否利用索引等,最后生成相应的执行操作。如果是select语句,服务器还会查询内部的缓存,如果缓存空间足够大,这样在解决大量读操作的环境中能够很好的提升系统的性能。
- 引擎层：存储引擎层, 存储引擎真正的负责了MySQL中数据的存储和提取,服务器通过API和存储引擎进行通信。不同的存储引擎具有不同的功能,这样我们可以根据自己的需要,来选取合适的存储引擎。数据库中的索引是在存储引擎层实现的。
- 存储层：数据存储层, 主要是将数据(如: redolog、undolog、数据、索引、二进制日志、错误日志、查询日志、慢查询日志等)存储在文件系统之上,并完成与存储引擎的交互。

#### 存储引擎

MySQL 默认使用的存储引擎是 InnoDB。

```sql
# 建表时指定存储引擎
CREATE TABLE 表名(
	xxx
) ENGINE = INNODB [ COMMENT 表注释 ];
# 查询当前数据库支持的存储引擎
show engines;
```

#### InnoDB 特点

InnoDB是一种兼顾高可靠性和高性能的通用存储引擎,在 MySQL 5.5 之后,InnoDB是默认的 MySQL 存储引擎。

- DML操作遵循ACID模型,支持事务;
- 行级锁,提高并发访问性能;
- 支持外键FOREIGN KEY约束,保证数据的完整性和正确性;

文件：

```sql
 # 查看是否每个表一个文件
 show variables like 'innodb_file_per_table';
 # 查看文件存放目录(里面不同的文件夹代表不同的数据库)
 show variables like 'datadir';
```

存储逻辑：

- 表空间：InnoDB 存储逻辑结构的最高层，包含多个 segment
- 段：包括数据段、索引段、回滚段等，由引擎完成对段的管理
- 区：是表空间的单元结构。每个区大小为 1 M，一个区有 64 个页
- 页：最小单元，默认 16KB 。
- 行：InnoDB 存储引擎是面向行的，数据按照行存放

### 索引

> 索引(index)是帮助MySQL高效获取数据的数据结构(有序)。在数据之外,数据库系统还维护着满足特定查找算法的数据结构,这些数据结构以某种方式引用(指向)数据, 这样就可以在这些数据结构上实现高级查找算法,这种数据结构就是索引。

如果没有索引，那么查询需要 `O(n)` 的时间复杂度，如果我们使用二叉树的索引结构建立索引，查询复杂度就会是 `O(logn)` 。

索引就是一种典型的空间换时间的方法，并且减少了查询时间，增加了增删改的效率。

#### B-Tree

B树是一种多叉路衡查找树,相对于二叉树,B树每个节点可以有多个分支,即多叉。以一颗最大度数(max-degree)为5(5阶)的b-tree为例,那这个B树每个节点最多存储4个key,5个指针。

#### B+Tree

非子节点不存储数据，或者说不唯一存储数据，所有的数据都存储在子节点中，非子节点的数据只是用来导航，索引数据。并且所有的子节点形成一个有序的单向链表。

#### Hash

哈希不支持范围查询，但是用于对等判断很快。

#### 为什么 InnoDB 存储引擎选择使用 B+Tree ？

- 相对于二叉树，层级更少，搜索效率高
- 相对于 B-Tree，B+Tree 的数据都存储在子节点中，子节点中是没有指针的，那么相同数量的数据，B+Tree 访问的节点少，也就是 I/O 操作少，效率更高。（一般来说，一个节点大小为一页，大致可以有 1000 个节点（指针大小为 6 字节））。
- 相比于 Hash 表，支持范围查询及排序操作

#### 索引分类

- 主键索引：默认创建，只有一个，value 保存所有信息
- 唯一索引：避免值重复
- 常规索引：快速定位特定数据
- 全文索引：查找的是文本中的关键词

- 聚集索引：value 保存行数据，只有一个，如果有主键就是主键
- 二级索引：value 为主键，然后再去主键索引查找元素（这种两次查询称为回表查询）

#### 索引语法

```sql
# 创建索引
create [unique | fulltext] index index_name on table_name (index_col_name,...);
# 查看索引
show index from table_name;
# 删除索引
drop index index_name on table_name;

# 常规索引
CREATE INDEX idx_user_name ON tb_user(name);
# 联合索引
CREATE INDEX idx_user_pro_age_sta ON tb_user(profession,age,status);
```

#### 性能分析

```sql
# 查看 sql 执行频率
show [global | session] status like 'Com_______';
# 根据频率决定索引，索引越多，增删改越慢，查越快

# 慢查询日志是否开启
show variables like 'slow_query_log';
# 开启慢查询日志
vim /etc/my.conf # 我是在 /etc/mysql/my.conf 中
    slow_query_log=1
    long_query_time=2 # 两秒算慢
systemctl restart mysqld
# 查看慢日志文件中的记录
cat /var/lib/mysql/localhost-slow.log

# 查看是否支持 profiles 操作
select @@have_profiling;
# 查看是否开启
select @@profiling;
# 设置
SET profiling = 1;
# 查看每一条 sql 的耗时基本情况
show profiles;
# 查看特定 query_id 的 sql 各个阶段的耗时
show profile for query query_id;
# 查看指定 query_id 的 sql 语句 CPU 的使用情况
show profile cpu for query query_id;

# explain 或者 desc 命令获取 mysql 如何执行 select 语句的信息
EXPLAIN SELECT 字段列表 FROM 表名 WHERE 条件;
```

#### 索引使用

```sql
# \G 竖着显示
select * from tb_sku where id = 1\G;
# 建立索引后查询就会很快，否则，就会很慢
# 最左前缀法则，联合索引只有最左边的部分可以走索引
```

#### 范围查询

```sql
# status 不走索引
explain select * from tb_user where profession = '软件工程' and age > 30 and status = '0';
```

#### 索引失效

```sql
# 不满足最左前缀法则
explain select * from tb_user where substring(phone,10,2) = '15';
# 字符串不加引号
explain select * from tb_user where phone = 18888888888;
# or 连接条件，都有索引，才会生效
# mysql 自己评估全局扫描更快
```

#### 指定索引

```sql
# use index 建议 mysql 使用索引 
explain select * from tb_user use index(idx_user_pro) where profession = '软件工程';
# ignore index 忽略指定的索引
# force index 强制使用索引
```

#### 覆盖索引

覆盖索引是指查询使用了索引，并且需要返回的列,在该索引中已经全部能够找到。（避免回表查询）

> 思考题:
>
> 一张表, 有四个字段 `(id, username, password, status)`,  由于数据量大, 需要对以下 SQL 语句进行优化, 该如何进行才是最优方案:
> `select id, username, password from tb_user where username = 'itcast';`
> 答案: 针对于 `username, password` 建立联合索引, SQL 语句为: 
> `create index idx_user_name_pass on tb_user(username,password);`
> 这样可以避免上述的 SQL 语句,在查询的过程中,出现回表查询。

#### 前缀索引

当为字符串等建立索引时，key 会很大，为了节省空间，我们可以使用前缀索引，取前 x 个字符建立索引，满足最左前缀原则，并且可以节省空间。x 同样需要在时间和空间的平衡中抉择。

#### 索引设计原则

- 为数据量大，查询频繁的表建立索引
- 针对常作为查询条件、排序、分组操作的字段建立索引
- 尽量选择区分度高的列作为索引，尽量建立唯一索引，区分度越高,使用索引的效率越高
- 尽量使用联合索引，减少单列索引，查询时，联合索引很多时候可以覆盖索引，节省存储空间，避免回表，提高查询效率
- 如果是字符串类型的字段，字段的长度较长，可以针对于字段的特点，建立前缀索引
- 如果索引列不能存储NULL值，在创建表时使用NOT NULL约束它

### SQL 优化

#### 插入数据

```sql
# 批量插入
insert into tb_test values(xxx),(xxx),(xxx)；
# 手动控制事务
start transaction;
insert xxx
insert xxx
commit;
# 主键顺序插入，性能更高

# 大批量插入数据
# 客户端连接服务器时，加上参数 --local-infile
mysql --local-infile -u root -p
# 设置全局参数 local_infile=1
set global local-infile = 1;
# 执行 load 加载数据
load data local infile 'root/sqll.log' into table tb_user fields terminated by ',' lines terminated by '\n';
```

#### 主键优化

当乱序插入的时候，就会造成大量的页分裂（回想一下 B+Tree 的结构），删除的时候会造成页合并。

因此，尽量选择顺序插入，使用 AUTO_INCREMENT 自增主键。

#### order by 优化

- Using filesort ：先查询，然后在缓冲区排序
- Using index：通过有序索引查找后直接返回排序结果

```sql
# 索引
create index idx_user_age_phone_ad on tb_user(age asc ,phone desc);
# 这样就能 Using index 排序了
explain select id,age,phone from tb_user order by age asc , phone desc ;
```

优化原则：

1. 根据排序字段建立合适的索引,多字段排序时,也遵循最左前缀法则。
2. 尽量使用覆盖索引。
3. 多字段排序, 一个升序一个降序,此时需要注意联合索引在创建时的规则(ASC/DESC)。
4. 如果不可避免的出现filesort,大数据量排序时,可以适当增大排序缓冲区大小 sort_buffer_size(默认256k)

#### group by 优化

1. 在分组操作时,可以通过索引来提高效率。
2. 分组操作时,索引的使用也是满足最左前缀法则的。

#### limit 优化

当在进行分页查询时,如果执行 limit 2000000,10 ,此时需要MySQL排序前2000010 记录,仅仅返回 2000000 - 2000010 的记录,其他记录丢弃,查询排序的代价非常大 。

优化思路: 一般分页查询时,通过创建覆盖索引能够比较好地提高性能,可以通过覆盖索引加子查询形式进行优化。

```sql
explain select * from tb_sku t, (select id from tb_sku order by id limit 2000000, 10) a where t.id = a.id;
```

#### count 优化

$count(字段) < count(主键 id) < count(1) ≈ count(*)$

因为后两种不仅要遍历，还要把数据拿出来。

#### update 优化

```sql
# 行锁
update course set name = 'javaEE' where id = 1;
# 表锁
update course set name = 'javaEE' where name = 'PHP';
```

> InnoDB 的行锁是针对索引加的锁，不是针对记录，并且该索引不能失效，否则会从行锁升级为表锁。

### 视图

视图（View）是一种虚拟存在的表。视图中的数据并不在数据库中实际存在，行和列数据来自定义视图的查询中使用的表，并且是在使用视图时动态生成的。

通俗的讲，视图只保存了查询的 SQL 逻辑，不保存查询结果。所以我们在创建视图的时候，主要的工作就落在创建这条 SQL 查询语句上。

#### 语法

1. 创建

```sql
CREATE [OR REPLACE] VIEW 视图名称[(列名列表)] AS SELECT语句 [ WITH [ CASCADED | LOCAL ] CHECK OPTION ]

create or replace view stu_v_1 as select id,name from student where id <= 10;
```

2. 查询

```sql
查看创建视图语句：
SHOW CREATE VIEW 视图名称; 
查看视图数据：
SELECT * FROM 视图名称 ...... ;

show create view stu_v_1; 
select * from stu_v_1; 
select * from stu_v_1 where id < 3;
```

3. 修改

```sql
方法一：使用创建语句覆盖
方法二：
ALTER VIEW 视图名称[(列名列表)] AS SELECT语句 [ WITH [ CASCADED | LOCAL ] CHECK OPTION ]

alter view stu_v_1 as select id,name from student where id <= 10;
```

4. 删除

```sql
DROP VIEW [IF EXISTS] 视图名称 [,视图名称] ...

drop view if exists stu_v_1;
```

我们可以直接在视图中执行插入修改等操作，对数据库生效，能否在视图中看到看是否满足 WHERE 等条件。

#### 检查选项

当使用WITH CHECK OPTION子句创建视图时，MySQL 会通过视图检查正在更改的每个行，例如插入，更新，删除，以使其符合视图的定义。 MySQL 允许基于另一个视图创建视图，它还会检查依赖视图中的规则以保持一致性。为了确定检查的范围，MySQL 提供了两个选项： CASCADED 和 LOCAL，默认值为CASCADED 。

1. CASCADED 级联

比如，v2视图是基于v1视图的，如果在v2视图创建的时候指定了检查选项为 cascaded，但是v1视图创建时未指定检查选项。 则在执行检查时，不仅会检查v2，还会级联检查v2的关联视图v1。

2. LOCAL 本地

不进行递归，只检查当前语句。

#### 视图的更新

要使视图可更新，视图中的行与基础表中的行之间必须存在一对一的关系。因此如果包含聚合函数、DISTINCT、GROUP BY 等，视图不可更新。

#### 视图的作用

1. 简单

可以将复杂查询（如多表联查）存在一个视图中，方便操作。

2. 安全

数据库可以授权，但不能授权到数据库特定行和特定的列上。通过视图用户只能查询和修改他们所能见到的数据。

3. 数据独立

视图可帮助用户屏蔽真实表结构变化带来的影响。

### 存储过程

存储过程是事先经过编译并存储在数据库中的一段 SQL 语句的集合，调用存储过程可以简化应用开发人员的很多工作，减少数据在数据库和应用服务器之间传输，对于提高数据处理的效率是有好处的。

存储过程思想上很简单，就是数据库 SQL 语言层面的代码封装和重用。

特点：

- 封装，复用：可以把某一业务 SQL 封装在存储过程中，需要用到的时候直接调用即可。
- 可以接收参数，也可以放回数据
- 减少网络交互，效率提升。封装的多条 SQL 语句只需要一个网络交互

#### 基本语法

```sql
# 创建
create procedure 存储过程名称([ 参数列表 ])
begin
	-- SQL 语句
end;
# 调用
call 名称 ([ 参数列表 ]);
# 查看
-- 查询指定数据库的存储过程及状态信息
select * from information_schema.routines where routine_schema = 'xxx';
-- 查询某个存储过程的定义
show create procedure 存储过程名称;
# 删除
drop procedure [if exists] 存储过程名称;
```

例如：

```shell
# 终端中需要更改结束符
mysql> delimiter //
mysql> create procedure p1()
    -> begin
    -> select count(*) from student;
    -> end
    -> //
Query OK, 0 rows affected (0.01 sec)
mysql> delimiter ;
mysql> call p1();
```

#### 变量

- 系统变量：MySQL 服务器提供的，不是用户定义的，属于服务器层面，主要分为全局变量（GLOBAL）、会话（SESSION）
- 用户定义变量：用户定义变量是用户根据需要自己定义的变量，用户变量不用提前声明，在用的时候直接用 "@变量
  名" 使用就可以。其作用域为当前连接。
- 局部变量：是根据需要定义的在局部生效的变量，访问之前，需要DECLARE声明。可用作存储过程内的局部变量和输入参数，局部变量的范围是在其内声明的BEGIN ... END块。

#### if

用于做条件判断

```sql
create procedure p()
begin
	declare score int default 58;
	declare result varchar(10);
	
	if score >= 85 then
		set result := '优秀';
	elseif score >= 60 then
		set result := '及格';
	else
		set result := '不及格';
	endif;
	select result;
end;

call p();
```

#### 参数

| 类型  | 含义                                         | 备注 |
| ----- | -------------------------------------------- | ---- |
| IN    | 该类参数作为输入，也就是需要调用时传入值     | 默认 |
| OUT   | 该类参数作为输出，也就是该参数可以作为返回值 |      |
| INOUT | 既可以作为输入也可以作为输出                 |      |

```sql
create procedure 存储过程名称 ([IN/OUT/INOUT 参数名 参数类型])
begin 
	xxx
end;
```

例如

```sql
create procedure p(in score int, out result varchar(10))
begin
	if score >= 85 then
		set result := '优秀';
	elseif score >= 60 then
		set result := '及格';
	else
		set result := '不及格';
	endif;
end;

call p(18, @result);

select @result;
```

#### case

```sql
case [case_value]
	when xxx then xxx
	[when xxx then xxx] ...
	[else xxx]
end case;
```

#### while

```sql
while 条件 do
	xxx
end while;

create procedure p(in n int)
bagin
	declare total int default 0;
	
	while n > 0 do
		set total := total + n;
		set n := n - 1;
	end while;
	
	select total;
end;

call p(100);
```

#### repeat

```sql
repeat
	xxx
	until 条件
end repeat;
```

#### loop

```sql
[begin_label:] loop
	xxx
end loop [end_label];

leave label; -- 退出指定标记的循环体
iterate label; -- 直接进入下一次循环
```

#### 游标

游标时用来存储查询结果集的数据类型，在存储过程和函数中可以使用游标对结果集进行循环的处理。

```sql
# 声明游标
declare 游标名称 cursor for 查询语句;
# 打开游标
open 游标名称
# 获取游标记录
fetch 游标名称 into 变量 [, 变量];
# 关闭游标
close 游标名称;
# 这个东西就类似于通信的管道一下，查询语句结果写入游标，然后从别的地方获取游标记录
```

#### 条件处理程序

用来定义在流程控制结构执行过程中遇到问题时相应的处理步骤，类似于一种异常处理。

### 存储函数

存储函数是有返回值的存储过程，存储函数的参数只能是 in 类型的：

```sql
create function 存储函数名称([参数列表])
returns type [characteristic...]
begin
	-- SQL 语句
	return ...;
end;
```

characteristic 说明:

- DETERMINISTIC：相同的输入参数总是产生相同的结果
- NO SQL ：不包含 SQL 语句。
- READS SQL DATA：包含读取数据的语句，但不包含写入数据的语句

例如，从 1 累加到 n：

```sql
create function func(n int)
returns int deterministic
begin
	declare total int default 0;
	while n > 0 do
		set total := total + n;
		set n := n - 1;
	end while;
	return total;
end;

select func(50);
```



### 触发器

触发器是与表有关的数据库对象，指在 insert/update/delete 之前(BEFORE)或之后(AFTER)，触发并执行触发器中定义的SQL语句集合。触发器的这种特性可以协助应用在数据库端确保数据的完整性，日志记录，数据校验等操作 。使用别名OLD和NEW来引用触发器中发生变化的记录内容。

| 触发器类型 | NEW和OLD                                               |
| ---------- | ------------------------------------------------------ |
| INSERT型   | new 表示将要或已经新增的数据                           |
| UPDATE型   | old 表示修改之前的数据，new 表示将要或已经修改后的数据 |
| DELETE型   | old 表示将要或已经删除的数据                           |

触发器就是可以在你执行特定的 sql 语句时，它自动在你执行之前或之后执行一段语句，这样就可以用于日志、备份等操作了。

```sql
# 创建触发器
create trigger trigger_name
before/after insert/update/delete
on table_name for each row -- 行级触发器
begin
	trigger_stmt;
end;
# 查看
show triggers;
# 删除(没有指定 schema 就默认当前数据库)
drop trigger [schema_name.]trigger_name;
```

### 锁

锁是计算机协调多个进程或线程并发访问某一资源的机制，用于保证数据并发访问的一致性。

#### 全局锁

对整个数据库实例加锁，加锁后是只读状态，后续 DML、DDL 语句，更新操作的事务提交语句都将被阻塞。

典型使用场景是全库逻辑备份，对所有的表进行锁定，从而获得一致性视图，保证数据完整性。

```sql
# 加全局锁
flush tables with read lock;
# 数据备份
mysqldump -u root -p xxxx itcast > itcast.sql;
# 释放锁
unlock tables;
```

- 如果在主库上备份，那么备份期间不能执行更新，业务停摆
- 如果在从库上备份，那么备份期间不能执行主库同步过来的二进制日志，会导致主从延迟

```sql
# --single-transaction
mysqldump --single-transaction -u root -p xxxx itcast > itcast.sql;
```

#### 表级锁

每次操作锁住整张表。锁定粒度大，发生锁冲突的概率最高，并发度最低。（全局表是无法操作）

- 表锁
  - 表共享读锁（read lock）
  - 表独占写锁（read lock）

```sql
# 加锁
lock tables 表名 read/write；
# 释放锁
unlock tables/客户端断开连接
```

- 元数据锁
  - meta data lock 元数据锁，简写 MDL
  - 系统自动控制，无需显式使用。在访问一张表的时候会自动加上。MDL锁主要作用是维护表元数据的数据一致性，在表上有活动事务的时候，不可以对元数据进行写入操作。为了避免DML与DDL冲突，保证读写的正确性。
  - 对一张表进行增删改查时，加 MDL 读锁（共享），因为这些不会改变表结构；对表结构进行变更操作时，加 MDL 写锁（排他）。

```sql
# 查看元数据锁的情况
select object_type, object_schema, object_name, lock_type, lock_duration from performance_schema.metadata_locks;
```

- 意象锁	
  - 为了避免 DML 在执行时，加的行锁与表锁的冲突，引入了意向锁，使得表锁不用检查每行数据是否加锁，使用意向锁来减少表锁的检查
  - 客户端一在执行 DML 时，会对涉及的行加行锁，同时也会对表加上意向锁。客户端二和根据表上的意向锁来决定自己能否成功加表锁，而不需要逐行判断。

> 意向共享锁，与表锁共享锁兼容，与表锁排它锁互斥。
>
> 意向排他锁，与表锁都互斥。意向锁之间不互斥。
>
> 事务提交后，意向锁自动释放。

```sql
# 查看意向锁及行锁的加锁情况
select object_schema, object_name, lock_type, lock_mode, lock_data from performance_schema.data_locks;
```

#### 行锁

- 共享锁、排它锁

- 默认情况下，InnoDB在 REPEATABLE READ事务隔离级运行，InnoDB使用 next-key 锁进行搜索和索引扫描，以防止幻读。
- 针对唯一索引进行检索时，对已存在的记录进行等值匹配时，会自动优化为行锁
- InnoDB 的行锁是针对索引加的锁，不通过索引条件检索数据，那么 InnoDB 将对所有记录加锁，就会升级为表锁。

```sql
# 查看意向锁和行锁的加锁情况
select object_schema, object_name, lock_type, lock_mode, lock_data from performance_schema.data_locks;
# 普通的 select 语句不会加锁
# select 加共享锁
select ... lock in share mode;
# 共享锁与排它锁互斥
# 排它锁和排它锁互斥
# 更新操作就会自动加排它锁
# 无索引行锁升级为表锁
# 事务根据没有索引的字段更新，就会升级为表锁
```

- 间隙锁&临键锁
  - 索引上的等值查询(唯一索引)，给不存在的记录加锁时，优化为间隙锁 。
  - 索引上的等值查询(非唯一普通索引)，向右遍历时最后一个值不满足查询需求时，next-key lock 退化为间隙锁。
  - 索引上的范围查询(唯一索引)，会访问到不满足条件的第一个值为止。

> 间隙锁唯一目的是防止其他事务插入间隙。间隙锁可以共存，一个事务采用的间隙锁不会阻止另一个事务在同一间隙上采用间隙锁。
