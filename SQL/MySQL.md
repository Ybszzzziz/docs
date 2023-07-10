# MySQL

## MySQL属于关系型数据库管理软件

### 存储，管理，备份等

### MySQL优势

- 开源

- 免费

- 性能提高

## 安装

### 目录选择

- 软件目录

- 数据目录

### 字符编码

### 端口号

- 3306

### 用户名密码

- 超级管理员

	- root

## MySQL使用

### MySQL作为服务端

- MySQL服务必须先启动，然后其他客户端才能连接

  ServerSocket server = new ServerSocket(3306);
  
  while(true){
  	Socket socket = server.accept();
  	//每一个客户端一个线程
  }
### 客户端

- 命令行cmd

- JDBC程序

- 可视化工具：Navicat，Sqlyog。。。。。

### 客户端连接mysql服务端：

- mysql服务所在的IP地址

- 端口号：3306

- 用户名密码

- 网络协议

	- SQL语法

## MySQL使用命令行连接

### mysql -u 用户名 -p 
Enter Password：输入密码

- 登录成功 mysql>

## MySQL数据类型

### 整型

- xxxint

	- 给int指定宽度必须结合zerofill 和unsigned才有意义

### 浮点型

- float和double

	- 可以指定宽度（M,D）

		- M是指总宽度

		- D是指小数点后几位

### 定点型

- decimal和numeric

### 字符型

- char，varchar，xxxtext

	- char类型

		- 适合于字符串较短，长度固定，修改频繁

		- char如果不指定宽度，默认是1

	- varchar类型

		- 必须指定宽度，最多不能超过几个字符

		- 实际占的宽度  N个字符的宽度 + 1或2个字节

		- 因为每次要计算实际占的字节数，因此varchar效率较低，但是对于不定长的字符串，节省空间

	- xxxtext类型

		- 适合于小摘要

### 日期时间类型

- date, time,  datetime,  year,  timestamp

### 二进制数据

- xxxblob

## MySQL运算符

### 算术运算符

- + ， - ， * ， / （div），%（mod）

### 比较运算符

- > ,< ,>=,<=,=, !=（<>）

### 逻辑运算符

- 与(&&, and)，或(|| , or)，非（not）

### 范围

- between ... and ...

### 集合范围

- in (x1,x2,x3...)

- not in (x1,x2,x3)

### 模糊查询

- like

	- 结合通配符

		- %

			- 表示任意个字符

		- _

			- 一个_表示一个字符

### NULL判断

- is null

- is not null

- 千万不要使用=判断

## MySQL语句

### 数据库操作

- 显示当前mysql软件中，当前用户可以看到的数据库

	- show databases;

- 创建数据库

	- create database 数据库名;

- 删除数据库

	- drop database 数据库名;

- 使用数据库，选择数据库

	- use 数据库名;

- 查看当前数据库下的所有表格

	- show tables;

### 表结构的操作

- 查看某个表的结构

	- desc 表名称;

- 创建表格

	- create table 表名称(
     字段名  数据类型  约束,
    字段名  数据类型  约束, 
   字段名  数据类型  约束
);

		- 子主题 1

		- 子主题 2

- 修改表结构

	- 增加一列

		- alter table 表名称  add 【column】 字段名  数据类型 【not null】【default 默认值】;

	- 删除一列

		- alter table 表名称 drop 【column】 字段名;

	- 修改数据类型，非空约束，默认值

		- alter table 表名称 modify 【column】 字段名  新的数据类型 【not null】【default 默认值】;

	- 修改列名称

		- alter table 表名称 change 【column】 旧字段名 新的列名称  新的数据类型 【not null】【default 默认值】;

	- 表的重名

		- alter table 表名称 rename 新名称;

		- rename  table  表名称 to 新名称;

- 修改约束

	- 增加主键

		- alter table 表名称 add 【约束名】 primary key(字段列表);

	- 删除主键

		- alter table 表名称 drop primary key;

	- 增加唯一性约束

		- alter table 表名称 add 【约束名】 unique (字段列表);

	- 删除唯一性约束

		- alter table 表名称 drop index   约束名;

### 数据的操作

