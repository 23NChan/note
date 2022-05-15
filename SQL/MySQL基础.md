  

## SQL

### SQL通用语法

1. SQL语句可一单行或多行书写，以分号结尾
2. SQL语句可以使用空格/缩进来增强语句的可读性
3. MySQL数据库的SQL语句不区分大小写，关键字建议使用大写
4. 注释：
   1. 单行注释：--注释内容 或 #注释内容(MySQL特有)
   2. 多行注释： /* 注释内容 */

### SQL分类

| 分类 | 全程                       | 说明         |
| ---- | -------------------------- | ------------ |
| DDL  | Data Definition Language   | 数据定义语言 |
| DML  | Data Manipulation Language | 数据操作语言 |
| DQL  | Data Query Language        | 输出查询语言 |
| DCL  | Data Control Language      | 数据控制语言 |

### DDL数据定义语言

#### DDL数据库操作

**查询**

~~~sql
查询所有数据库
show databases;
查询当前数据库
select database();
~~~

**创建**

```sql
create database [if not exists] 数据库名 [default charset 字符集] [collate 排序规则];
```

**删除**

```sql
drop database [if exists] 数据库名;
```

**使用**

```sql
use 数据库名;
```

#### DDL表操作

**查询**

```sql
查询当前数据库所有表
show tables;
查询表结构
desc 表名;
查询指定表的建表语句
show create table 表名;
```

**创建**

``` sql
create table 表名(
	字段1 字段1类型[comment 字段1注释],
	字段2 字段2类型[comment 字段2注释],
    ......
   	字段n 字段n类型[comment 字段n注释]
)[comment 表注释];
```

**数据类型**

主要分为三类：数值类型、字符串类型、日期时间类型

数值类型

| 类型         | 大小   | 有符号范围                             | 无符号范围                   | 描述         |
| ------------ | ------ | -------------------------------------- | ---------------------------- | ------------ |
| tinyint      | 1byte  | -128，127                              | 255                          | 小整数值     |
| smallint     | 2bytes | -32768,32767                           | 65535                        | 小整数值     |
| mediumint    | 3bytes | -8388608,8388607                       | 16777215                     | 小整数值     |
| int或integer | 4bytes | -2147483648,2147483647                 | 4294967295                   | 整数值       |
| bigint       | 8bytes | -2^63 ,2^63-1                          | 2^64-1                       | 大整数值     |
| float        | 4bytes | -3.402823466 E+38, 3.402823466351 E+38 | 1.17... E-38, 3.40... E+38   | 单精度浮点数 |
| double       | 8bytes | -1.79... E+308, 1.79... E+308          | 2.22... E-308, 1.79... E+308 | 双精度浮点数 |
| decimal      |        | 依赖于M(精度)和D(标度)的值             |                              | 小数值       |

字符串类型

| 类型       | 大小       | 描述                         |
| ---------- | ---------- | ---------------------------- |
| char       | 255        | 定长字符串                   |
| varchar    | 65535      | 变长字符串                   |
| tinyblob   | 255        | 不超过255个字符的二进制数据  |
| tinytext   | 255        | 短文本字符串                 |
| blob       | 65535      | 二进制形式的长文本数据       |
| text       | 65535      | 长文本数据                   |
| mediumblob | 16777215   | 二进制形式的中等长度文本数据 |
| mediumtext | 16777215   | 中等长度文本数据             |
| longblob   | 4294967295 | 二进制形式的极大文本数据     |
| longtext   | 4294967295 | 极大文本数据                 |

日期时间类型

| 类型      | 大小 | 范围                                     | 格式                | 描述                     |
| --------- | ---- | ---------------------------------------- | ------------------- | ------------------------ |
| date      | 3    | 1000-01-01，9999-12-31                   | YYYY-MM-DD          | 日期值                   |
| time      | 3    | -838:59:59,838:59:59                     | HH:MM:SS            | 时间值或持续时间         |
| year      | 1    | 1901,2155                                | YYYY                | 年份值                   |
| datetime  | 8    | 1000-01-01 00:00:00, 999-12-31 23:59:59  | YYYY-MM-DD HH:MM:SS | 混合日期和时间值         |
| timestamp | 4    | 1970-01-01 00:00:01, 2038-01-19 03:14:07 | YYYY-MM-DD HH:MM:SS | 混合日期和时间值，时间戳 |

**修改**

``` sql
添加字段
alter table 表名 add 字段名 类型(长度) [comment 注释] [约束];
修改数据类型
alter table 表名 modify 字段名 新数据类型(长度);
修改字段名和字段类型
alter table 表名 change 旧字段名 新字段名 类型[长度] [comment 注释] [约束];
删除字段
alter table 表名 drop 字段名;
修改表名
alter table 表名 rename to 新表名;
删除表
drop table [if exists] 表名;
删除指定表，并重新创建该表
truncate table 表名;tur
```

