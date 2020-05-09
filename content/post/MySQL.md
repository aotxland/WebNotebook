+++
title = "MySQL基础"
date = "2020-05-08"
author = "Yif"
+++

## 数据库的相关概念

1. DB：数据库，保存一组有组织的数据的容器
2. DBMS：数据库管理系统，又称为数据库软件（产品），用于管理DB中的数据
3. SQL:结构化查询语言，用于和DBMS通信的语言

### 数据库存储数据的特点

1. 将数据放到表中，表再放到库中
2. 一个数据库中可以有多个表，每个表都有一个的名字，用来标识自己。表名具有唯一性。
3. 表具有一些特性，这些特性定义了数据在表中如何存储，类似java中 “类”的设计。
4. 表由列组成，我们也称为字段。所有表都是由一个或多个列组成的，每一列类似java 中的”属性”
5. 表中的数据是按行存储的，每一行类似于java中的“对象”。

## MySQL产品的介绍和安装

### MySQL服务的启动和停止

方式一：计算机——右击管理——服务
方式二：通过管理员身份运行
`net start 服务名（启动服务）`
`net stop 服务名（停止服务）`

### MySQL服务的登录和退出

方式一：通过mysql自带的客户端
只限于root用户

方式二：通过windows自带的客户端
登录：
`mysql 【-h主机名 -P端口号 】-u用户名 -p密码`

退出：
`exit或ctrl+C`

### MySQL的常见命令

1. 查看当前所有的数据库
 `show databases;`
2. 打开指定的库
`use 库名`
3. 查看当前库的所有表
`show tables;`
4. 查看其它库的所有表
`show tables from 库名;`
5. 创建表

```SQL
  create table 表名(
      列名 列类型,
      列名 列类型，
  );
```

6. 查看表结构
`desc 表名;`
7. 查看服务器的版本
方式一：登录到mysql服务端
`select version();`
方式二：没有登录到mysql服务端
`mysql --version`或`mysql --V`

### MySQL的语法规范

1. 不区分大小写,但建议关键字大写，表名、列名小写
2. 每条命令最好用分号结尾
3. 每条命令根据需要，可以进行缩进 或换行
4. 注释
单行注释：#注释文字
单行注释：-- 注释文字
多行注释：\/* 注释文字  \*/

### SQL的语言分类

* DQL（Data Query Language）：数据查询语言
`select`
* DML(Data Manipulate Language):数据操作语言
`insert 、update、delete`
* DDL（Data Define Languge）：数据定义语言
`create、drop、alter`
* TCL（Transaction Control Language）：事务控制语言
`commit、rollback`

## 数据定义

### 库的管理

1. 创建库
`create database (if not exists) 库名`
2. 删除库
`drop database (if exists) 库名`
3. 修改库
一般不改，有可能改字符集
`alter database 库名 character set 字符集`

### 表的管理

#### 创建表

```sql
CREATE TABLE IF NOT EXISTS 表名(
    列名 列的类型 (长度) (约束),
    stuId INT,
    stuName VARCHAR(20),
    gender CHAR,
    bornDate DATETIME
 );
DESC 表名; #用来展示表格
```

#### 修改表

 语法：
 `ALTER TABLE 表名 ADD|MODIFY|DROP|CHANGE COLUMN 字段名 【字段类型】;`

```sql
#修改字段名
 ALTER TABLE studentinfo CHANGE  COLUMN sex gender CHAR;

#修改表名
 ALTER TABLE stuinfo RENAME [TO]  studentinfo;
#修改字段类型和列级约束
 ALTER TABLE studentinfo MODIFY COLUMN borndate DATE ;

#添加字段
 ALTER TABLE studentinfo ADD COLUMN email VARCHAR(20) first;
#删除字段
 ALTER TABLE studentinfo DROP COLUMN email;
```

#### 删除表
`DROP TABLE [IF EXISTS] studentinfo;`

#### 复制表

1. 只复制结构
`create table 新表名 from 原表名;`
2. 复制结构和数据
`create table 新表名 select * from 原表名`

### 常见数据类型

#### 整型 

特点：
默认有符号，无符号 unsigned，零填充zerofill
超范围会显示为临界值

类型：
  tinyint 1字节
  smallint 2字节
  mediumint 3字节
  int 4字节
  bigint 8字节

#### 小数

浮点型
float(m,d) 4字节
double(m,d) 8字节
m表示一共位数，d表示小数点后位数
m和d默认由插入的数值精度决定

定点型
dec(m,d) m+2字节
dec m默认为10，d默认为0

#### 字符型

