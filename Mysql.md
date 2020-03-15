# Mysql

#### 简介

1、what：用来存储数据的仓库，以一定的形式存在磁盘上

2、数据库系统：由数据库管理系统、数据库、应用开发软件组成

#### 简单操作
1、打开数据库
cmd形式：mysql  -h host -u root -p 

mysql  -h host -u root -p -D 要打开的数据库名

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

zerofill：零填充，当插入的数据没有达到最小范围时，用 0 进行填充，设置 zerofill 后会自动设置为无符号



2、浮点类型：可以四舍五入

+ float[(M,D)]   M 代表的总位数，D 代表小数点后的位数
+ double
+ decimal ：内部以字符串形式存储，精度比较高





3、字符串

+ char(M) ：定长字符串，M 代表字符长度，空间大时间少，会去掉字符串末尾空格，开始空格不会被去掉
+ varchar(M)：变长字符串  M 代表字符长度，空间小时间多，结尾和开始的空格都不会被去掉
+ TINYTEXT
+ TEXT :存储文本字符，不能有默认值
+ MEDIUMTEXT
+ LONGTEXT
+ ENUM("value1","value2"......)：枚举类型字符串，最多列举 65535 个，插入数据时可以填入序号，可以去掉空格
+ SET("value1","value2"............)：集合

char>varchar>text 速率

char 和 varchar 存储中文字符时，使用一个1个字符，utf-8 存储中文字符时使用 3 个字符



4、日期时间类型

+ TIME ：时间

  insert test10 values(‘1 12：12：12’)

  结果为 1 天的小时数加上 12 小时

  insert test10 values(‘12：11’)

  结果为 12：11：00

  insert test10 values(‘1211’)

  结果为 00：12：11

+ DATE：日期

  insert test11 values(‘1211’)、insert test11 values(‘12/1/1’)、insert test11 values(‘120101’)

  结果为 2012-01-01

+ DATETIME：日期时间

+ TIMESTAMP：时间戳

+ YEAR：年份（1901-2155），可以设置 4 位年份、2 位年份、字符串形式，需要注意的时 0，字符串 0 认为 2000 年，0 代表 0000

5、二进制类型

#### 存储引擎

1、什么是存储引擎

  表的类型

2、如何查看 MySql 的存储引擎

show engines
show engines\G  以网格形式查看
show variables like ‘have%’ ：查看显示支持的存储引擎信息
show variables like ‘storage_engine’：查看默认的存储引擎

3、mysql 常用存储引擎及特点
InnoDB：
MyISAM：
MEMORy：

4、如何选择合适的存储引擎



#### 数据库表操作

1、如何创建数据表

create table [if not exists] tbl_name(字段名 字段类型 [unsigned|zerofill] [not null] [default] [[primary ] key|unique [key]])engine=引擎名称 charset=编码方式 auto_increment = 自增长值;

完整性约束条件：

+ primary key：主键

  值不能重复，非空，作用在无意义的字段上

  create table if not exists user1(id int primary key,username varchar(20));

  复合主键：primary key（id，card）：必须同时满足多个条件

  create table if not exists user1(id int ,username varchar(20),card char(18),primary key(id,card));

  设置主键时可以省略 primary

  create table if not exists user1(id int key,username varchar(20));

  

+ auto_increment：自增长，要和主键配合使用

  create table if not exists user2(id int key auto_increment,username varchar(20));

  这只指定 auto_inctement 值：

  create table if not exists user2(id int key auto_increment,username varchar(20))auto_increment=100;

  默认情况下，插入 null 或其他数据时，id 会根据已有最大号自加 1

  修改自增长值：alter table user2 auto_increment=500；

  

+ foreign key：外键

  create table if not exists employee(id int key auto_increment,username varchar(20)not null，depId tinyint unsigned,foreign key(depId) references department(id));

  

+ not null：非空

  create table if not exists user3(id int key auto_increment,username varchar(20)not null，password char(32)not null);

  

+ unique key：唯一，可以省略 key

  

+ default：默认值

  not null 配合默认值使用

  create table if not exists user3(id int key auto_increment,username varchar(20)not null，password char(32)not null，age tinyint unsigned not null default 18);



