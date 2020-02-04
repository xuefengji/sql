# Mysql进阶

1、mysql编码设定

+ 查看服务端编码格式

  show variables like 'char%'

  修改server端编码：修改my.ini中编码格式都要修改成utf8，永久有效

+ set names ‘utf8’：将客户端、连接等编码格式，只会对当前连接有效

+ 修改表的编码格式

  alter table tbl_name chartset set utf8；

  设置字段的编码：

   alter table tbl_name change 旧字段名称 新字段名称 字段类型character set utf8 ;

+ 解决拥有多张表的字符编码问题

  + 导出

    mysqldump -uroot -p --default-character-set-utf8 -d 数据库名> 保存路径

  + 将数据库表的数据导出

    mysqldump -uroot -p --quick --no-create-info --extended-insert --default-character-set=utf8 companys>路径

  + 删除原有的数据库表

  + 重新创建数据库表

  + 导入：

    mysql -u root -p companys < 刚保存的文件

    导入数据时需要在数据文件中，加入set names 'utf8';



2、会话变量和全局变量

+ 会话变量
  + 查看会话变量

    show session variables like 'auto%'或select @@session.autocommit

  + 设置变量值

    set autocommit = ‘off’；或 set @@session.autocommit = 'utf8';

    设置的值只对当前会话有效

+ 全局变量

  + 查看全局变量

    show global variables like 'auto%';或select @@global.autocommit

  + 设置全局变量

    set global autocommit = 'utf8'或 set @@global.autocommit = 'utf8';

    设置的值对所有用户有效



3、存储过程的创建

+ 什么是存储过程

  常用的sql语句在执行时需要先编译，在执行，而存储过程是一组为了完成特定功能的sql语句集，经编译后存储在数据库中，用户通过指定存储过程的名字并给定参数（如果有参数）来调用执行。

  优点：

  + 增强了sql语言的功能和灵活性，可以用流程控制语句的编写，有很强的灵活性，可以完成复杂的判断和较复杂的运算
  + 允许标准组件式编程。存储过程被创建后，可以在程序中被多次调用，不必重新编写sql语句，数据库专业人员可以随时对存储过程进行修改，对应用程序源代码毫无影响
  + 存储过程能实现较快的执行速度。如果一台操作包含大量的transaction-sql代码或分别被多次执行，那么存储过程要比批处理的执行速度快很多。存储过程时预编译的，在首次运行一个存储过程时查询，优化器对其进行分析优化，并给出最终被存储在系统表中的执行计划，而大量的transaction-sql在执行时都要进行编译和优化，速度比较慢
  + 存储过程能减少网络流量
  + 可被作为一种安全机制来充分利用

+ 创建存储过程

  + 选中一个数据库

    use 数据库名

  + 改变分隔符，不让；作为执行结束的标记

    delimiter $$ ;

  + 创建

    create procedure p_hello(存储过程名称)()

     begin

    select 'hello';

    select 'world';

    end

    $$;

  + 改变分隔符 delimiter ;

  + 调用存储过程

    call  p_helo;
    
  + 定义变量
  
    + create procedure p_hello(存储过程名称)()
  
       begin
  
      declare 变量名 数据类型 [default 默认值]
  
      end
  
    + create procedure p_hello(存储过程名称)()
  
       begin
  
      declare 变量名 数据类型 
  
      set 变量名=值
  
      end
  
    + 参数
  
      + in：输入参数，必须在调用存储过程前指定，在存储过程中修改的值在调用结束后不能被返回，只会返回调用之前设置的值。调用存储过程的时候，会copy一份参数的值给存储过程使用
  
        create procedure p_hello(存储过程名称)(in 参数名  参数类型)
  
         begin
  
        有参数的存储过程在调用前 需要先设置参数值
  
        set @参数名 = 值
  
        call 存储过程名称(@参数名)
  
      + out：输出参数：可在存储过程内部改变，并返回
  
        在调用存储过程之前设置的值，调用时查询显示为null，只会显示在存储过程中设置的值
  
      + inout：输入输出参数，可在调用时指定，并可修改和返回
  
        调用之前设置的值，在存储过程被改变后，调用结束，显示的是改变后的值