短文本：
  char(m)：长度不可变，效率高，m可省略
  varchar(m)：长度可变，效率低

长文本：text, bigtext

集合： enum（单选）, set（可多选）

Blob类型：二进制文本

#### 日期型

data
datatime
timestamp
time
year

### 约束

常见约束类型：

* NOT NULL非空
* DEFAULT默认
* UNIQUE唯一
* CHECK检查
* PRIMARY KEY主键
* FOREIGN KEY外键
* AUTO_INCREMENT自增

主键和唯一的区别：

1. 都保证唯一性
2. 主键必须非空；唯一可以空
3. 主键只能有一个，唯一可以很多
4. 都可以组合使用

修改约束：

```sql
修改非空约束
ALTER TABLE 表名 MODIFY COLUMN 字段名 数据类型 NOT NULL;
ALTER TABLE 表名 MODIFY COLUMN 字段名 数据类型;
修改默认约束
ALTER TABLE 表名 MODIFY COLUMN 字段名 数据类型 DEFAULT 默认值;
ALTER TABLE 表名 MODIFY COLUMN 字段名 数据类型;
修改自增约束
ALTER TABLE 表名 MODIFY COLUMN 字段名 数据类型 AUTO_INCREMENT;
ALTER TABLE 表名 MODIFY COLUMN 字段名 数据类型;
修改主键约束
ALTER TABLE 表名 MODIFY COLUMN 字段名 数据类型 PRIMARY KEY;
ALTER TABLE 表名 ADD PRIMARY KEY(字段名);
ALTER TABLE 表名 DROP PRIMARY KEY(字段名);
修改唯一约束
ALTER TABLE 表名 MODIFY COLUMN 字段名 数据类型 UNIQUE;
ALTER TABLE 表名 ADD UNIQUE(字段名);
ALTER TABLE 表名 DROP UNIQUE(字段名);
修改外键
ALTER TABLE 表名 ADD FOREIGN KEY(字段名) REFERENCE 表名(字段名);
ALTER TABLE 表名 DROP FOREIGN KEY(字段名);
```

## 数据查询

### 基础查询和条件查询

特点：

1. 通过select查询完的结果 ，是一个虚拟的表格，不是真实存在
2. 要查询的东西 可以是常量值、可以是表达式、可以是字段、可以是函数

语法：

```SQL
select
    要查询的字段|表达式|常量值|函数
from
    表
where
    条件 ;
```

分类：

1. 条件表达式
示例：`salary>10000`
    条件运算符：
    `> < >= <= = != <>`

2. 逻辑表达式
示例：`salary>10000 && salary<20000`
    逻辑运算符：
    and（&&）:两个条件如果同时成立，结果为true，否则为false
    or(||)：两个条件只要有一个成立，结果为true，否则为false
    not(!)：如果条件成立，则not后为false，否则为true

3. 模糊查询 like
示例：`last_name like 'a%'`
可使用%  _ 填充

### 排序查询(order by)

语法：

```SQL
select
  要查询的东西
from
  表
where
  条件

order by 排序的字段|表达式|函数|别名 【asc|desc】
```

### 使用常见函数查询

#### (一)、单行函数

##### 1、字符函数

* concat拼接
* substr截取子串：共四个重载形式
* upper转换成大写
* lower转换成小写
* trim去前后指定的空格和字符
* ltrim去左边空格
* rtrim去右边空格
* replace替换
* lpad左填充
* rpad右填充
* instr返回子串第一次出现的索引
* length 获取字节个数

##### 2、数学函数

* round 四舍五入
* rand 随机数
* floor向下取整
* ceil向上取整
* mod取余
* truncate截断

##### 3、日期函数

* now当前系统日期+时间
* curdate当前系统日期
* curtime当前系统时间
* str_to_date 将字符转换成日期
* date_format将日期转换成字符

##### 4、流程控制函数

* if 处理双分支
* case语句 处理多分支
  情况1：处理等值判断
  情况2：处理条件判断

##### 5、其他函数

* version版本
* database当前库
* user当前连接用户

#### (二)、分组函数

* sum 求和
* max 最大值
* min 最小值
* avg 平均值
* count 计数

特点：

1. 以上五个分组函数都忽略null值，除了count(*)
2. sum和avg一般用于处理数值型
max、min、count可以处理任何数据类型
3. 都可以搭配distinct使用，用于统计去重后的结果
4. count的参数可以支持：
字段、\*、常量值，一般放1，建议使用 count(*)

### 分组查询(group by)

语法：

```SQL
select 查询的字段，分组函数
from 表
group by 分组的字段
```

特点：