### DML数据操作语言

#### 添加数据(insert)

``` sql
1、为指定字段添加数据
insert into 表名 (字段名1,字段名2,...) values (值1,值2,...);
2、为全部字段添加数据
insert into 表名 values (值1,值2,...);
3、批量添加数据
insert into 表名 (字段名1,字段名2,...) values (值1,值2,...),(值1,值2,...),(值1,值2,...),...;
insert into 表名 values (值1,值2,...),(值1,值2,...),(值1,值2,...),...;
```

#### 修改数据(update)

``` sql
update 表名 set 字段名1=值1, 字段名2=值2, ...[where 条件];
```

#### 删除数据(delete)

``` sql
delete from 表名 [where 条件];
```

### DQL 数据查询语句

**DQL语法**

``` sql
select 
	字段列表
from
	表名列表
where
	条件列表
group by
	分组字段列表
having
	分组后条件列表
order by
	排序字段列表
limit
	分页参数

```

+ 基本查询

  + ``` sql
    1、查询多个字段
    select 字段1, 字段2, ... from 表名;
    2、设置别名
    select 字段1 [as 别名1], 字段2 [as 别名2] ... from 表名；
    3、去除重复记录
    select distinct 字段列表 from 表名;
    ```

+ 条件查询：where

  + | 比较运算符       | 功能                                   |
    | ---------------- | -------------------------------------- |
    | between...and... | 在某个范围之内(含最小、最大值)         |
    | in(...)          | 在in之后的列表中的值，多选一           |
    | like 占位符      | 模糊匹配(_匹配单个字符，%匹配多个字符) |
    | is null          | 是null                                 |

  + | 逻辑运算符 | 功能                 |
      | ---------- | -------------------- |
      | and 或  && | 多个条件同时成立     |
      | or 或 \|\| | 多个条件任意一个成立 |
      | not 或 !   | 非，不是             |

    

+ 聚合查询：count、max、min、avg、sum

  + 将一列数据作为一个整体，纵向计算，**所有null值不参与计算**

  + ``` sql
    select 聚合函数(字段列表) from 表名;
    ```

  + | 函数  | 功能     |
    | ----- | -------- |
    | count | 统计数量 |
    | max   | 最大值   |
    | min   | 最小值   |
    | avg   | 平均值   |
    | sum   | 求和     |

+ 分组查询：group by

  + ``` sql
    select 字段列表 from 表名 [where 条件] group by 分组字段名 [having 分组后过滤条件];
    ```

+ 排序查询：order by

  + ``` sql
    select 字段列表 from 表名 order by 字段1 排序方式1 ,字段2 排序方式2, ...;
    ```

  + asc：升序(默认值)

  + desc：降序

+ 分页查询：limit

  + ``` sql
    select 字段列表 from 表名 limit 起始索引, 查询记录数;
    ```

  + 起始索引从0开始，起始索引=(查询页码-1) * 每页显示记录数

  + 分页查询是数据库的方言，不同的数据库有不同的实现，MySQL中是limit

  + 如果查询的是第一页数据，起始索引可以省略，直接简写为limit 10

### DCL数据控制语言

#### DCL管理用户

``` sql
1、查询用户
use mysql;
select * from user;
2、创建用户
create user '用户名'@'主机名' identified by '密码';
3、修改用户密码
alter user '用户名'@'主机名' identified with mysql_native_password by '新密码';
4.删除用户
drop user ' 用户名'@'主机名';
```

#### DCL权限控制

| 权限                | 说明               |
| ------------------- | ------------------ |
| all, all privileges | 所有权限           |
| select              | 查询数据           |
| insert              | 插入数据           |
| updata              | 修改数据           |
| delete              | 删除数据           |
| alter               | 修改表             |
| drop                | 删除数据库/表/视图 |
| create              | 创建数据库/表      |

``` sql
1、查询权限
show grants for '用户名'@'主机名';
2、授予权限
grant 权限列表 on 数据库.表名 to '用户名'@'主机名';
3、撤销权限
revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';
```

## 函数

### 字符串函数

| 函数                     | 说明                          |
| ------------------------ | ----------------------------- |
| concat(S1,S2,Sn)         | 字符串拼接                    |
| lower(str)               | 将字符串全部传为小写          |
| uppper(str)              | 将字符串全部转为大写          |
| lpad(str,n,pad)          | 左填充，用pad将str填充到n长度 |
| rpad(str,n,pad)          | 右填充，用pad将str填充到n长度 |
| trim(str)                | 去掉字符串前后空格            |
| substring(str,start,len) | 截取字符串                    |

