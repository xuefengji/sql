# Mysql

#### 简介

1、what：用来存储数据的仓库，以一定的形式存在磁盘上

2、数据库系统：由数据库管理系统、数据库、应用开发软件组成

#### 简单操作
1、打开数据库
cmd形式：mysql  -h host -u root -p 
修改提示符：mysql  -h host -u root -p --prompt 要修改的提示符
查看数据库：show database
```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| test               |
+--------------------+
5 rows in set (0.35 sec)

mysql>
```
查看表结构方式：

+ desc 表名
+ describe 表名
+ show columns from 表名

2、数据库简单操作
create：
create database [if not exsits] maizi [[default] chart-set utf-8];
删除：
drop database maizi；
use：
use maizi

#### 数据类型
1、整数类型

+ TINYINT  
+ SMALLINT
+ MEDIUMINT
+ INT
+ BIGINT
+ BOOL，BOOLEA 等价于TINYINT(1)，0为false，其余为true

无符号：unsined 

zerofill：零填充，当插入的数据没有达到最小范围时，用0进行填充，设置zerofill后会自动设置为无符号



2、浮点类型：可以四舍五入

+ float[(M,D)]   M代表的总位数，D代表小数点后的位数
+ double
+ decimal ：内部以字符串形式存储，精度比较高





3、字符串

+ char(M) ：定长字符串，M代表字符长度，空间大时间少，会去掉字符串末尾空格，开始空格不会被去掉
+ varchar(M)：变长字符串 M代表字符长度，空间小时间多，结尾和开始的空格都不会被去掉
+ TINYTEXT
+ TEXT :存储文本字符，不能有默认值
+ MEDIUMTEXT
+ LONGTEXT
+ ENUM("value1","value2"......)：枚举类型字符串，最多列举65535个，插入数据时可以填入序号，可以去掉空格
+ SET("value1","value2"............)：集合

char>varchar>text速率

char和varchar存储中文字符时，使用一个1个字符，utf-8存储中文字符时使用3个字符



4、日期时间类型

+ TIME ：时间

  insert test10 values(‘1 12：12：12’)

  结果为1天的小时数加上12小时

  insert test10 values(‘12：11’)

  结果为12：11：00

  insert test10 values(‘1211’)

  结果为00：12：11

+ DATE：日期

  insert test11 values(‘1211’)、insert test11 values(‘12/1/1’)、insert test11 values(‘120101’)

  结果为2012-01-01

+ DATETIME：日期时间

+ TIMESTAMP：时间戳

+ YEAR：年份（1901-2155），可以设置4位年份、2位年份、字符串形式，需要注意的时0，字符串0认为2000年，0代表0000

5、二进制类型

#### 存储引擎

1、什么是存储引擎
表的类型

2、如何查看MySql的存储引擎
show engines
show engines\G 以网格形式查看
show variables like ‘have%’ ：查看显示支持的存储引擎信息
show variables like ‘storage_engine’：查看默认的存储引擎


3、mysql常用存储引擎及特点
InnoDB：
MyISAM：
MEMORy：

4、如何选择合适的存储引擎



#### 数据库表操作

1、如何创建数据表

create table [if not exists] tbl_name(字段名 字段类型 [unsigned|zerofill] [not null] [default] [[primary ] key|unique [key]])engine=引擎名称 charset=编码方式 auto_increment = z自增长值;

完整性约束条件：

+ primary key：主键

  值不能重复，非空，作用在无意义的字段上

  create table if not exists user1(id int primary key,username varchar(20));

  复合主键：primary key（id，card）：必须同时满足多个条件

  create table if not exists user1(id int ,username varchar(20),card char(18),primary key(id,card));

  设置主键时可以省略primary

  create table if not exists user1(id int key,username varchar(20));

  

+ auto_increment：自增长，要和主键配合使用

  create table if not exists user2(id int key auto_increment,username varchar(20));

  这只指定auto_inctement值：

  create table if not exists user2(id int key auto_increment,username varchar(20))auto_increment=100;

  默认情况下，插入null或其他数据时，id会根据已有最大号自加1

  修改自增长值：alter table user2 auto_increment=500；

  

+ foreign key：外键

+ not null：非空

  create table if not exists user3(id int key auto_increment,username varchar(20)not null，password char(32)not null);

  

+ unique key：唯一，可以省略key

  

+ default：默认值

  not null配合默认值使用

  create table if not exists user3(id int key auto_increment,username varchar(20)not null，password char(32)not null，age tinyint unsigned not null default 18);



2、如何查看数据库中的数据表以及表结构

+ 查看数据表：show tables；
+ 查看指定表的结构：
  + desc tbl_name
  + describe tbl_name
  + show columns from tbl_name
+ 查看创建表时信息：
  + show create table 表名