1. 可以按单个字段分组
2. 和分组函数一同查询的字段最好是分组后的字段
3. 分组筛选
针对的表    位置    关键字
分组前筛选：    原始表    group by的前面    where
分组后筛选：    分组后的结果集    group by的后面    having
4. 可以按多个字段分组，字段之间用逗号隔开
5. 可以支持排序
6. having后可以支持别名

### 连接查询(join)

笛卡尔乘积：如果连接条件省略或无效则会出现
解决办法：添加上连接条件

按功能分类：

  * 内连接
    * 等值连接
    * 非等值连接
    * 自连接
  * 外连接
    * 右外连接
    * 左外连接
    * 全外连接
  * 交叉连接

#### (一)、内连接

1. 等值连接的结果 = 多个表的交集
2. n表连接，至少需要n-1个连接条件
3. 多个表不分主次，没有顺序要求
4. 一般为表起别名，起别名后不能用原名

语法：

```sql
select 字段，...
from 表1
join 表2 on 连接条件
【where 筛选条件】
【group by 分组字段】
【having 分组后的筛选条件】
【order by 排序的字段或表达式】
```

#### (二)、 外连接

语法：

左外左侧主表，右外右侧主表

```sql
select 字段，...
from 表1
left outer|right outer join 表2 on 连接条件
【where 筛选条件】
【group by 分组字段】
【having 分组后的筛选条件】
【order by 排序的字段或表达式】
```

#### (三)、交叉连接

交叉连接(cross)

语法：

```sql
select 字段，...
from 表1
cross join 表2 on 连接条件
cross join 表3 on 连接条件
【where 筛选条件】
【group by 分组字段】
【having 分组后的筛选条件】
【order by 排序的字段或表达式】
```

### 子查询

含义：

一条查询语句中又嵌套了另一条完整的select语句，其中被嵌套的select语句,称为子查询或内查询
在外面的查询语句，称为主查询或外查询

特点：

1. 子查询都放在小括号内，一般放在条件的右侧
2. 子查询可以放在from后面、select后面、where后面、having后面
3. 子查询优先于主查询执行，主查询使用了子查询的执行结果

分类：

按照子查询结果的位置：

* 标量子查询（单行子查询）
   结果集只有一行一列（一个数），一般搭配单行操作符使用：> < = <> >= <=
    非法使用子查询的情况：
      a、子查询的结果为一组值
      b、子查询的结果为空
* 列子查询（多行子查询）
  结果集有一列多行，一般搭配多行操作符使用：any、all、in、not in
    in属于子查询结果中的任意一个就行
    any和all往往可以用其他查询代替
* 行子查询：一行多列
* 表子查询

按照子查询出现的位置：

* select后面：标量子查询
* from后面：行子查询
* where或having后面：标量子查询、列子查询、行子查询
* exists后边：表子查询(相关子查询)

### 分页查询

应用场景：

 实际的web项目中需要根据用户的需求提交对应的分页查询的sql语句

语法：

```sql
select 字段|表达式,...
from 表
【where 条件】
【group by 分组字段】
【having 条件】
【order by 排序的字段】
limit 【起始的条目索引，】条目数;
```

特点：

1. 起始条目索引从0开始
2. limit子句放在查询语句的最后
3. 公式：`select * from  表 limit （page-1）*sizePerPage,sizePerPage`
    假如:
    每页显示条目数sizePerPage
    要显示的页数 page

### 联合查询(union)

 特点：

1. 多条查询语句的查询的列数必须是一致的
2. 多条查询语句的查询的列的类型几乎相同
3. union代表去重，union all代表不去重

语法：

```sql
 select 字段|常量|表达式|函数 【from 表】 【where 条件】 union
 select 字段|常量|表达式|函数 【from 表】 【where 条件】 union
 .....
 select 字段|常量|表达式|函数 【from 表】 【where 条件】
```

## 数据操作

### 插入(insert)

语法：

```sql
insert into 表名(字段名，...)
values(值1，...);
```

特点：

1. 字段类型和值类型一致或兼容，而且一一对应
2. 可以为空的字段，可以不用插入值，或用null填充
3. 不可以为空的字段，必须插入值
4. 字段个数和值的个数必须一致
5. 字段可以省略，但默认所有字段，并且顺序和表中的存储顺序一致

### 修改(update)

修改单表语法：

```sql
update 表名 set 字段=新值,字段=新值
【where 条件】
```

修改多表语法：

```sql
 update 表1 别名1,表2 别名2
 set 字段=新值，字段=新值
 where 连接条件
 and 筛选条件
```

### 删除(delete)

方式1：delete语句
单表的删除： ★
`delete from 表名 【where 筛选条件】`