+ 定义条件和处理

  可以用来定义在处理过程中遇到问题时相应的处理步骤

  declare continue handler for sqlstate '错误代码值' set 变量=变量值

+ 存储过程管理

  + 查看存储过程

    show procedure status where db=’数据库‘;

  + 查看当前数据库下的存储过程列表

    select specific_name from mysql.proc;

  + 查看存储过程内容

    select specific_name,body from mysql.proc;

    或show create procedure 存储过程名字

  + 删除存储过程

    drop procedure if exists 存储过程名称；

  + 修改存储过程

    alter {procedure| function} 存储过程名称 [characteristic....]

    characteristic:

    {condition sql | no sql：表示子程序不包含sql语句|reads sql data：表示子程序中包含读数据的语句|modifies sql data：表示子程序中包含写数据的语句} |

    sql security {definer表示只有定义者才能执行|invoker表示调用者可以执行} :指明权限|

    comment 'string'：表示注释信息

4、控制流程

+ if...else..

  if search_condition then statement_list

  [elseif search_condition then statement_list]....

  [else statement_list]

  end if

+ case分支

  case  case_value 

  when when_value then statement_list

  [when when_value then statement_list].....

  [else statement_list]

  end case

  其中，case_value表示条件判断的变量

  when_value表示变量的取值

  statement_list表示

+ ifnull(exp1,exp2)

  如果exp1是空值，显示的是exp2值，如果不是空值，显示的是exp1值

+ 循环语句

  + while循环

    while 条件 do

    ​	内容

    end while；

  + repeat

    repeat 

    ​	内容 

    ​	until 条件    //循环退出条件

    end repeat；

  + loop

    loop名字：loop

    ​	内容

    if 条件 then

    ​	leave loop名字；

    end if；

    end loop；

5、函数创建

+ 查看是否已经开启了创建函数的功能

  show variables like '%fun%';

+ 如果变量的值为off，需要开启

  set global log_bin_trust_function_creators = 1；

+ 创建函数的语法

  create function 函数名(变量1，变量2.......)

  returns  数据类型

  begin

  ​	......执行的程序代码

  return 数据；

  end；



6、视图创建

+ 什么是视图

  视图是由查询结果形成的一张虚拟表

+ 什么时候要用到视图

  如果某个查询结果出现的非常频繁，也就是，要经常拿这个查询结果来做子查询

+ 使用视图的好处

  + 简化查询语句

    案例：比如求每个人的平均工资

  + 可以进行权限控制

    把表的权限封闭，但是开放相应的视图权限，视图里只开放部分数据列

    比如把员工的工资表的权限关闭，但是开放查询员工发放工资的次数的权限

  + 大数据表分表的时候

    比如某张表的数据有100万条，那么可以将这张表分成4个视图

+ 创建视图语法

  create [or replace] [algorithm ={undefined|merge|temptable}]

  view view_name [(column_list)]

  as select_statement

  [with [cascaded|local] check option]

  如果给定了or replace子句，该语句还能替换已有的视图。select_statement是一种select语句，它给出了视图的定义。该语句可从基表或其他视图进行选择。

