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

  + loop

