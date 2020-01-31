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

2、浮点类型
+ float[(M,D)]   M代表的总位数，D代表小数点后的位数
+ double
+ decimal 和double一样，内部以字符串形式存储

3、字符串
+ char(M) ：定长字符串，M代表字符长度
+ varchar(M)：变长字符串 M代表字符长度
+ TINYTEXT
+ TEXT :存储字符串
+ MEDIUMTEXT
+ LONGTEXT
+ ENUM("value1","value2"......)：枚举类型字符串，最多列举65535个
+ SET("value1","value2"............)：集合

4、日期时间类型
+ TIME ：时间
+ DATE：日期
+ DATETIME：日期时间
+ TIMESTAMP：时间戳
+ YEAR：年份

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
