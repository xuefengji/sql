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