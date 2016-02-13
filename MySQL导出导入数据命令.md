平时开发，常用到MySQL，下面讲一下MySQL的常用数据导入导出操作以及基本的MySQL命令。
###导出整个数据库
>mysqldump -u 用户名 -p 数据库名 > 导出的文件名

例如：

```sh
mysqldump -u root -p test_schema >/Users/meixxd/Downloads/test_schema.sql
```

###导出数据库中的单个表
>mysqldump -u 用户名 -p 数据库名 表名\> 导出的文件名

例如：

```sh
mysqldump -u root -p test_schema score>/Users/meixxd/Downloads/test_schema_score.sql
```

###导出数据库结构
>mysqldump -u 用户名 -p -d 数据库名 > 导出的文件名

>-d代表没有数据

例如：

```sh
mysqldump -u root -p -d test_schema >/Users/meixxd/Downloads/test_schema_d.sql
```

###导入数据库
>source 数据库脚本文件

例如：

```sh
$ mysql -u root -p
...

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> source /Users/meixxd/Downloads/test_schema.sql;
```

---
下面顺便补充一下MySQL的常用命令：
###数据库服务开启、关闭、重启
>mysql.server start/stop/restart

###连接本地数据库
>mysql -u root -p  `代表以root用户登录，按照提示输出密码，即可连接本地数据库`

###连接远程主机数据库
>mysql -h 主机地址 -u root -p  `填写主机IP地址即可，如12.2.1.1`

###退出数据库
>exit

###修改密码
>mysqladmin -u 用户名 -p 旧密码 password 新密码

###显示MySQL中的数据库
>show databases;

###选中使用的数据库
>use 数据库名;  `例如use test_schema;`

###显示数据中的表
>show tables;

###显示数据表结构
>describe 表名;

例如：

```sh
mysql> describe score;
+-------+------------------+------+-----+---------+----------------+
| Field | Type             | Null | Key | Default | Extra          |
+-------+------------------+------+-----+---------+----------------+
| id    | int(11)          | NO   | PRI | NULL    | auto_increment |
| name  | varchar(40)      | NO   |     |         |                |
| score | int(10) unsigned | NO   |     | 0       |                |
+-------+------------------+------+-----+---------+----------------+
3 rows in set (0.01 sec)
```

###建库
>create database 库名;

###删库
>drop database 库名;

###建表
>use 库名;
>create table 表名 (字段设定列表)；

###删表
>drop table 表名;

###其他指令
>其他还有基本的数据库操作指令，例如select、delete等这里不在赘述



