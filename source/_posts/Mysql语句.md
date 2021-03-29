---
title: Mysql语句
description: '-'
tags:
  - 数据库
  - mysql
abbrlink: 9b2a9201
date: 2020-12-15 18:50:26
---



数据库连接

mysql -u['username'] -p['password']

数据库退出

exit

数据库操作

创建：create database test(数据库名)  DEFAULT CHARACTER SET UTF8 COLLATE UTF8_GENERAL_CI;

修改：alter database test

​      deafault character set gb2312 (修改默认字符集为gb2312)

​      deafalut collate gb2312_chinese_ci; (修改默认校对规则为gb2312_chinese_ci)

删除：drop database test;

查看：show databases [like 'test'];

​          select database();查看当前使用的数据库

使用数据库：use test  （不用分号）



表操作

创建：create table students

​          (id char(10) not null primary key,

​           name char(6) not null ，

​           sex char(2) not null default 0，

​            major char(20 not null));---列名 数据类型 约束

更新：alter table test.student

​      add column city char(20) default null       （添加列）

​      change column  city home char(20) not null  （可同时修改表中指定列的名称和数据类型）

​      modify column id int first/after             (修改指定列的数据类型，first/after修改指定列在表中的位置)

​      alter column city set default '广州';        (可修改或删除表中指定列的默认值)

​      drop column city;                           （删除列）

​      rename to test.university_student;           (为表student重命名为university_student)

重命名：rename table test.university_student to student;

复制：create table test.student_copy like test.student;    (复制表结构)

​        create table test.student_copy as test.student;      (复制表结构和内容，但是索引和完整性约束不会被复制)

删除：drop table test.student;

查看：show tables;                  (查看当前数据库的所有表名称)

​      show columns from student; | desc 表名;    (查看表结构)

插入数据：insert into test.student values('001','lwk','0','计算科学');

删除数据：delete from test.student where id='001';    (不指定where将删除所有数据)

修改数据：updata test.student set id='002' where name='lwk';



数据库的查询

