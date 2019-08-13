 1. MySQL 下载、安装、配置:
     1.1 下载地址:https://dev.mysql.com/downloads/mysql/
 ![download_the_mysql_server8.0.17](https://img-blog.csdnimg.cn/20190806230254616.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODg3Mjc3MQ==,size_16,color_FFFFFF,t_70)1.2 解压、配置文件：解压至C盘'web'文件夹,
 在mysql-8.0.17文件夹下新增配置文件"my.ini"：
 

```
[client]
# 设置mysql客户端默认字符集
default-character-set=utf8
 
[mysqld]
# 设置3306端口
port = 3306
# 设置mysql的安装目录
basedir=C:\\web\\mysql-8.0.17
# 设置 mysql数据库的数据的存放目录，MySQL 8+ 不需要以下配置，系统自己生成即可，否则有可能报错
# datadir=C:\\web\\sqldata
# 允许最大连接数
max_connections=20
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
```
1.3数据库初始化、安装、启动

```
#切换目录
cd C:\web\mysql-8.0.17\bin
#数据库初始化，初始化完成后会输出用户名及密码
mysqld --initialize --console
# 数据库安装
mysqld install
#数据库启动
net start mysql




#待解决问题：若根用户密码忘记，再次初始化能否覆盖之前的用户名与密码
#再次初始化，报错信息
C:\web\mysql-8.0.17\bin>mysql --initialize --console
mysql: [ERROR] unknown option '--initialize'.

C:\web\mysql-8.0.17\bin>mysqld --initialize --console
2019-08-06T12:38:51.917931Z 0 [System] [MY-013169] [Server] C:\web\mysql-8.0.17\bin\mysqld.exe (mysqld 8.0.17) initializing of server in progress as process 16740
2019-08-06T12:38:51.923761Z 0 [Warning] [MY-013242] [Server] --character-set-server: 'utf8' is currently an alias for the character set UTF8MB3, but will be an alias for UTF8MB4 in a future release. Please consider using UTF8MB4 in order to be unambiguous.
2019-08-06T12:38:51.925938Z 0 [ERROR] [MY-010457] [Server] --initialize specified but the data directory has files in it. Aborting.
2019-08-06T12:38:51.925976Z 0 [ERROR] [MY-013236] [Server] The designated data directory C:\web\mysql-8.0.17\data\ is unusable. You can remove all files that the server added to it.
2019-08-06T12:38:51.939936Z 0 [ERROR] [MY-010119] [Server] Aborting
2019-08-06T12:38:51.940969Z 0 [System] [MY-010910] [Server] C:\web\mysql-8.0.17\bin\mysqld.exe: Shutdown complete (mysqld 8.0.17)  MySQL Community Server - GPL.

```
1.4 登陆数据库：

```
#按如下格式输入命令行
mysql -h 主机名 -u 用户名 -p  # mysql -u root -p  连接本机数据库
#根据提示输入密码即可登陆

# e.g.
C:\web\mysql-8.0.17\bin>mysql -u root -p
Enter password: ***********
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 8.0.17 MySQL Community Server - GPL

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>



#根用户密码重置，参见博主：https://blog.csdn.net/qq_43342301/article/details/91288891
#本次实践部分代码命令行丢失，信息不完整，可参照上行博主的实践
#首先需要管理员命令行窗口关闭服务
C:\web\mysql-8.0.17\bin>net stop mysql
MySQL 服务正在停止.
MySQL 服务已成功停止。
#第一个命令行窗口不要关闭，打开第二个管理员命令行窗口
C:\WINDOWS\System32>cd C:\web\mysql-8.0.17\bin
C:\web\mysql-8.0.17\bin>mysql
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 7
Server version: 8.0.17 MySQL Community Server - GPL

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
#刷新更改
mysql> FLUSH PRIVILEGES；
Query OK, 0 rows affected (0.03 sec)
#更改根用户登陆密码
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY ‘**************8'；
Query OK, 0 rows affected (0.02 sec)

#root根用户重新登陆MySQL系统
#启动服务
C:\web\mysql-8.0.17\bin>net start mysql
MySQL 服务正在启动 ..
MySQL 服务已经启动成功。
#登陆
C:\web\mysql-8.0.17\bin>mysql -u root -p
Enter password: ***********
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.17 MySQL Community Server - GPL

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

```

 2. NaviCat 12.1.12 下载、安装、破解
 Navicat 与 MySQL Server 之间版本有一定适配性，可能出现报错“ 1251  ”，此报错的可能原因为两款软件“加密方式”的不同。解决办法参考论坛相关帖子“更改加密方式”
 2.1 下载：官网为最新版12.1.20，论坛内可搜索到12.1.12 版本下载链接
 2.2 安装：根据提示，一步步next 即可，安装完后先不要打开软件
 2.3 破解：论坛内提供的破解工具多数无效，破解方法及工具参见Double Sine博主github 链接:
       https://github.com/DoubleLabyrinth/navicat-keygen
       一步步根据博主的示例操作即可，破解成功。博主牛X
 3. NaviCat 12.1.12 连接 MySQL Server 8.0.17
   `按照提示，填写相关信息即可成功连接`![settings_of_navicat_connection](https://img-blog.csdnimg.cn/20190806234222197.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODg3Mjc3MQ==,size_16,color_FFFFFF,t_70)
 4. 数据库基础知识
  4.1定义：数据集合；类比图书馆书籍编号、书架、图书分区、图书分馆
  4.2 关系型数据库：
  关系型与非关系型数据库对比，参见博客：https://blog.csdn.net/aaronthon/article/details/81714528
     最典型的数据结构是表
     ![关系型数据库](https://img-blog.csdnimg.cn/20190807070734817.png)
     非关系型数据库：
     数据结构化存储方法的集合，可以是文档或者键值对等
     ![非关系型数据库](https://img-blog.csdnimg.cn/20190807070910277.png)
     4.3 表、行、列、主键、外键：
     表：表（table）：某种特定类型数据的结构化清单。
     模式：关于数据库和表的布局及特性的信息。
     列（column）：表中的一个字段。所有表都是由一个或多个列组成的。
     行（row）：表中的一个记录。
    主键（primary key）：一列（或一组列），其值能够唯一标识表中每一行。
    外键(FOREIGN KEY）：一个表中的 FOREIGN KEY 指向另一个表中的 PRIMARY KEY。
    
 5. MySQL基础（一）-查询语句
  5.1 导入示例数据库：	
  参考资料：https://www.yiibai.com/mysql/how-to-load-sample-database-into-mysql-database-server.html
  

```
Microsoft Windows [版本 10.0.17134.915]
(c) 2018 Microsoft Corporation。保留所有权利。

C:\Users\arthur>cd C:\web\mysql-8.0.17\bin

C:\web\mysql-8.0.17\bin>mysql -h localhost -u root -p
Enter password: ***********
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 13
Server version: 8.0.17 MySQL Community Server - GPL

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
#创建yiibaidb数据库
mysql> create database if not exists yiibaidb default charset utf8 collate utf8_general_ci;
Query OK, 1 row affected, 2 warnings (0.03 sec)
#调用数据库
mysql> use yiibaidb;
Database changed
#导入表
mysql> source D:/worksp/yiibaidb.sql;
Query OK, 0 rows affected, 1 warning (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 1 row affected, 1 warning (0.01 sec)

Database changed
Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.02 sec)

Query OK, 0 rows affected, 3 warnings (0.10 sec)

Query OK, 1 row affected (0.03 sec)

Query OK, 0 rows affected, 1 warning (0.09 sec)

Query OK, 1 row affected (0.01 sec)

#注意：office 两侧不是中、英状态单引号、双引号
#而是英文输入状态下，数字1左边的符号，查询表
mysql> select city,phone,country from 'offices';
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''offices'' at line 1

mysql> select city,phone,country from `offices`;
+---------------+------------------+-----------+
| city          | phone            | country   |
+---------------+------------------+-----------+
| San Francisco | +1 650 219 4782  | USA       |
| Boston        | +1 215 837 0825  | USA       |
| NYC           | +1 212 555 3000  | USA       |
| Paris         | +33 14 723 4404  | France    |
| Beijing       | +86 33 224 5000  | China     |
| Sydney        | +61 2 9264 2451  | Australia |
| London        | +44 20 7877 2041 | UK        |
+---------------+------------------+-----------+
7 rows in set (0.00 sec)

mysql>
```
5.2 SQL与MySQL：
SQL（发音为字母S-Q-L 或sequel）是Structured Query Language（结构
化查询语言）的缩写。SQL 是一种专门用来与数据库沟通的语言。
 根本的区别是它们遵循的基本原则
 
二者所遵循的基本原则是它们的主要区别：开放vs保守。SQL服务器的狭隘的，保守的存储引擎与MySQL服务器的可扩展，开放的存储引擎绝然不同。虽然你可以使用SQL服务器的Sybase引擎，但MySQL能够提供更多种的选择，如MyISAM,Heap, InnoDB, and BerkeleyDB。MySQL不完全支持陌生的关键词，所以它比SQL服务器要少一些相关的数据库。同时，MySQL也缺乏一些存储程序的功能，比如MyISAM引擎联支持交换功能。（参考资料：https://www.cnblogs.com/qingqing-919/p/8417773.html）

5.3 查询语句： select 与 from
select :  
SQL 语句由简单的英语单词(关键字)构成的。用途：从一个或多个表中检索信息。
 检索多个列时，列名之间以逗号隔开，最后一个列名后不加逗号
 检索全部列时，列名位置以通配符星号（*）代替
 检索结果仅显示唯一值，关键字（distinct）; DISTINCT 关键字作用于所有的列
 检索结果行数限制，关键字（`top 5`, 前5行）（`limit5`, 不超过5行的数据）
      `LIMIT 5 OFFSET 5` 指示MySQL 等DBMS 返回从第5 行起的5 行数据。
第一个数字是检索的行数，第二个数字是指从哪儿开始。
 提示：结束SQL 语句，多条SQL 语句必须以分号（；）分隔。SQL 语句不区分大小写。在处理SQL 语句时，其中所有空格都被忽略。
 5.4 筛选语句 `where` 
根据特定操作或报告的需要提取表数据的子集。只检索所需数据需要指
定搜索条件（search criteria），搜索条件也称为过滤条件（filter condition）。`SELECT` 语句中，数据根据WHERE 子句中指定的搜索条件进行过滤。

5.5高级数据过滤：
5.5.1 操作符：组合`where` 字句  ：SQL 允许给出多个`WHERE` 子句。使用方式：AND 子句或OR 子句的方式使用。
`IN` 操作符用来指定条件范围，范围中的每个条件都可以进行匹配。`IN` 取一组由逗号分隔、括在圆括号中的合法值。
`WHERE` 子句中的`NOT` 操作符有且只有一个功能，那就是否定其后所跟的任何条件
5.5.2通配符
`%`表示任何字符出现任意次数
下划线的用途与%一样，但它只匹配单个字符，而不是多个字符。下划线（`_`）
方括号（`[]`）通配符用来指定一个字符集，它必须匹配指定位置（通配符的位置）的一个字符。

5.6 分组语句
分组是使用`SELECT` 语句的`GROUP BY` 子句建立的
聚集函数
 语句解释
 `HAVING`子句:，`WHERE`过滤行，而`HAVING` 过滤分组。
5.7 排序语句` ORDER BY `
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190807193928344.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODg3Mjc3MQ==,size_16,color_FFFFFF,t_70) 语句解释
 正序、逆序
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2019080719400225.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODg3Mjc3MQ==,size_16,color_FFFFFF,t_70)5.8 函数
 与几乎所有DBMS 都等同地支持SQL 语句（如SELECT）不同，每一个DBMS 都有特定的函数。事实上，只有少数几个函数被所有主要的DBMS等同地支持。虽然所有类型的函数一般都可以在每个DBMS 中使用，但各个函数的名称和语法可能极其不同![在这里插入图片描述](https://img-blog.csdnimg.cn/20190807194100676.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODg3Mjc3MQ==,size_16,color_FFFFFF,t_70)常用的文本处理函数
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190807194220204.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODg3Mjc3MQ==,size_16,color_FFFFFF,t_70)常用的数值处理函数
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190807194258931.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODg3Mjc3MQ==,size_16,color_FFFFFF,t_70)5.9 SQL注释
 使用"#"
  "-- " 
  /* */
  
 5.10 SQL代码规范
  [SQL编程格式的优化建议] [https://zhuanlan.zhihu.com/p/27466166](https://zhuanlan.zhihu.com/p/27466166)
    [SQL Style Guide] [https://www.sqlstyle.guide/](https://www.sqlstyle.guide/)


**作业部分**
task1   查找重复邮箱
创建 email表，并插入如下三行数据
+----+---------+
| Id | Email   |
+----+---------+
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |
+----+---------+

编写一个 SQL 查询，查找 Email 表中所有重复的电子邮箱。
根据以上输入，你的查询应返回以下结果：
+---------+
| Email   |
+---------+
| a@b.com |
+---------+
说明：所有电子邮箱都是小写字母。

```
# 
select email from person
group by email
having count(*) >= 2
```

task2  查找大国
创建如下 World 表
+-----------------+------------+------------+--------------+---------------+
| name            | continent  | area       | population   | gdp           |
+-----------------+------------+------------+--------------+---------------+
| Afghanistan     | Asia       | 652230     | 25500100     | 20343000      |
| Albania         | Europe     | 28748      | 2831741      | 12960000      |
| Algeria         | Africa     | 2381741    | 37100000     | 188681000     |
| Andorra         | Europe     | 468        | 78115        | 3712000       |
| Angola          | Africa     | 1246700    | 20609294     | 100990000     |
+-----------------+------------+------------+--------------+---------------+
如果一个国家的面积超过300万平方公里，或者(人口超过2500万并且gdp超过2000万)，那么这个国家就是大国家。
编写一个SQL查询，输出表中所有大国家的名称、人口和面积。
例如，根据上表，我们应该输出:
+--------------+-------------+--------------+
| name         | population  | area         |
+--------------+-------------+--------------+
| Afghanistan  | 25500100    | 652230       |
| Algeria      | 37100000    | 2381741      |
+--------------+-------------+--------------+

```
SELECT name,population,area 
FROM World
WHERE area > 3000000 OR population > 25000000
```

