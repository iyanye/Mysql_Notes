```

```



# 一.概念篇

### 1.关于数据库，数据库管理系统和SQL

数据库（DataBase）是一个仓库，通过存放表单（table）来构建在硬盘上的进行数据的存储。

数据库管理系统（DataBase Management System）是一个软件层面上的称呼，通过它可以让人们简单方便的对数据库进行构增删改查等操作，而本文的主要内容就是对关系型数据库（Relational DataBase Management System ）MySql进行知识的整理和总结。

Sql（Structured Query Language  结构化查询语言）是通过约定俗成的准则（SQL标准）来对数据库进行操作和变动。

下图为实际情况中数据库和客户的联系。

<img src="file:///C:\Users\HP\Documents\Tencent Files\2486303400\Image\C2C\_ATT{0$1C~17`2]BC`VND_6.png" alt="img" style="zoom: 80%;" />

### 2.关系型数据库

在阐述关系型数据库（Relational DataBase Management System）的“关系型”之前我们必须先了解关系型数据库模型，关系型数据库模型是指将我们所要记录的复杂的关系数据整理为二维表的状态，通过行（row）和列（colunm）来组成表（table），再将表（table）和表（table）之间通过关系连接整合为库即为关系型数据库，如下表是一个常见的二维表

| 姓名     | 年龄 | 身份     |
| -------- | ---- | -------- |
| 初音未来 | 16   | 虚拟歌姬 |
| 樋口円香 | 17   | 偶像     |
| 廖维     | 21   | 学生     |

而一行（row）也被称为一个实体（instance）或是一条记录（record）下文统称为记录（record），而一列（colunm）也被称为一个属性（attribute）或是一个字段（filed）下文统称为属性（attribute）。

### 3.基本属性类型

在正式开始面前，我想先说明一下sql中的基本属性类型，包括数值型（int为例），字符串型（string为例），日期型（date为例）。如下表

| 分类         | 类型                    | 大小                                     | 描述                                                         | 格式                |
| ------------ | ----------------------- | ---------------------------------------- | ------------------------------------------------------------ | ------------------- |
| 数值型       | TinnyInt                | 1byte                                    | 有符号为（-128，127） 无符号（0，255）                       |                     |
|              | SmallInt                | 2bytes                                   | 有符号为（-32768，32767） 无符号（0，65535）                 |                     |
|              | MediumInt               | 3bytes                                   | 有符号为（-8388608，8388607） 无符号（0，16777215）          |                     |
|              | Int或Integer            | 4bytes                                   | 有符号为（-2147483648，2147483647） 无符号（0，4294967295）  |                     |
|              | BigInt                  | 8bytes                                   | 有符号为（-2 ^ 63，2^63 - 1） 无符号（0，2 ^ 64 - 1）        |                     |
|              | Float                   | 4bytes                                   | 有符号(-3.402 823 466 E+38，-1.175 494 351 E-38)，（0，1.175 494 351 E-38，3.402 823 466 351 E+38) 无符号（0，1.175 494 351 E-38，3.402 823 466 E+38) |                     |
|              | Double                  | 8bytes                                   | 有符号(-1.797 693 134 862 315 7 E+308，-2.225 073 858 507 201 4 E-308)，0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308)  无符号 （0，2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) |                     |
|              | Decimal（小数）         | 对Decimal(M,D) ，如果M>D，为M+2否则为D+2 | M为标度，D为精度。例：100.1  -》  double（4，1）             |                     |
| 字符串型     | Char                    | 0-255 bytes                              | 定长度字符串                                                 | char(/\*长度\*/)    |
|              | Varchar                 | 0-65535 bytes                            | 变长字符串                                                   | varchar(/\*长度\*/) |
|              | TinBlob（blob意为斑马） | 0-255 bytes                              | 不超过 255 个字符的二进制字符串                              |                     |
|              | TinyText                | 0-255 bytes                              | 短文本字符串                                                 |                     |
|              | Blob                    | 0-65 535 bytes                           | 二进制形式的长文本数据                                       |                     |
|              | Text                    | 0-65 535 bytes                           | 长文本数据                                                   |                     |
|              | MediumBlob              | 0-16 777 215 bytes                       | 二进制形式的中等长度文本数据                                 |                     |
|              | MediumText              | 0-16 777 215 bytes                       | 中等长度文本数据                                             |                     |
|              | LongBlob                | 0-4 294 967 295 bytes                    | 二进制形式的极大文本数据                                     |                     |
|              | LongText                | 0-4 294 967 295 bytes                    | 极大文本数据                                                 |                     |
| 日期和时间型 | Date                    | 3bytes                                   | 1000-01-01到9999-12-31                                       | YYYY-MM-DD          |
|              | Time                    | 3bytes                                   | '-838:59:59'到'838:59:59'                                    | HH:MM:SS            |
|              | Year                    | 1bytes                                   | 1901到2155                                                   | YYYY                |
|              | DateTime                | 8bytes                                   | 1000-01-01 00:00:00到9999-12-31 23:59:59                     | YYYY-MM-DD HH:MM:SS |
|              | TimeStampt              | 4bytes                                   | 1970-01-01 00:00:00到2038 -01-19 03:14:07                    | YYYYMMDD HHMMSS     |