查询表达式结果：select 2+3;  (可得到2+3的计算结果5 注：select 'c'+'d';输出0）

查询表数据：select id,name from student;

替换查询结果数据： select sex,

​                   case

​                       when sex='0'then '男'

​                       else '女'

​                   end [as '性别']             (as '性别'是给列sex取列别名)

​                   from test.student;          （注意这里还是一个select…from…子句）

高级查询和sqlserver一样（嵌套查询和join连接）

嵌套查询：select 城市 from lwk.仓库  where 仓库号 in (select 仓库号 from lwk.职工 where 工资>5000)

join连接：select 供应商名 from lwk.供应商 join lwk.订购单 on 供应商.供应商号=订购单.供应商号 join lwk.职工 on 职工.职工号=订购单.职工号 join lwk.仓库 on 仓库.仓库号=职工.仓库号 where 地址='韶关' and 城市='广州'



字符串、文本匹配

通配符匹配：select name from student where major like('计%')                                  （%代表多个字符 _代表一个字符）

正则表达式匹配：select name from student where major [not] regexp|rlike '计';            (基本字符匹配，与百分号通配符效果类似)

​                select name from student where id [not] regexp|rlike '计算科学|信息计算';     （选择匹配）

​                select name from student where major [not] regexp|rlike '[1-9]';                   （范围匹配）



group by …having…子句

select major ,cout(*) 总人数 from student group by student.major having cout(*)>2;



order by 子句 (asc升序|desc降序)

asc和desc不能影响到逗号外的列，默认是asc

select * from app_public_feedback order by created_on desc limit 0,10;



limit 子句

select id,name from student limit 2,4;                    (从第3行开始取4行数据)

select id,name from student limit 4 offset 2;             (从第3行开始取4行数据)



视图

创建：create view test.id_view as select id from student where sex='0';

修改：alter view test.name_view as select name from student where sex='0';

查看：show create view name_view;

更新：update/delete/insert(和表的使用方法类似，把表名改成视图名就好)

​      如果视图包含 聚合函数 distinct关键字 group by子句 having子句 union运算符        位于选择列表中的子查询    from子句中的不可更新视图或包含多个表 where子句中的子查询，引用from子句中的表 时视图不可更新

删除：drop view name_view;



触发器

创建：create trigger test.student_trigger <before|after> <insert| updata| delete> on test.student for each row <主体>  （主体如果要执行多个语句，要使用begin…end复合结构）

​      例如：create trigger test.student_insert after insert on test.student for each row set @str='add a new student';

删除：drop trigger [if exists] test.student_insert;

当触发器涉及对表自身的更新操作时，只能使用before触发器。

old表中的值全部是只读的。（new操作后的表，old操作前的表）

load data语句能激活before insert触发器。

每个表最多有六个触发器，每种各一个。每个表每个事件每次只允许一个触发器。

触发器不能建立在视图上。



事件   

InnoDB ： 事物型数据库首选引擎

需要确保MySQL中的event_scheduler已开启

（查看当前事件调度器是否开启 show variables like 'event_scheduler';|select @@event_scheduler;

​    开启|关闭事件调度器            set global event_scheduler=true|false;）

创建：create event event_name

​      on scheduler every 1 month|year|quarter|day|hour|minute|……

​      do

​      insert into student values('002','路人甲',default,'君子修养');

修改：allter event event_name [rename to event_newname][do <事件主体> ][disable|enable(关闭|开启事件)];

删除：drop event event_name;

事件是基于特定时间周期来触发的

如果不显式地指明，事件在创建后处于关闭状态 



存储过程

创建：create procedure name ([过程参数])<过程体>

​     过程体一般写在begin……end之间

​     过程参数格式： [in|out|inout] <全局参数名><类型>

​       IN 输入参数:表示该参数的值必须在调用存储过程时指定，在存储过程中修改该参数的值不能被返回，为默认值

​       OUT 输出参数:该值可在存储过程内部被改变，并可返回

​       INOUT 输入输出参数:调用时指定，并且可被改变和返回

需要具有create routine权限。

过程体：delimiter $$             (修改命令结束符为$$)

​        declare <变量名> <类型> [default <默认值>]  （声明局部变量，局部变量只能在存储过程体begin…end语句中声明并作用）

​        局部变量与用户变量的区别：局部变量的声明时没有使用@符号，并且只能在begin…end范围内使用；用户变量在声明时需要使用@符，已经声明的变量存在与整个会话中。用户变量使用SET语句定义，局部变量使用DECLARE语句定义。

​     变量声明后可使用set语句为局部变量赋值 （set xname='小马'）



调用：call <存储过程名>[<参数>]

修改：alter procedure <过程名> [<特征>]

删除：drop procedure <过程名>

查看：show create procedure <存储过程>

​           show procedure status（查看数据库中存在哪些存储过程）



游标

声明：declare <游标名> cursor for <select语句>

打开：open <游标名>

读取数据：fetch <游标名> into <变量1>[，变量2，…]

关闭：close <游标名>

注意：！游标只能用于存储过程和存储函数

​          ！游标不是一条select语句，是被select语句检索出来的结果集



存储函数

创建：create function <函数名>[参数1 类型1]

​      returns <类型>

​      <函数主体>

调用：select <存储函数名>[参数1]

删除：drop function<存储函数名>

查看：show create function <函数名>

​           show function status（查看数据库中存在哪些存储过程）



用户账号管理

创建：create user 'luo' [@'localhost'] identified by 'mima';(默认主机名是@'%')

删除：delete from user where User='myuser' and Host='localhost';

修改用户账号：rename 'luo'[@'localhost'] to 'he' [@'localhost'];（语句前he账户不存在)

修改用户口令：select password('mima');                          （单向加密函数）