多表的删除：

```sql
delete 别名1，别名2
from 表1 别名1，表2 别名2
where 连接条件
and 筛选条件;
```

方式2：truncate语句
`truncate table 表名`

两种方式的区别

1. truncate不能加where条件，而delete可以加where条件
2. truncate的效率高一丢丢
3. truncate 删除带自增长的列的表后，如果再插入数据，数据从1开始
   delete 删除带自增长列的表后，如果再插入数据，数据从上一次的断点处开始
4. truncate删除不能回滚，delete删除可以回滚

## 事务

 通过一组逻辑操作单元（一组DML——sql语句），将数据从一种状态切换到另外一种状态。执行单元要么全执行，要么全不执行。

### 特点（ACID）

  原子性：要么都执行，要么都回滚
  一致性：保证数据的状态操作前和操作后保持一致
  隔离性：多个事务同时操作相同数据库的同一个数据时，一个事务的执行不受另外一个事务的干扰
  持久性：一个事务一旦提交，则数据将持久化到本地，除非其他事务对其进行修改

### 事务的分类

隐式事务：没有明显的开启和结束事务的标志
  例如insert、update、delete语句本身就是一个事务

显式事务：具有明显的开启和结束事务的标志

### 使用步骤

1. 开启事务，取消自动提交事务的功能

2. 编写事务的一组逻辑操作单元（多条sql语句）
    insert
    update
    delete

3. 提交事务或回滚事务

```sql
 set autocommit=0;
 start transaction; #可选
 commit;
 rollback;
```

 savepoint  断点
 commit to 断点
 rollback to 断点

### 事务并发

#### 事务并发问题如何发生

 当多个事务同时操作同一个数据库的相同数据时

#### 事务的并发问题有哪些

* 脏读：一个事务读取到了另外一个事务未提交的数据
* 不可重复读：同一个事务中，多次读取到的数据不一致
* 幻读：一个事务读取数据时，另外一个事务进行更新，导致第一个事务读取到了没有更新的数据

#### 如何避免事务的并发问题

通过设置事务的隔离级别

1. READ UNCOMMITTED读未提交数据
2. READ COMMITTED读已提交数据：可以避免脏读
3. REPEATABLE READ可重复读：可以避免脏读、不可重复读和一部分幻读
4. SERIALIZABLE串行化：可以避免脏读、不可重复读和幻读

设置隔离级别
`set session|global  transaction isolation level 隔离级别名;`

查看隔离级别
`select @@tx_isolation;`
MySQL8之后改为
`select @@transaction_isolation`

## 视图

### 视图的特点

一张虚拟的表

视图和表的区别：

1. 使用方式完全相同
2. 表占用物理空间；视图不占用，仅仅保存的是sql逻辑

视图的好处：

 1. sql语句提高重用性，效率高
 2. 和表实现了分离，提高了安全性

### 视图的创建修改查看

创建

```sql
CREATE VIEW 视图名
AS
查询语句;
```

修改

```sql
ALTER VIEW 视图名
AS
查询语句;
或
CREATE OR REPLACE VIEW 视图名
AS
查询语句;

```

删除
`DROP VIEW 视图名(,视图名);`

查看

```sql
DESC 视图名;
SHOW CREATE VIEW 视图名;
```

### 视图的更新

1. 查看视图的数据 ★
`SELECT * FROM 视图名;`
或
`SELECT * FROM 视图名 WHERE 语句;`
2. 插入视图的数据
`INSERT INTO 视图名(last_name,department_id) VALUES('虚竹',90);`
3. 修改视图的数据
`UPDATE my_v4 SET last_name ='梦姑' WHERE last_name='虚竹';`
4. 删除视图的数据
`DELETE FROM 视图名;`

以下视图不能更新

* 包含以下关键字的sql语句：分组函数、distinct、group  by、having、union或者union all
* 常量视图
* Select中包含子查询
* join
* from一个不能更新的视图
* where子句的子查询引用了from子句中的表

## 变量、函数和存储过程

### 变量

#### 系统变量

##### 全局变量

作用域：针对于所有会话（连接）有效，但不能跨重启

```sql
 查看所有全局变量
 SHOW GLOBAL VARIABLES;
 查看满足条件的部分系统变量
 SHOW GLOBAL VARIABLES LIKE '%char%';
 查看指定的系统变量的值
 SELECT @@global.autocommit;
 为某个系统变量赋值
 SET @@global.autocommit=0;
 SET GLOBAL autocommit=0;
```

##### 会话变量

作用域：针对于当前会话（连接）有效