# 二.基础语法篇

### 1.SQL分类

sql可以从功能上分为四类分别为DDL（DataBase Definition Language）,DML（DataBase Manipulation Language ）,DQL（DataBase Query Language ）,DCL（DataBase Control Language ）。

注：sql中注释为：--注释 或 #注释（MySql特有）或 /\*注释\*/。sql不区分大小写。 

### 2.DDL（DataBase Definition Language）

意为数据库定于语言，多用来定义和管理数据对象如库，表，和属性

#### 展示

```mysql
show databases;#展示所有数据库
```

```mysql
select database();#查询当前所处数据库
```

```mysql
desc 表名;#展示表细节（属性）
```

```mysql
show create 表名;#展示创建表时语句
```

```mysql
show tables;#展示目前数据库下所有表
```



#### 创建

```mysql
create database /*if not exists*/ 数据库名 /*default chatset 字符集*/ /*collate 排序规则*/;#创建一个数据库，/*如果存在则不创建*/ /*默认字符集*/  /*排序规则*/
```

```mysql
create table 表名(
	属性1 属性1类型, 
    属性2 属性2类型,
    属性3 属性3类型,
); #创建一个表
/*
	create table student（
		sno int,
		name varchar(10),
		gender char(1),
		age int unsigned
	）；
*/
```

#### 删除

```mysql
drop database /*if exists*/ 数据库名; #删除数据库 
```

```mysql
drop table 表名;#删除表
```

```mysql
truncate table 表名;#删除表并重新创建该表 truncate意为截断，缩短
```

```mysql
alert table 表名 drop 属性名;#删除表中某一属性
```



#### 切换数据库

```mysql
use 数据库名; #切换数据库
```

#### 修改

```mysql
alert table 表名 add 字段名 类型;#给表中增加一个属性 
/*alert table student add height int*/
```

```mysql
alert table 表名 rename to 新表名;#修改表名
```

```mysql
alert table 表名 modify 属性名 新类型;#修改表中某属性的类型
```

```mysql
alert table 表名 change 旧属性名 新属性名 类型;#将一个属性重新命名和修改类型
```

### 2.DML（DataBase Manipulation Language）

数据库操作语言，具体的对某个表进行增删改查操作，Manipulation意为操作，操控。

#### 增加

```mysql
insert into 表名 （属性1，属性2） values （值1，值2）;#给属性1，2插入值1，2注意属性和值要一一对应
```

```mysql
insert into 表名 vales （值1，值2等等）;#给所有属性依次增加值（增加一个记录）
```

```mysql
insert into 表名 （属性1，属性2） values （值1，值2），（值3，值4）;#批量给属性增加值
```

```mysql
insert into 表名 vales （值1，值2），（值3，值4）;#批量增加记录
```

#### 删除

```mysql
delete from 表名 /*where 条件*/;#从表中删除数据，如果不加条件删除整表所有数据，若想删除某一记录建议使用update修改语句
```

#### 修改

```mysql
update 表名 set 属性1=值1，属性2=值2 /*wehere 条件*/:#更新属性值 /*修改条件*/
```

### 3.DQL（DataBase Query Language）

在sql中最复杂和重要的部分就是所谓的查询语句也就是DQL，接下来我将一步步的增加查询的复杂度来解释DQL

#### 基础查询

```mysql
select 属性1，属性2 from 表名;#将表中的属性单独整理成为一张表
```

```mysql
select * from 表名;#查询表中全部属性，并不建议使用，因为既不直观，使用“*”也会增加多余的消耗
```

```mysql
select 属性1 /*as*/ 属性1别名 from 表名;#将属性1设置一个别名，后续无法使用属性1，只能使用别名调用属性1
```

```mysql
select distinct 属性1 from 表名;#查询属性1，并将属性1数值重复的去除
```

#### 条件查询

```mysql
select 属性1 from 表名 where 条件;#在基础查询整理成表后，在该表中进行条件筛选，去除不符合条件的记录
```

其中“条件”中除了常用的“==”，“>”，“<”以外还有如下表中的符号