2、如何查看数据库中的数据表以及表结构

+ 查看数据表：show tables；

+ 查看指定表的结构：

  + desc tbl_name
  + describe tbl_name
  + show columns from tbl_name

+ 查看创建表时信息：

  + show create table 表名

+ 修改表名

  + alter table user1 rename to user11；

  + alter table user1 rename as user11；

  + alter table user1 rename user11；
  + rename table user1 to user10；

+ 添加和删除字段

  + 添加: add

    alter table tbl_name add 字段名称 字段类型[约束条件] [first|after 字段名]

  + 删除：drop

    alter table tbl_name drop 字段名

    alter table tbl_name drop 字段名,drop 字段名....：一次删除多个字段

+ 修改字段

  alter table tbl_name modify 字段名称 字段类型 [完整性约束] [first|after]

+ 修改字段名称

  alter table tbl_name change 旧字段名称 新字段名称 字段类型 [完整性约束] [first|after]

+ 添加和删除默认值：set/drop
  + 添加：alter table tbl_name alter 字段名称 set default 默认值
  + 删除：alter table tbl_name alter 字段名称 drop default 
+ 添加删除主键
  + 添加：alter table tbl_name add [constraint [symbol]] primary key[index_type] (字段名称 ,....)
  + 删除：alter table tbl_name drop primary key
  + 主键有自增长时 ，需要先去掉自增长属性，再进行删除

+ 添加删除唯一

  + 添加：alter table tbl_name add [constraint [symbol]] unique[index|key] [索引名称] (字段名称 ,....)

  + 删除：alter table tbl_name drop {index|key} index_name

+ 修改表的存储引擎

  alter table tbl_name engine = 存储引擎名称

+ 设置自增长的值

  alter table tbl_name auto_increment = 自增长的值

+ 删除数据表

  drop table [if exists] tbl_name[,tbl_name......]



#### 表数据操作 DML

1、插入数据

+ 不指定具体的字段名

  insert [into] tbl_name values|value(值1,.....)

+ 列出指定字段

  insert [into] tbl_name(字段名1,.....) values|value(值1,.....)

+ 同时插入多条记录

  insert [into] tbl_name [(字段名1,.....)]  values(值,.....),(值....).....

+ 通过set形式插入记录

  insert [into] tbl_name set 字段名称=值,.....

+ 将查询结果插入到表中

  insert [into] tbl_name[(字段名称,......)] select 字段名称 from tbl_name[where条件]

2、更新数据

update tbl_name set 字段名称=值,.....[where 条件] [order by  字段名] [limit 限制条数]

3、删除数据

delete from tbl_name [where 条件] [order by 字段名] [limit 限制条数]

删除数据时，带有自增长属性的下次再重新插入值时，会从之前的最大增长值加1，除非重新设置自增长值

彻底清空数据表：truncate [table] tbl_name,不能有where条件



#### 查询数据操作 DQL

1、单表查询：

select select_expr[,select_expr查询表达式] 

[from tbl_name 

[where 条件]

 [group by {col_name|position} [asc|desc],....分组]

[having 条件 对分组结果二次筛选]

[order by {col_name|position} [asc|desc],......排序]

[limit 限制显示条数]

 ]

+ 查询表达式

  + 至少有一列，可以有多列
  + *表示所有字段，table_name.字段为哪个表中的字段
  + 可以使用[as] alias_name 为其赋予别名

+ 去除重复的值

  select distinct * from user;

+ where条件查询

  + 比较运算符：=、>= 、<=、> 、<、!=、!>、!<、<=>可以检测 null 值

  + 指定范围: between and   / not between and

  + 指定集合: in /not in

  + 匹配字符串: like \not  like

    %: 代表 0 个一个或多个任意字符串

    _: 代表1个任意字符

  + 是否为控制: is null \ is not null

  + 多个查询条件: and\or 

