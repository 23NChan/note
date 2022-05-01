  

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

| 函数                                                       | 说明                                        |
| ---------------------------------------------------------- | ------------------------------------------- |
| if(value,t,f)                                              | 如果value为true则返回t，否则返回f           |
| if null(value1,value2)                                     | 如果value1不为空，返回value1,否则返回value2 |
| cse when [val1] then[res1] ... else [default] end          |                                             |
| case [expr] when [val1] then [res1] ... else [default] end |                                             |