​              set password for 'he'[@'localhost']=' *98703F7A543944FD3818077669F74D60616DB2C8';

查看所有账户：select user from mysql.user;



用户权限管理

授与：grant all/updata…权限类型 on test.*/test.表名 (*.*代表所有数据库的所有表) to 'he'[@'localhost'];

  末尾加上 with grant option--该账户可将自身权限授予其他账户

  末尾加上 with max_updates_per_hour number等格式 --限制update次数为number

撤销：revoke updata…权限类型 on test.* from 'he'[@'localhost'];

​      revoke all privileges,grant option from user 'he';

查看用户拥有的权限：show grants for 'he'[@'localhost'];



注意：授权包括：创建表、索引、列、视图、存储过程、函数等权限，最小的是列。

​           DELETE 不能授予到操作权限。



备份与恢复

SQL语句备份：select *from test.表名 into outfile 'E:\data.txt'

​             fields                    （fields子句有下面三个亚子句）

​             terminated by ','          (指定字段间逗号分隔)

​             optionally enclosed by '"' （指定字符用双引号标注）

​             lines terminated by '?';   （指定行末用问号结束）



SQL语句恢复：load data infile 'E:\data.txt' into test.表名

​             fields                   

​             terminated by ','         

​             optionally enclosed by '"'

​             lines terminated by '?';   (表结构要相同，恢复的只是数据，结果如果破坏要重新构建结构)



mysqldump程序备份：

导出：

mysqldump -uroot -ppassword db_name > /root/backup/db_name.sql  

(导出部分表：mysqldump -uroot -ppassword db_name --tables table_name1 table_name2 table_name3 > /root/backup/db_name_table123.sql  )

(如果只导出数据加参数 -t  如果只导出结构加参数 -d)

导入：

mysql -uroot -ppassword db_name < /root/backup/db_name.sql

> 导出的表也可以用这个方式导入

参考：https://www.cmsky.com/mysqldump/

复制到本地：scp /root/backup/db_name.sql ion@172.20.1.179:/home/ion/

删除已复制得数据库文件：rm -rf /root/backup/db_name.sql

重启数据库：

service mysqld restart 
service mysql restart (5.5.7版本命令)





下面的是恢复备份是全库备份，使用情况较少。

​              mysqldump -h (主机名) -u(账户名) -p(密码)  数据库名.表名/--database 数据库名/--all-databases >目录位置（保存文件后缀为sql）

​              例子：mysqldump -uroot -p123456 --all-databases>E:\360Downloads;



mysqldump程序恢复：

​              mysqldump -h (主机名) -u(账户名) -p(密码) --replace  数据库名 目录位置



PHP数据库操作

建立连接 :   mysql_connect("服务器名","用户名","密码")

​                   mysql_pconnect("服务器名","用户名","密码")

选择数据库：mysql_select_db("database_name"[,connection])

执行数据库操作： mysql_query(query,[connection])   query可为字符串或者字符串变量

数据的查询：mysql_fetch_array(data[,datatype])

​                     mysql_fetch_row(data)

​                     mysql_fetch_assoc(data)

​                     mysql_fetch_seek(data)

关闭连接：mysql_close()



@$_POST["b"]==添加、修改 or other



PHP 5 及以上版本建议使用mysqli代替mysql来进行数据库操作。 :如  mysql_fetch_row(data)改为mysqli_fetch_row(data)



配置允许其他电脑连接数据库：

sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf 

注释掉   bind-address = 127.0.0.1

登录数据库

mysql -u['username'] -p['password']

使用命令：

grant all on *.* to ['username'] @'['host'] ' identified by ['password'];

*例子：grant all on \*.* to root @172.20.0.150 ' identified by '123456';*

*主机：172.20.0.150         账户：root          密码：123456*

刷新权限并重启mysql：

flush privileges;

/etc/init.d/mysql restart