### 数值函数

| 函数       | 顺明                              |
| ---------- | --------------------------------- |
| ceil(x)    | 向上取整                          |
| floor(x)   | 向下取整                          |
| mod(x,y)   | 返回x/y的余数                     |
| rand()     | 返回0~1的随机数                   |
| round(x,y) | 求参数x的 四舍五入值，保留y位小数 |

### 日期函数

| 函数                              | 说明                                              |
| --------------------------------- | ------------------------------------------------- |
| curdate()                         | 返回当前日期                                      |
| curtime()                         | 返回当前时间                                      |
| now()                             | 返回当前日期和时间                                |
| year(date)                        | 获取指定date的年份                                |
| month(date)                       | 获取指定date的月份                                |
| day(day)                          | 获取指定date的日期                                |
| date_add(date,interval expr type) | 返回一个日期/时间增加上一个时间间隔expr后的时间值 |
| datediff(date1,date2)             | 返回起始时间date1和结束时间date2之间的天数        |

### 流程函数

| 函数                                                       | 说明                                                  |
| ---------------------------------------------------------- | ----------------------------------------------------- |
| if(value,t,f)                                              | 如果value为true则返回t，否则返回f                     |
| if null(value1,value2)                                     | 如果value1不为空，返回value1,否则返回value2           |
| case when [val1] then[res1] ... else [default] end         | 如果val1为true，返回res1，...否则返回default默认值    |
| case [expr] when [val1] then [res1] ... else [default] end | 如果expr的值等于val1，返回res1，否则返回default默认值 |

##  约束

概念：约束是作用于表中字段上的规则，用于限制存储在表中的数据

目的：保证数据库中数据的正确性、有效性和完整性

| 约束                     | 描述                                                     | 关键字      |
| ------------------------ | -------------------------------------------------------- | ----------- |
| 非空约束                 | 限制该字段的数据不能为null                               | not null    |
| 唯一约束                 | 保证该字段的所有数据都是唯一、不重复的                   | unique      |
| 主键约束                 | 主键是一行数据的唯一标识，要求非空且唯一                 | primary key |
| 默认约束                 | 保存数据时，如果未指定该字段的值，采用默认值             | default     |
| 检查约束(8.0.16版本之后) | 保证字段值满足某一个条件                                 | check       |
| 外键约束                 | 用来让两张表的数据之间建立连接，保证数据的一致性和完整性 | foreign key |

``` sql
添加外键：
create table 表名(
	字段名 数据类型,
    ...
    [constraint] [外键名称] foreign key(外键字段名) references 主表(主表字段名)
);

alter table 表名 add constraint 外键名称 foreign key(外键字段名) references 主表(主表字段名) on update [行为] on delete [行为];
删除外键：
alter table 表名 drop foreign key 外键名称;
```

### 外键约束

| 行为        | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| no action   | 父表 删除/更新，检索记录是否有对应外键，有则不允许删除/更新(与restrict一致) |
| restrict    | 父表 删除/更新，检索记录是否有对应外键，有则不允许删除/更新(与no action一致) |
| cascade     | 父表 删除/更新，检索记录是否有对应外键，有则，也删除/更新外键在子表中的记录 |
| set null    | 父表删除，检索记录是否有对应的外键，有则，设置子表中该外键值为null(这里要求该外键允许取null) |
| set default | 父表有变更时，子表将外键设置成一个默认的值(Innodb不支持)     |

## 多表查询

### 多表关系

**一对多**

案例：部门与员工的关系

关系：一个部门对应多个员工，一个员工对应一个部门

实现：在多的一方建立外键，指向一的一方主键

**多对多**

案例：学生与课程的关系

关系：一个学生可以选修多门课程，一门课程也可以提供多个学生选择

实现：建立第三张中间表，中间表至少包含两个外键，分别关联两方主键

**一对一**

案例：用户与用户详情的关系

关系：一对一关系，多用于单表拆分，将一张表的基础字段放在一张表中，其他详情字段放在另一张表中，以提升操作效率

实现：在任意一方加入外键，关联另外一方的主键，并且设置外键唯一的(unique)

### 内连接

相当于查询A、B交集部分数据

``` sql
-- 隐式内连接
select 字段列表 from 表1, 表2 where 条件 ...;
-- 显式内连接
select 字段列表 from 表1 [inner] join 表2 on 连接条件 ...;
```



### 外连接

左外连接：查询左表所有数据，以及两张表交集部分数据

右外连接：查询右表所有数据，以及两张表交集部分数据

