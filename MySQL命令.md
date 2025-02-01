# MySQL命令

命令行启动/关闭数据库：net start/stop MySQL82

登录命令：mysql -uroot -proot -h<host> -P<port> <databasename>

查看版本号：select version();

退出命令：exit

创建数据库：create database if not exists 数据库名 character set 字符集 collate 排序方式;

查看数据库字符集：show variables like 'character_set_database';

查看数据库排序方式：show variables like 'collection_database';

查看库：show databases;

查看当前使用库：select database();

查看指定库下所有表：show tables from 数据库名；

切换库/选中库：use 数据库名；