+ 视图的管理

  + 视图的存放位置

    存放在information_schema数据库下的views表里

  + 视图的算法

    + merge

      合并的执行方式，每当执行的时候，先将我们视图的sql语句与外部查询视图的sql语句，混合在一起，最终执行

    + temptable

      临时表模式，每当查询的时候，将视图所使用的select语句生成一个结果的临时表，再在当前的临时表内进行查询

      指的是一个视图在什么时候执行，依据哪些方式执行

      对应merge，会将引用视图的语句的文本与视图定义合并起来，使得视图定义的某一部分取代语句的对应部分

      对于temptable，视图的结果将被置于临时表中，然后使用它执行语句

      对于undefined，MySQL将选择所要使用的算法，如果可能，它倾向于merge而不是temptable，这是因为merge通常更有效，如果使用了临时表，视图是不可更新

  + 视图管理

    + 查看视图定义

      show table  status [from 数据库名称] [like '匹配']；

    + 删除视图

      drop view [if exists] view_name [restrict|cascade]

      只能删除视图的定义，不能删除数据，必须有drop权限

    + 查看权限

      select Drop_priv from mysql.user where user='root';

    + 删除视图

      drop view if exists worker_view1;

      删多个：

      drop view if exists department_view1,department_view2;

    + 视图数据更新问题

      某些视图是可更新的，在诸如update】delete或insert等语句中使用它们，以更新基表的内容。对于可更新的视图，在视图中的行和基表中的行之间必须具有一对一的关系。还有一些特定的其他结构，这些结构会使得视图不可更新，如下：

      + 聚合函数：sum、min、max、count等
      + distinct
      + group by
      + having
      + union 或union all
      + 位于选择列表中的子查询
      + join
      + from字句中的不可更新视图
      + where字句中的子查询，引用from字句中的表
      + 仅引用文字值，该情况下没有必要更新基表
      + algorithm=temptable

    + 关于with check option的理解及应用

      更新视图，更新的数据必须要满足视图的条件，才能更新到基表中

7、触发器

+ 什么是触发器

  是一种特殊的存储过程，它在插入、删除或修改特定表中的数据时触发执行，比数据库本身标准的功能有更精细和更复杂的数据控制能力

+ 特性

  监视地点：一般就是表名

  监视时间：update/insert/delete

  触发时间：after/before

  触发事件：update/insert/delete

  注意：不能被直接调用，是由数据库主动去执行

+ 创建语法

  create trigger trigger_name trigger_time trigger_event on tbl_name for each row trigger_stmt(begin ......)

  tbl_name:必须是永久性表，不能将触发程序与temporary表或视图关联

  trigger_time:是触发程序的动作事件，可以是before或after，以指明触发程序是在激活它的语句之前或之后触发

  trigger_event：指明了激活触发程序的语句类型，可以是以下：

  insert：将新行插入表时激活触发程序，如：insert、load data 和replace语句

  update：更改某一行时激活，如update

  delete：从表中删除某一行时激活，如delete和replace

  注意：trigger_event与表操作方式激活触发程序的sql语句并不很类似，如：

  关于insert的before触发程序不仅能被insert语句激活，也能被load data语句激活

  特别说明：

  1、对于insert而言，新插入的行用new来表示，行中的每一列的值用new.列名来表示

  2、对于delete而言，原本由一行后被删除，想引用被删除的这一行，用old表示，old.列名可以引用被删除的行的值

  3、对于update而言，被修改的行在被修改前的数据用old表示，old.列名引用被修改之前行中的值，修改后的数据用new表示，new.列名引用被修改之后行中的值

+ 触发器管理：

  + 查看所有触发器

    show triggers;

  + mysql中有一个information_schema.triggers表，存储所有库中的所有触发器，desc information_schema.trigger可以查看触发器结构

  + 查看触发器名字

    select * from information_schema.trigger where trigger_name ='触发器名字'\G;

  + 删除触发器

    drop trigger [schema_name.]trigger_name;



8、sql锁

不同的引擎支持不同的锁：MyISAM 和memory采用的是表级锁；bdb采用的是页面锁，也支持表级锁；Innodb支持行级锁，也支持表级锁，但默认情况下采用的是行级锁

mysql3种锁的特性：

表级锁：开销小、加锁快，不会出现死锁，锁定力度大，发生锁冲突的概率最高，并发度最低