```sql
 查看所有会话变量
 SHOW SESSION VARIABLES;
 查看满足条件的部分会话变量
 SHOW SESSION VARIABLES LIKE '%char%';
 查看指定的会话变量的值
 SELECT @@autocommit;
 SELECT @@session.tx_isolation;
 为某个会话变量赋值
 SET @@session.tx_isolation='read-uncommitted';
 SET SESSION tx_isolation='read-committed';
```

#### 自定义变量

##### 用户变量

```sql
声明并初始化：
 SET @变量名=值;
 SET @变量名:=值;
 SELECT @变量名:=值;
赋值：
 方式一：一般用于赋简单的值
 SET 变量名=值;
 SET 变量名:=值;
 SELECT 变量名:=值;
 方式二：一般用于赋表 中的字段值
 SELECT 字段名或表达式 INTO 变量
 FROM 表;
使用：
 select @变量名;
```

二、局部变量

```sql
声明：
 declare 变量名 类型 【default 值】;
赋值：
 方式一：一般用于赋简单的值
 SET 变量名=值;
 SET 变量名:=值;
 SELECT 变量名:=值;
 方式二：一般用于赋表 中的字段值
 SELECT 字段名或表达式 INTO 变量
 FROM 表;
使用：
 select 变量名
```

### 存储过程

含义：一组经过预先编译的sql语句的集合
好处：

1. 提高了sql语句的重用性，减少了开发程序员的压力
2. 提高了效率
3. 减少了传输次数

分类：

1. 无返回无参
2. 仅仅带in类型，无返回有参
3. 仅仅带out类型，有返回无参
4. 既带in又带out，有返回有参
5. 带inout，有返回有参
注意：in、out、inout都可以在一个存储过程中带多个

#### 创建存储过程

语法：
in:该参数只能作为输入 （该参数不能做返回值）
out：该参数只能作为输出（该参数只能做返回值）
inout：既能做输入又能做输出

```sql
 create procedure 存储过程名(in|out|inout 参数名  参数类型,...)
 begin
  存储过程体
 end
```

#### 注意事项

1. 需要设置新的结束标记
 delimiter 新的结束标记
 示例：
```sql
 delimiter $

 CREATE PROCEDURE 存储过程名(IN|OUT|INOUT 参数名  参数类型,...)
 BEGIN
  sql语句1;
  sql语句2;

 END $
```

2. 存储过程体中可以有多条sql语句，如果仅仅一条sql语句，则可以省略begin end

#### 调用存储过程

`call 存储过程名(实参列表)`

### 函数

#### 创建函数

语法：

```sql
 CREATE FUNCTION 函数名(参数名 参数类型,...) RETURNS 返回类型
 BEGIN
  函数体

 END
```

#### 调用函数
`SELECT 函数名（实参列表）`

### 函数和存储过程的区别

   关键字  调用语法 返回值   应用场景
 函数  FUNCTION SELECT 函数() 只能是一个  一般用于查询结果为一个值并返回时，当有返回值而且仅仅一个
 存储过程 PROCEDURE CALL 存储过程() 可以有0个或多个  一般用于更新

## 流程控制结构

### 分支

#### if

 语法：if(条件，值1，值2)
 特点：可以用在任何位置

#### case

语法：

```sql
 情况一：类似于switch
 case 表达式
 when 值1 then 结果1或语句1(如果是语句，需要加分号) 
 when 值2 then 结果2或语句2(如果是语句，需要加分号)
 ...
 else 结果n或语句n(如果是语句，需要加分号)
 end 【case】（如果是放在begin end中需要加上case，如果放在select后面不需要）

 情况二：类似于多重if
 case 
 when 条件1 then 结果1或语句1(如果是语句，需要加分号) 
 when 条件2 then 结果2或语句2(如果是语句，需要加分号)
 ...
 else 结果n或语句n(如果是语句，需要加分号)
 end 【case】（如果是放在begin end中需要加上case，如果放在select后面不需要）
```

特点：
 可以用在任何位置

#### if elseif

语法：

```sql
 if 情况1 then 语句1;
 elseif 情况2 then 语句2;
 ...
 else 语句n;
 end if;
```

特点：
 只能用在begin end中

#### 三者比较

* if函数：简单双分支
* case结构：等值判断的多分支
* if结构：区间判断的多分支

### 循环

语法：

```sql
 【标签：】WHILE 循环条件  DO
  循环体
 END WHILE 【标签】;
```

特点：

* 只能放在BEGIN END里面
* 如果要搭配leave跳转语句，需要使用标签，否则可以不用标签
* leave类似于java中的break语句，跳出所在循环