+ 分组查询: group by

  + group by:
    + select * from user group by sex
    + select * from user group by sex,id  多个条件分组

  注意:如果使用 group by 出现以下错误:

  ```
  Expression #1 of SELECT list is not in GROUP BY clause and contains nonaggregated column 'test.user.id' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by
   出错原因:5.7版本中默认以only_full_group_by sql模式(sql严格模式),也就是select后的查询表达式只能是group by后面的或由聚合函数得到的
   
   解决办法:
   1.查看sql_mode:select version(),@@sql_mode
  mysql> select version(),@@sql_mode;
  +-----------+-------------------------------------------------------------------------------------------------------------------------------------------+
  | version() | @@sql_mode                                                                                                                                |
  +-----------+-------------------------------------------------------------------------------------------------------------------------------------------+
  | 5.7.27    | ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION |
  
  2.将ONLY_FULL_GROUP_BY删除:
  set sql_mode=(select replace(@@sql_mode,‘ONLY_FULL_GROUP_BY’,’’));
  ```

  

  + 配合 group_concat() 得到分组详情

    select proID,group_concat(username)from user group by sex;

  + 配合聚合函数

    + count()

      + count(*) 表示统计总记录数

        select proID,group_concat(username),count(*)from user group by sex;

      + count(字段名): 不统计 null 值

    + max()

    + min()

    + avg()

    + sum()

  + 配合 with rollup 记录上面的所有记录总和:对分组之后再进行统计

    select proID,group_concat(username),count(*)from user group by sex with rollup;

+ having语句对分组结果进行二次筛选

  + 只能再分组之后
  + select proID,group_concat(username),count(*) from user group by sex having count (*) >2;

+ order by: 对结果排序

  + desc 降序

  + asc 升序:默认排序方式

  + 可以使用字段位置进行排序

  + 可以多个字段排序,如果第一个字段值一样会根据其他字段进行排序

  + 随机记录查询,配合 rand() 函数

    select * from user order by rand();

+ limit 限制条数,再更新时只能写一个参数

  + limit 显示条数

    select  * from user limit 10;显示 10 条

  + limit 偏移量,显示条数

    select  * from user limit 0,1;   第一条偏移量为 0,可以实现分页



2、多表连接查询

连接查询:将 2 个或 2 个以上的表按照某个条件连接起来,从中选取需要的数据

+ 内连接查询:查询多个表中符合连接条件的数据记录

  + join|cross join |inner join

  + 通过 on 连接条件

  + 显示两个表中符合连接条件的记录

    select u.id,u.username,u.sex,p.proName from user as u inner join provinces on u.proId= p.id; 

+ 外连接查询

  + 左外连接

    left [outer] join: 显示左表的全部记录及右表符合连接条件的记录,当没有记录时以 null 显示
    select u.id,u.username,u.sex,p.proName from user as u left join provinces on u.proId= p.id;

  + 右外连接

    right [outer] join:显示右表的全部记录及左表符合连接条件的记录,当没有记录时以 null 显示
    select u.id,u.username,u.sex,p.proName from user as u right join provinces on u.proId= p.id;

+ 外键:保证数据的一致性和完整性
  + 参照已有表的主键,将主表与子表建立关联关系,父表对记录进行操作时,子表中与之对应的信息也应有相应的改变
  
  + 可以实现一对一或一对多的关系
  
  + 注意:
    + 父子表应该使用相同的存储引擎,而且禁止使用临时表
    + 存储引擎只能为 Innodb
    + 外键列和参照列必须创建索引,如果外键列不存在索引,mysql 会自动创建索引列
    + 外键列和参照列必须具有相似的数据类型,其中数字的长度或是否有符号位必须相同,而字符的长度可以不同
    
  + 外键约束的参照操作
    + cascade:从父表删除或更新且自动删除或更新子表中匹配的行
    
      forioreign key (字段名) references 参照的表名(参照表的字段名) on delete|on update cascade
    
    + set null:从父表删除或更新,并设置子表中的外键列位 null.如果使用该选项,必须保证子表列没有指定 not null
    + restrict:拒绝对父表的删除或更新操作
    + no action:标准 SQL 的关键字,再 Mysql 中与 restrict 相同
    
  + 添加删除外键
  
    + 添加：
  
      + foreign key (字段名) references 参照的表名(参照表的字段名) ：没有指定外键名称mysql 会自动创建外键名称
  
      + constraint 外键别名  foreign key (字段名) references 参照的表名(参照表的字段名) 
  
      + 动态添加：
  
        alter table tbl_name add constraint 外键名称 foreign key (字段名) references 参照的表名(参照表的字段名)
  
    + 删除
  
      alter table tbl_name drop   foreign key  外键名称



