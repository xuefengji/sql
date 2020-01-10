# Mysql

#### 简介

1、what：用来存储数据的仓库，以一定的形式存在磁盘上

2、数据库系统：由数据库管理系统、数据库、应用开发软件组成

#### 简单操作
1、打开数据库
cmd形式：mysql  -h host -u root -p 
修改提示符：mysql  -h host -u root -p --prompt 要修改的提示符
查看数据库：show database

2、数据库简单操作
create：
create database [if not exsits] maizi [[default] chart-set utf-8];
删除：
drop database maizi；
use：
use maizi

#### 数据类型