| 符号                        | 含义                                             |
| --------------------------- | ------------------------------------------------ |
| ！=或<>                     | 不等于                                           |
| between   边界1  and  边界2 | 边界1必须小于边界2，并且查询结果包括两个边界     |
| in（表）                    | 在表中进行查询                                   |
| like 占位符                 | 模糊匹配，占位符包括“_”占一个字符，“%”占任意字符 |
| is null                     | 是null                                           |
| and 或 &&                   | 并                                               |
| or 或 \|\|                  | 或                                               |
| not 或 ！                   | 非                                               |

```mysql
/*举例*/ select name form sutdent where age between 15 and 20;#从学生表中选取年龄15到20的学生姓名整理成表 
```

#### 聚合函数

常用的函数如下表

| 函数          | 功能     |
| ------------- | -------- |
| count（属性） | 统计数量 |
| max（属性）   | 最大值   |
| min（属性）   | 最小值   |
| avg（属性）   | 平均值   |
| sum（属性）   | 求和     |

聚合函数是将括号内的属性这一列，进行函数运算，并且null不会进行函数运算

```mysql
select 函数（属性） from 表名;#从表中将某属性整理成表后再将这一列进行函数运算
```

#### 分组查询

```mysql
selec 属性 from 表 /*where 条件*/ group by 条件 /*having 条件*/;#将基础查询整理的表，再按照条件进行分组再出表，可以再通过having之后的条件再进行筛选
```

#### 排序查询

```mysql
select 属性 from 表 order by 条件1，条件2 /*desc 或 asc*/;#将基础查询整理的表，进行排序，desc为降序，asc为升序，如果有多个条件，会将在条件一下相同的记录根据条件2排序
```

#### 分页查询

这个我是几乎没有使用过，而且不同语言的分页查询关键字也不尽相同，Mysql中是limit

```mysql
selec 属性 from 表名 limit 起始索引，查询记录数;#其实索引从0开始，起始索引为记录条数，例两页，每页记录数为十，起始索引就为2*10=20.查询记录数为每页展示的记录条数
```

#### 小结

这些DQL语句可以整合成为如下的一个语句

```mysql
selet 属性 from 表 where 条件1 group by 条件2 having 条件3 order by 条件4 limit条件5;#并且执行顺序为where->group by->having->select ->order by->limit
```

### 4.DCL（DataBase Control Language）

此处的语句大多为对数据库的操作

#### 用户管理

```mysql
select * from user;#查询所有用户和他们的数据库权限权限
```

```mysql
create  user '用户名'@'主机名' identified by '密码';#创建新用户 本地主机为localhost 任意主机为%
```

```mysql
alert user '用户名'@'主机名' identified with mysql_native_password by '密码';#修改用户密码
```

```mysql
drop user '用户名'@'主机名';#删除用户
```

#### 权限管理

```mysql
show grants for '用户名'@'主机名';#展示用户权限
```

```mysql
grant 权限 on 数据库.表 to  '用户名'@'主机名';#给予某个用户某个数据库中表的权限 全部权限为 all on *.*
```

### 5.常用函数表

![image-20220321204734527](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220321204734527.png)

![image-20220321204804615](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220321204804615.png)![image-20220321204818003](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220321204818003.png)

#### 简单案例

生成六伪随机数的验证码

```mysql
select round（rand（）*1000000，0）;
```

成绩85以上为优秀60以上为及格60以下为不及格

```mysql
select id，name， （case when math >=85 then  '优秀' when math >=60 then '及格' else '不及格' end） '数学' from score
```

### 6.约束

约束就是对属性的数值进行一定的限定，在图形化界面输入时就可以设置。约束是为了确保数据的正确，有效性和完整性。

| 约束     | 描述           | 关键字      |
| -------- | -------------- | ----------- |
| 非空约束 | 数值不可为null | not null    |
| 唯一约束 | 不可重复       | unique      |
| 主键约束 | 非空且唯一     | primary key |
| 默认约束 | 默认值         | default     |
| 检查约束 | 满足某一个条件 | check       |
| 外键约束 | 为外键         | foreign key |

### 7.多表关系

分为三种，一对一，多对多，一对多

#### 多表查询

多张表之间可以通过内连接和外连接（左，右连接）方式联系起来，内连接为两个表交集的部分，外连接为一张表本身和两表的交集部分

```mysql
select * form 表1，表2; #内连接，表1和表2会做笛卡尔集（表1的每个记录和表2的所有记录组合一次）
```

```mysql
select * from 表1 left join 表2 on 条件;#左外连接
```

```mysql
select 属性1 from 表1 union /*all*/ select 属性2 from 表2；#联合查询，将两个查询结果合并成一个新的查询结果集（上下拼接，所以属性个数要相等）
```