3、联合查询

+ 查询的字段数要相同

+ union：会去掉相同的记录

  select username from employee union select username from cms_user；

+ union all：简单的合并到一起

  select username from employee union all select username from cms_user；



4、子查询

+ 子查询：将一个查询语句嵌套在另一个查询语句中，内层查询语句的查询结果可以为外层查询语句提供条件

+ 子查询的情况：

  + 由 [not] in 引发的子查询

    select username from user where in(select ........)

  + 通过比较运算符引发的子查询

    select username from user where score>=(select ........)

  + 使用 [not] exists 引发的子查询

    select username from user where exists(select ........)

  + 使用 any|some 或 all 引发的子查询

    ```
    关键字           any            some            all
    >、>=           最小值          最小值           最大值
    <、<=           最大值          最大值           最小值
    =               任意值          任意值             
    <>、!=                                         任意值
    ```

    select username from user where score>=any|some(select ........)

+ 将查询结果写入数据表中

  insert [into] tbl_name [(col_name,.....)] select.......

+ 创建数据表同时将查询结果写入到数据表

  create table [if not exists] tbl_name [(create_definition,....)] select .....



5、正则表达式查询

+ regexp 的匹配方式

  select * from user where username regexp '^t';

+ 常用匹配方式

  ```
  模式字符                       含义
  ^                            匹配字符的开始部分
  $                            匹配字符的结尾部分
  .                            代表字符串中的任意一个字符，包括回车和换行
  [字符集合]                     匹配字符集合中的任意一个字符
  [^字符集合]                   匹配除了字符集合以外的任意一个字符
  s1|s2|s3                    匹配s1、s2、s3中任意一个字符
  *                           代表0个、1个或多个其前的字符
  +                           代表1个或多个其前的字符
  string{N}                   字符串出现n次
  字符串{M,N}                  字符串至少出现M次，最多N次
  ```

  



6、mysql 中的运算符使用

+ 算术运算符

  + +、-、*、/（除数为0时，结果为null）、div（除法，x1 div x2）
  + %取余、mod（取余，mod(x1，x2)）

+ 比较运算符

  + < 、> 、!=、.....
  + is null /is notnull
  + between and
  + in /not in
  + like / not like
  + regexp

+ 逻辑运算符

  + &&/and
  + ||/or
  + ！/not：取反
  + xor：异或

+ 运算符的优先级

  ```
  优先级       运算符
  1           ！
  2           ~
  3           ^
  4           *、/、div、%、mod
  5           +、-
  6           >>、<<
  7           &
  8           |
  9           =、<=>、<、>...、in、is null、like、regexp
  10          between and、case、when、then、else
  11          not
  12          &&、and
  13          ||、or、xor
  14          ;=
  ```

  可以通过()改变优先级



7、数据函数库

+ 数学函数

  ```  
  名称              描述
  ceil()          进一取整
  floor()         舍一取整
  mod             取余数
  power()         幂运算
  round()         四舍五入
  truncate()      数字截取
  abs()           取绝对值
  pi()            圆周率
  rand()/rand(x)  返回0-1之间随机数
  sign(x)         返回x的符号，x为负数返回-1，0返回0，正数返回1
  exp(x)          计算e的几次方
  ```

+ 字符串函数

  ```
  名称                      描述
  char_length(s)           返回字符串的字符数
  length                   返回字符串的长度
  concatenate（s1，s2）     将指定字符串合并为一个字符串
  concat_ws(x，s1，s2）     以指定分隔符连接字符串
  upper（s）/ucase(s)       将字符串转换为大写
  lower(s)/lcase(s)        将字符串转换为小写
  left(s,n)/right(s,n)     返回字符串的前/后那个字符
  lpad(s1,len,s2)/rpad(s1,len,s2)   将字符串s1用s2 填充到指定len
  ltrim(s)/rtrim(s)/trim(s)   去掉字符串中的空格
  trim(s1 from s)          去掉字符串s中开始和结尾处的字符串s1
  repeat(s,n)              重复字符串指定次数
  space(n)                 返回n个空格
  replace(s,s1,s2)         将字符串s中的s1替换成s2
  strcmp(s1,s2)            比较字符串>=<分别返回1，0，-1
  substring(s,n,len)       截取字符串
  reverse(s)               反转字符串
  elt(n,s1,s2....)         返回指定位置的字符串
  
  ```