- 增加

	- insert into 表名称(字段列表) values(值列表);

		- 值列表要与字段列表一一对应

	- insert into 表名称  values(值列表);

		- 默认所有列都要插入数据，值列表的数量与顺序要与表结构一一对应

	- insert into 表名称(字段列表） values(值列表), (值列表);

- 修改

	- update 表名称 set 字段名 = 字段值，字段名 = 字段值。。。  where 条件;

- 删除

	- 删除全表的数据

		- delete from 表名称;

		- truncate 表名称;

			- 效率高，但不能回滚

	- 删除部分行

		- delete from 表名称  where 条件;

### 数据的查询

- 查询全部行全部列

	- select * from 表名;

- 查询部分列，所有行

	- select 字段列表  from 表名;

- 查询部分列，部分行

	- select 字段列表  from 表名   where  条件;

## Select的5个子句

### select 字段列表/函数/计算表达式  from 表名 

### where子句

- 从数据库表中直接筛选出满足条件的那些行

### group by

- 分组

	- 如果没有where那么就是对整张表进行分组

	- 如果有where就是对满足where条件的行进行分组

### having

- （1）和where的不同

	- （1）where是在表中直接筛选,having它是在查询的初步结果中再次筛选

	- （2）where后面条件中不能出现  聚合函数 

	- （3）having和group by一起使用

### order by

- 排序

	- 默认是升序

	- 明确指明是升序还是降序

		- order by xxx  desc

			- 降序

		- order by xxx asc

			- 升序

		- select * from employee order by did 【asc】,salary desc;

			- 按部门编号升序，同一个部门的按照薪资的降序排列

### limit

- 参数

	- 第几页

		- page

	- 每页显示几行

		- n

- limit  (page-1)*n , n

## 关联查询

### 从2张表或多张表中一起筛选结果

- 子主题 1

### 内连接

- 隐式的内连接

	- 示例：查询员工的姓名，以及它所在的部门的名称

	- select  ename as "员工的姓名" , dname as "部门的名称" 
 from  employee , department
 where  employee.部门编号  =  department.部门编号;

- 显式的内连接

	- select  ename as "员工的姓名" , dname as "部门的名称"  
from  employee inner join department
 on  employee.部门编号  =  department.部门编号 
where 其他的条件;

### 外连接

- 左外连接

	- 示例：查询所有的部门，以及部门下的员工的姓名和手机号码，包括哪些没有分配部门的员工

	- select 员工的姓名，手机号码 ，部门的名称 
from employee  left 【outer】 join department 
on employee.部门编号 = department.部门编号;

- 右外连接

	- 示例：查询所有的部门，以及部门下的员工的姓名和手机号码，包括哪些没有分配部门的员工

	- select 员工的姓名，手机号码 ，部门的名称 
from department  right 【outer】 join  employee
on employee.部门编号 = department.部门编号;

- 全外连接

	- 不支持full outer join

	- 可以通过 左外连接 union 右外连接

### 交叉连接

- cross join了解

## 子查询

### 当在修改，删除，查询语句中需要嵌套一个select语句，来获取某个值或结果等这样的查询

### 例如：

- （1）把赵柯祥的工资修改为  它所在部门的平均工资

- （2）查询那些比  部门的平均工资低的员工

### 根据子查询嵌套的位置不同，可以分为

- where型

	- 例如：查询全公司工资最高的员工信息

	- select * from employee where salary  = (最高工资值);

	- select * from employee where salary  = (select max(salary) from employee);

	- 例如：只找出该部门有员工的部门信息

	- select * from department where 部门编号 in (员工表的员工所在部门的编号集合)

	- select * from department where 部门编号 in (select 部门编号  from employee where 部门编号 is not null)

- from型

	- 例如：找出比部门平均工资高的员工

	- select * from employee where salary > (该员工所在部门的平均工资)

		- 每一个部门都有自己的平均工资

		- 所有员工的部门各不同

	- select 员工的编号，员工的姓名 
 from employee ,  (每一个部门的平均工资) as temp
on employee.部门编号 = temp.部门编号
where employee.薪资 > temp.该部门平均工资;

## 事务

### 概念

- 事务是数据库运行中的一个逻辑工作单位，要么完全执行，要么完全不执行。

### 特性

- 原子性(Atomicity)

	- 表示一组语句要么同时成功要么同时失败

- 一致性(Consistency) 

	- 回滚后返回到上一次正确状态

- 隔离性 (Isolation) 

	- 多个线程之间事务互不影响

- 持久性(Durability)

	- 一旦提交，不可撤销

### mysql的每一个连接默认都是自动提交事务，每一句语句的执行，就提交一句

### 如果需要手动提交事务

- （1）set autocommit = false/0;

	- 对于整个连接都起作用，整个连接的sql语句都变成手动提交

- （2）start transaction; 

	- 仅仅是对于start transaction; 到 rollback; /commit;之间的语句是手动提交，对于该连接的其他语句还是自动提交

### mysql只有innodb引擎才支持事务

### 多线程事务

- 问题

	- （1）脏读

		- 例如：T1线程中，访问了一个数据，这个数据是T2线程中修改但还未提交的数据

			- 比喻

				- 教师看到了学生答卷还未交卷的答案，在未交卷之前都是可以修改

	- （2）不可重复读

		- 例如：T1线程未结束之前，访问了一个数据，之后又访问这个数据，两次的访问结果不一致，这个数据在T2线程中被修改，并提交了

			- 比喻

				- 教师在巡考过程中看到了学生的答卷的答案，之后这个学生交卷，但是它在交卷之前，修改了这个答案，教师在两次看到这个答案是不一样

	- （3）幻读

		- 例如：T1线程对某个表做查询，同样一个查询语句，第一次查询有n行记录，然后又查询一次时，发现有N+M行记录，发现多了或少了M行，因为T2线程增加了M行或删除了M行

	- 原因：两个或多个线程同时访问同一份数据，就有线程安全问题

- 解决的方案

	- 事务的隔离级别

	- 如果以上三个问题都允许，那么事务的隔离级别就可以是

		- Read uncommitted

	- 如果要避免脏读，允许后两个，那么事务的隔离级别可以是

		- Read committed

	- 如果要避免脏读和不可重复读，但是允许幻读，那么事务的隔离级别可以是

		- Repeatable Read

		- 特殊mysql在这个级别，也会避免幻读

		- mysql默认的事务隔离级别就是这级

	- 如果要避免以上三个问题，事务的隔离级别是

		- Serializable

## 函数

### MySQL提供了很多函数包括
1数学函数 ： 比如: MOD（x,y） 返回x除以y以后的余数
2字符串函数  比如：LENGTH(s)返回字符串的长度 和字符集有关
3日期和时间函数：比如 :now()获取当前日期和时间
4条件判断函数：if(expr,v1,v2);如果expr成立返回v1 否则返回v2，case...when
5系统信息函数
6加密函数
7格式化函数
….