``` sql
左外连接
select 字段列表 from 表1 left [outer] join 表2 on 条件 ...;
右外连接
select 字段列表 from 表1 right [outer] join 表2 on 条件 ...;
```

### 自连接

当前表与自身的连接查询，自连接必须使用表别名

``` sql
select 字段列表 from 表A 别名A join 表A 别名B on 条件... ;
```

### 联合查询 union, union all

对于union查询，就是把多次查询的结果合并起来，形成一个新的查询结果

``` sql
select 字段列表 from 表A ...
union [all]
select 字段列表 from 表B ...;
```

+ 对于联合查询的多张表的列数必须保持一致，字段类型也需要保持一致
+ union all 会将全部的数据直接合并在一起，union会对合并之后的数据去重

### 子查询

概念：SQL语句中嵌套select语句，称为嵌套查询，又称子查询

``` sql
select * from t1 where column1 = (select column1 from t2);
```

子查询外部的语句可以是insert / update / delete / select 的任何一个

根据子查询结果不同，分为：

+ 标量子查询(子查询结果为单个值)
+ 列子查询(子查询结果为一列)
+ 行子查询(子查询结果为一行)
+ 表子查询(子查询结果为多行多列)

根据子查询位置，分为：where之后、from之后、seelect之后

#### 标量子查询

子查询返回的结果是单个值(数字、字符串、日期)，最简单的形式，这种子查询称为**标量子查询**

常用的操作符： =  <>  >  >=  <  <=

```sql
select * from emp where country_id = (select id from country where country_name='稻妻');
```

#### 列子查询

子查询返回的结果是一列(可以是多行)，这种子查询称为**列子查询**

常用的操作符：in、not in、any、some、all

| 操作符 | 描述                                   |
| ------ | -------------------------------------- |
| in     | 在指定的集合范围之内，多选一           |
| not in | 不在指定的集合范围之内                 |
| any    | 子查询返回列表中，有任意一个满足即可   |
| some   | 与any等同，使用some的地方都可以使用any |
| all    | 子查询返回列表的所有值都必须满足       |

``` sql
-- 比财务部所有人工资都高的信息
select * from emp where salary > all (select salary from emp where dept_id = (select id from dept where name = '财务部') );
-- 比研发部任意一人工资高的信息
select * from emp where salary > any (select salary from emp where dept_id = (select id from dept where name = '研发部') );
```

#### 行子查询

子查询返回的结果是一行(可以是多列)，这种子查询称为杭子查询

常用的操作符：=   <>   in   not in

``` sql
-- 查询与"张三"的薪资及直属领导相同的员工信息
select * from emp where (salary, managerid) = (select salary, managerid from emp where name ='张无忌');
```

#### 表子查询

子查询返回的结果是多行多列，这种子查询称为表子查询

常用的操作符：in

``` sql
-- 查询与"张三", "李四"的制位和薪资相同的员工信息
select * from emp where (job, salary) in (select job, salary from emp where name = "张三" or name = "李四");
```

## 事务

**事务** 是一组操作的集合，它是一个不可分割的工作单位，事务会把所有的操作作为一个整体一起向系统提交或撤销操作请求，即这些操作 **要么同时成功**，**要么同时失败**

### 事务操作

``` sql
-- 查看/设置事务提交方式
select @@autocommit;
select @@autocommit =0;

-- 提交事务
commit;
-- 回滚事务
rollback;

```

### 事务四大特性

+ 原子性(Atomicity)：事务是不可分割的最小操作单元，要么全部成功，要么全部失败
+ 一致性(Consistency)：事务完成时，必须使所有的数据都保持一致
+ 隔离性(Isolation)：数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行
+ 持久性(Durability)：事务一旦提交或回滚，它对数据库中的数据的改变就是永久的

### 并发事务问题

| 问题       | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| 脏读       | 一个事务读到另一个事务还没有提交的数据                       |
| 不可重复读 | 一个事务先后读取同一条记录，但两次读取的数据不同，称之为不可重复读 |
| 幻读       | 一个事务按照条件查询数据时，没有对应的数据行，但是再插入数据时，又发现这条数据已经存在，好像出现了“幻影” |

### 事务隔离级别

| 隔离级别              | 脏读 | 不可重复读 | 幻读 |
| --------------------- | ---- | ---------- | ---- |
| Read uncommitted      | T    | T          | T    |
| Read committed        |      | T          | T    |
| Repeatable Read(默认) |      |            | T    |
| Serializable          |      |            |      |

``` sql
-- 查看事务隔离级别
select @@transaction_isolation;
-- 设置事务隔离级别
set [session|global] transaction isolation level {read uncommitted | read committed | repeatable read |}
```