+ 日期时间函数

  ```
  名称                                      描述
  curdate(),current_date()                 返回当前日期
  curtime(),current_time()                 返回当前时间
  now()                                    返回当前日期和时间
  month(d)                                 返回日期中月份的值
  monthname(d)                             返回日期中月份的名称
  dayname(d)                                返回日期是几，monday
  dayofweek(d)                             返回一周内的第几天，1代表星期日
  weekday(d)                               返回日期是星期几，0代表星期一
  week(d)                                  一年中的第多少个星期
  year(d)                                 返回年份值
  hour(t)                                 返回小时值
  minute(t)                               返回分钟值
  second(t)                               返回秒数
  datediff(d1,d2)                         计算2个日期之间相隔的天数
  ```

+ 条件判断函数

  ```
  名称                         描述
  if(expr,v1,v2)              如果表达式expr成立，返回结果v1，否则返回v2
  ifnull(v1,v2)               如果v1的值部位空，就显示v1的值，否则显示v2
  case when exp1 then v1      case表示函数开始，end表示函数结束。如果表达式
  [when exp2 then v2]         exp1成立时，返回v1。如果表达式exp2成立时，返回
  [else vn]                   v2的值，依此类推，最后遇到else时，返回vn的值
  end
  ```

+ 系统信息函数

  ```
  名称                            描述
  version()                      返回数据库的版本号
  connection_id()                返回服务器的连接数
  database(),schema()            返回当前数据库名
  user(),system_user()           返回当前用户
  current_user(),current_user    返回当前函数
  charset(str)                   返回字符串str的字符集
  collation(str)                 返回字符串str的校验字符集
  last_insert_id()               返回最近生成的auto_increment值
  ```

+ 加密函数

  ```
  名称                            描述
  MD5(str)                       信息摘要算法
  password(str)                  密码算法
  encode(str,pwd_str)            加盟结果是一二进制数，必须使用blob类型字段保存
  decode(crypt_str,pwd_str)      对通过encode加密之后的内容解密
  ```

+  其他常用函数

  ```
  名称                             描述
  format(x,n)                     将数字x进行格式化，将x保留到小数点后n位
  ascll(s)                        返回字符串s的第一个字符的ascll码
  bin(x)                          返回x的二进制编码
  hex(x)                          返回x的十六进制编码
  oct(x)                          返回x的八进制编码
  conv(x,f1,f2)                   将x从f1进制数变成f2进制数
  inet_aton(ip)                   将IP地址转换为数字
  inet_ntoa(n)                    将数字转换成IP地址
  get_loct(name,time)             定义锁
  release_lock(name)              解锁
  ```



8、索引的使用

+ 什么是索引

  + 索引由数据库中一列或多列组合而成，其作用是提高对表中数据的查询速度
  + 其优点是可以提高检索数据的速度
  + 缺点是创建和维护索引需要耗费时间
  + 可以提高查询速度，会减慢写入速度

+ 索引的分类

  + 普通索引
  + 唯一索引
  + 全文索引
  + 单列索引
  + 多列索引
  + 空间索引：必须是空间类型 geometry，且该空间类型必须不为空

+ 索引创建

  + 创建表时创建

    create table tbl_name(字段名称 字段类型[完整性约束条件]，

    ....,

    [unique|fulltext|spatial] index|key [索引名称] (字段名称 [( 长度)] [asc|desc])

    )  

  + 在已存在的表上创建索引

    + create [unique|fulltext|spatial] index 索引名称 on 表名 {字段名[(长度)] [asc|desc]}
    + alter table tbl_name add [unique|fulltext|spatial] index 索引名称(字段名称[(长度)] [asc|desc])
  + 全文索引查询
    select * from tbl_name where match(要查询的列名) against（'查询匹配的字符'）

+ 删除索引

  drop index 索引名称 on tbl_name

  alter table tbl_name drop index 索引名称
