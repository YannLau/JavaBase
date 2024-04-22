P731___P820

[toc]

# MySQL安装和配置

略

> 连接到Mysql服务（Mysql数据库）的指令
>
> 登录前,要保证服务启动了
>
> mysql -h 主机IP -P 端口 -u 用户名(root) -p密码
>
> -h 主机IP不写默认本机127.0.0.1 (host)
>
>  -P 端口不写默认用规定的 (port)
>
> 在实际工作中一般会修改默认端口
>
> 老师提醒：
> （1）-p密码不要有空格
> （2）-p后面没有写密码，回车会要求输入密码

> 启动mysql数据库的常用方式：［Dos命令］
>
> 1. 服务方式启动（界面）
>
> 2. net stop mysql服务名
> 3. net start mysql服务名

# Navicat 和 SQLyog

略

# 数据库三层结构-破处MySQL神秘

1. 所谓安装Mysq|数据库，就是在主机安装一个数据库管理系统（DBMS），这个管理程序可以管理多个数据库。DBMS（database manage system）
2. 一个数据库中可以创建多个表，以保存数据（信息）。
3. 数据库管理系统（DBMS）、数据库和表的关系如图所示：示意图

![image-20240226下午64436988](/Users/yannlau/Library/Application Support/typora-user-images/image-20240226下午64436988.png)

行row  列column

<img src="/Users/yannlau/Library/Application Support/typora-user-images/image-20240226下午64558295.png" alt="image-20240226下午64558295" style="zoom:150%;" />

> DDL：数据定义语句 ［create 表，库..］
> DML：数据操作语句 ［增加 insert，修改 update，删除 delete］
> DQL：数据查询语句 ［select ］
> DCL ：数据控制语句［管理数据库：比如用户权限 grant revoke ］

# 创建数据库

> CREATE DATABASE [IF NOT EXISTS] db_name
> [create_specification [, create_specification]...]
> create_specification:
> [DEFAULT] CHARACTER SET charset_name
> [DEFAULT] COLLATE collation_name

> 1. CHARACTER SET： 指定数据库采用的字符集，如果不指定字符集，默认utf8
> 2. COLLATE： 指定数据库字符集的校对规则（常用的 utf8_bin［区分大小写］、utf8_general_ci［不区分大小写］注意默认是 utf8_general_ci） ［举例说明database.sql 文件］

> 练习：
> 1. 创建一个名称为hsp_db01的数据库。［图形化和指令 演示］
> 2. 创建一个使用utf8字符集的hsp_db02数据库
> 3. 创建一个使用utf8字符集，并带校对规则的hsp_db03数据库

```mysql
# 演示数据库的操作
#创建一个名称为hsP_dbO1的数据库。［图形化和指令 演示】
#使用指令创建数据库
CREATE DATABASE hsp_db01;
#删除数据库指令
DROP DATABASE hsp_db01;
#创建一个使用utf8字符集的hsp db02数据库
CREATE DATABASE hsp_db02 CHARACTER SET utf8;
#创建一个使用utf8字符集，并带校对规则的hsP_db03数据库
CREATE DATABASE hsp_db03 CHARACTER SET utf8 COLLATE utf8_bin;
#校对规则 utf8_bin 区分大小 默认utf8_general_ci 不区分天小写
#下面是一条查询的sql，select 查询* 表示所有字段 FROM 从哪个表
#WHERE 从哪个字段 NAME ='tom，查询名字是tom
SELECT *
				FROM t1
				WHERE NAME = 'tom'
```

# 查看/删除数据库

> 显示数据库语句：
> SHOW DATABASES
> 显示数据库创建语句：
> SHOW CREATE DATABASE db_name
> 数据库删除语句 [一定要慎用]：
> DROP DATABASE [IF EXISTS] db_name

```mysql
#演示删除和查询数据库
#查看当前数据库服务器中的所有数据库
SHOW DATABASES
#查看前面创建的hsp_dbO1数据库的定义信息
SHOW CREATE DATABASE hsp_db01
#老师说明 在创建数据库，表的时候，为了规避关键字，可以使用反引号解决
CREATE DATABASE `CREATE`
#删除前面创建的hsp_db01数据库
DROP DATABASE hsp_db01
```

# 备份和恢复数据库

> • 备份数据库（注意：<u>***在DOS执行***</u>）命令行
> mysqldump -u 用户名 -p -B 数据库1 数据库2 数据库n >文件名.sql
> • 恢复数据库（注意：进入SQLyog的命令行再执行）
> Source 文件名.sql
>
> •备份库的<u>***表***</u>
> mysqldump -u用户名 -p密码 数据库 表1 表2 表n > d:\\文件名.sql

```mysql
#练习：database03.sql 备份hsP_db02 和 hsP_db03 库中的效据，并恢复
#备份，要在Dos下执行mysqldump指令其实在mysql安装日录\bin
#这个备份的文件，就是对应的sql语句
mysqldump -u root -p -B hsp_db02 hsp_db03 > d:\\bak.sql
DROP DATABASE hsp_db02;
DROP DATABASE hsp_db03;
#恢复数据库（注意：进入Mysql命令行再执行）
source d:\\bak.sql
#第二个恢复方法，直接将bak.sq1的内容放到查询编辑器中，执行
```

# 创建表

> CREATE TABLE table_name
> (
> 	field1 datatype,
> 	field2 datatype,
> 	field3 datatype
> )character set 字符集 collate 校对规则 engine 存储引擎
> field：指定列名 datatype：指定列类型（字段类型）
> character set：如不指定则为所在数据库字符集
> collate：如不指定则为所在数据库校对规则
> engine：引擎（这个涉及内容较多，后面单独讲解）

```mysql
CREATE TABLE `user` (
	id INT,
	`name` VARCHAR(255),
	`password` VARCHAR(255),
	`birthday` DATE)
	CHARACTER SET utf8 COLLATE utf8_bin ENGINE INNODB;
```

# Mysql常用数据类型(列类型)

![image-20240226下午105123317](/Users/yannlau/Library/Application Support/typora-user-images/image-20240226下午105123317.png)

<img src="/Users/yannlau/Library/Application Support/typora-user-images/image-20240226下午104609059.png" alt="image-20240226下午104609059" style="zoom:200%;" />

![image-20240226下午105108093](/Users/yannlau/Library/Application Support/typora-user-images/image-20240226下午105108093.png)

常用的类型 int double decimal varchar char text datetime timestamp

## 整形的基本使用

> 数值型（整数）的基本使用
>
> 1.说明，使用规范: 在能够满足需求的情况下，尽量选择占用空间小的类型

```mysql
#演示整型的是一个
#老韩使用tinyint 来演示范围 有符号 -128 ~127 如果没有符号 0-255
#说明：表的字符集，校验规则，存储引擎，老师使用默认
#1． 如果没有指定 unsinged，则TINYINT就是有符号
CREATE TABLE t3 (
id TINYINT);
INSERT INTO t3 VAIUES(127);#这是非常简单的添加语句
SELECT * FROM t3
```

> 如何定义无符号整数?
>
> create table t10(id tinyint ）：//默认是有符号的
> create table t11 (id tinyint unsigned）；无符号的

## 数值型bit的使用

```mysql
#演示bit类型使用
#说明
#1. bit(m)  m 在 1-64
#2. 添加数据 范围 按照你给的位数来确定，比如 m = 8 表示一个字节 0~255
#3．显示按照bit
#4．查询时，仍然可以按照数来查询
CREATE TABLE t05（num BIT（8））；
INSERT INTO tO5 VALUES （255）；
SELECT * FROM t05；
SELECT * FROM t05 WHERE num = 1；
```

> 1. 基本使用
>     mysql> create table t05 (num bit(8));
>     mysql> insert into t05 (1, 3);
>     mysql › insert into t05 values(2, 65);
> 2. bit 字段显示时，按照 位的方式显示。
>     查询的时候仍然可以用使用 添加的数值
>     如果一个值只有 0. 1可以考虑使用 bit（1），可以节约空间
>     位类型。M指定位数，默认值1，范围1-64
>     使用不多。

## 数值型(小数)的基本使用

1. FLOAT/DOUBLE [UNSIGNED]
Float 单精度精度，Double 双精度.

2. DECIMAL[M,D] [UNSIGNED]
•可以支持更加精确的小数位。M是小数位数（精度）的总数，D是小数点（标度）后面的位数。
•如果D是0，则值没有小数点或分数部分。M最大65。D最大是30。如果D被省略，默认是0。如果M被省略，默认是10。
•建议：如果希望小数的精度高，推荐使用decimal
3. 案例演示 floatDoubleDec.sql 文件，测试数据 88.12345678912345

```mysql
#演示decimal类型、float、double使用
#创建表
CREATE TABLE t06 (
num] FLOAT, #四舍五入
num2 DOUBLE,
num3 DECIMAL (30,20) ) ;
#添加数据
INSERT INTO t06 VALUES (88.12345678912345, 88.12345678912345, 88.12345678912345) ;
SELECT * FROM t06;

#decima1可以存放很大的数
CREATE TABLE t07 (
num DECIMAL (65)) ;
INSERT INTO t07 VALUES(8999999933338388388383838838383009338388383838383838383) ;
SELECT * FROM t07; # successor

CREATE TABLE t08 (
num BIGINT UNSIGNED)
INSERT INTO t08 VALUES (8999999933338388388383838838383009338388383838383838383) ; #error,加不进去
```

![image-20240227下午25142095](/Users/yannlau/Library/Application Support/typora-user-images/image-20240227下午25142095.png)

## 字符串的基本使用

> 字符串的基本使用
>
> - CHAR(size)
>   固定长度字符串 最大255<u>***字符***</u>
> - VARCHAR(size) 
>   可变长度字符串 最大65532<u>***字节***</u>
>   【utf8编码最大21844字符1-3个字节用于记录大小】
>   应用案例 charVarchar.sql 文件

```mysql
#漢示字符串类型使用char varchar
#注释的快捷键 shift+ctrl+c，取消注释 shift+ctrl+r
-- CHAR (size)
-- 固定长度字符串 最大255字符
-- VARCHAR(size) 0~65535
-- 可变长度字符串 最大65532字节 【utf8编码最大21844字符 1-3个字节用于记录大小】
-- 如果表的编码是 utf8 varchar（size） size = （65535-3）/ 3 = 21844
-- 如果表的编码是 gbk varchar（size）size = （65535-3）/ 2 = 32766
CREATE TABLE t09 ( `name` CHAR(255));
CREATE TABLE t10 ( `name` VARCHAR (32766)) CHARSET gbk;
```

> 字符串使用细节
> 1. 细节1
> char（4） //这个4表示字符数（最大255），不是字节数，不管是中文还是字母都是放四个，按字符计算.
> varchar（4）//这个4表示字符数，不管是字母还是中文都以定义好的表的编码来存放数据.
> 不管是中文还是英文字母，都是最多存放4个，是按照字符来存放的.

```mysql
#演示字符串类型的使用细节
#char（4）和 varchar（4）这个4表示的是字符，而不是字节，不区分字符是汉字还是字母
CREATE TABLE t11 (`name` CHAR (4) ) ;
INSERT INTO t11 VALUES（'韩顺平好'）;
SELECT * FROM t11;

CREATE TABLE t12 (
name VARCHAR (4) ) ;
INSERT INTO t12 VALUES （'韩顺平好'）；
INSERT INTO t12 VALUES ('ab北京o') ; #error
SELECT * FROM t12;
```

> 2. 细节2
> char（4） 是定长（固定的大小），就是说，即使你 插入‘aa'，也会占用分配的4个字符的空间.
> varchar（4） 是变长 (变化的大小)，就是说，如果你插入了‘aa'，实际占用空间大小井不是4个字符，而是按照实际占用空间来分配（老韩说明：varchar本身还需要占用1-3个字节来记录存放内容长度）
> L(实际数据大小) + (1~3)字节

> 3. 细节3
>     什么时候使用 char，什么时候使用varchar
>     1.如果数据是定长，推荐使用char，比如md5的密码，邮编，手
>     机号，身份证号码等.char（32）
>
>   如果一个字段的长度是不确定，我们使用varchar，比如留
>   言，文章
>   查询速度：char > varchar

> 4. 细节4
>    在存放文本时，也可以使用 Text 数据类型.可以将TEXT列视为
>    VARCHAR列，注意 Text 不能有默认值.大小 0-2^16字节
>    如果希望存放更多字符，可以选择MEDIUMTEXT 0-2^24 或者 LONGTEXT 0~2^32

## 日期类型的基本使用

```mysql
CREATE TABLE birthday6
(t1 DATE, t2 DATETIME,
	t3 TIMESTAMP NOT NULL DEFAULT
	CURRENT_TIMESTAMP ON UPDATE
 CURRENT_TIMESTAMP ); timestamp时间戳
mysql> INSERT INTO birthday(t1,t2)
VALUES('2022-11-11','202211-11 10:10:10');
#日期类型的细节说明
TimeStamp 在 Insert 该条目和 update 该条目时，自动更新
```

```mysql
#演示时间相关的类型
#创建一张表，date，datetime,timestamp
CREATE TABLE t14(
birthday DATE,-- 生日
job_time DATETIME,-- 记录年月日 时分秒
login_time TIMESTAMP
	NOT NULL DEFAULT CURRENT_TIMESTAMP
	ON UPDATE CURRENT_TIMESTAMP);
  #登录时间，如果希望1ogin_time列自动更新，需要增加配置信息NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
  
SELECT * FROM t14;
	INSERT INTO t14 (birthday, job_time)
	VALUES ('2022-11-11' , '2022-11-11 10:10:10');
	# 上面这条语句会自动地增加 login_time字段为当前时间
	-- 如果我们更新 t14表的某条记录，该记录的login_time列会自动的以当前时间进行更新,其他未更新的记录不会更新login_time;
```

# 创建表练习

创建一个员工表emp（课堂练习），选用适当的数据类型

![image-20240227下午113958127](/Users/yannlau/Library/Application Support/typora-user-images/image-20240227下午113958127.png)

```mysql
CREATE TABLE `emp` (
  id INT,
	`name` VARCHAR (32) ,
	sex CHAR (1) ,
	brithday DATE,
	entry_date DATETIME,
	job VARCHAR (32) ,
	salary DOUBLE,
	`resume` TEXT) CHARSET utf8 COLLATE utf8 bin ENGINE INNODB;

-- 添加一条
INSERT INTO `emp` VALUES(100,'小妖怪‘,'男','2000-11-11',
'2010-11-10 11:11:11','巡山的',3000,'大王叫我来巡山');
```

# 修改表

使用 ALTER TABLE 语句追加，修改，或删除列的语法.

```mysql
# 添加列
ALTER TABLE tablename
ADD (column datatype [DEFAULT expr]
	[, column datatypel...);

# 修改列(的数据类型)
ALTER TABLE tablename
MODIFY (column datatype [DEFAULT expr]
	[, column datatype]...);
   
# 删除列
ALTER TABLE tablename
DROP (column);
   
# 查看表的结构: desc 表名;  --可以查看表的列
   
修改表名：Rename table 表名 to 新表名
修改表字符集：alter table 表名 character set 字符集
```

```mysql
# 使用实例
#修改表的操作练习
-- 员工表emp的上增加一个image列，varchar类型（要求在resume后面）。
ALTER TABLE emp
ADD image VARCHAR （32） NOT NULL DEFAULT ''
AFTER RESUME;

DESC emp -- 显示表结构，可以查看表的所有列

-- 修改job列，使其长度为60。
ALTER TABLE emp
MODIFY job VARCHAR (60) NOT NULL DEFAULT '';

-- 删除sex列。
ALTER TABLE emp
		DROP sex;
-- 表名改为employee
RENAME TABLE emp TO employee;
-- 修改表的字符集为utf8
ALTER TABLE employee CHARACTER SET utf8;
-- 列名name修改次user_name
ALTER TABLE employee
	CAHNGE `name` `user_name` VARCHAR(64) NOT NULL DEFAULT '';
```

# 数据库CRUD

create read update delete

> insert语句 添加数据
>
> ```mysql
> # 使用 INSERT 语句向表中插入数据。insert.sql
> INSERT INTO table_name [(column [, column... ])]
> VALUES (value [, value...]);
> 
> 
> # 练习insert 语句
> -- 创建一张商品表goods （id int,goods_name varchar （10），Price double ）；
> -- 添加2条记录
> CREATE TABLE `goods` (
> id INT ,
> goods_name VARCHAR (10) ,
> price DOUBLE) ;
> -- 添加数据
> INSERT INTO `goods`（id, goods_name, price)
> VAIUES（10，'华为手机'，2000);
> INSERT INTO `goods`（id,goods_name, price)
> VAIUES （20，'苹果手机', 3000）;
> 
> SELECT * FROM goods;
> ```
>
> > 使用细节
> >
> > 1. 插入的数据应与字段的数据类型相同。
> >     比如 把‘abc' 添加到 int 类型会错误
> >     但是'123'会被转型为123
> > 2. 数据的长度应在列的规定范围内，例如：不能将一个长度为80的字符串加入到长度为40的列中。
> > 3. 在values中列出的数据位置必须与被加入的列的排列位置相对应。
> > 4. <u>***字符和日期型数据应包含在单引号中。***</u>
> > 5. 列可以插入空值［前提是该字段允许为空］，
> >     insert into table value(null)  <u>***null无需引号***</u>
> > 6. insert into tab_name（列名..) values (),(),() 形式添加多条记录
> > 7. 如果是给表中的所有字段添加数据，可以不写前面的字段名称
> > 8. 默认值的使用，当不给某个字段值时，如果有默认值就会添加，否则报错
> > 9. -- 如果某个列 没有指定 not nu11，那么当添加数据时，没有给定值，则会默认给nu11
> >    -- 如果我们希望指定某个列的默认值，可以在创建表时指定

> update语句
>
> • 使用 update语句修改表中数据
> `UPDATE tb1_ name`
> `SET col_namel=expr1 [ ,  col_name2=expr2...l`
> `[WHERE where_definition]`
>
> ```mysql
> -- 演示update语何
> -- 要求：在上面创建的employee表中修改表中的纪录
> -- 1. 将所有员工薪水修改为5000元。［如果没有带where 条件，会修改所有的记录，因此要小心］
> UPDATE employee SET salary = 5000;
> -- 2. 将姓名为 小妖怪 的员工薪水修改为3000元。
> UPDATE employee SET salary = 3000
> 	WHERE user name ='小妖怪';
> -- 3. 将 老妖怪 的薪水在原有基础上增加1000元
> UPDATE employee SET salary = salary + 1000
> WHERE user_name ='老妖怪';
> ```
>
> > •使用细节：
> > 1. UPDATE语法可以用新值更新原有表行中的各列。
> >
> > 2. SET子句指示要修改哪些列和要给予哪些值。
> > 3. WHERE子句指定应更新哪些行。如没有WHERE子句，则更新所有的行（记录），因此老师提醒一定小心。
> > 4. 如果需要修改多个字段，可以通过set 字段1=值1, 字段2=值2...

> delete语句
>
> - 使用 delete语句删除表中数据。
> - ``delete from tal_name
>    [WHRER where_definition]``
>
> ```mysql
> -- delete 语句演示
> -- 删除表中名称为'老妖'的记录。
> DELETE FROM employee
> 	WHERE user name = '老妖怪'；
> -- 删除表中所有记录，老师提醒，一定要小心
> DELETE FROM employee;
> 
> -- Delete语句不能删除某一列的值（可使用update 设为 null 或者 ''）
> UPDATE employee SET job = '' WHERE user_name ='老妖怪';
> 
> SELECT * FROM employee
> ```
>
> > 使用细节
> >
> > 1. 如果不使用where子句，将删除表中所有数据。
> > 2. Delete语句不能删除某一列的值（可使用update 设为 null 或者"）
> > 3. 使用delete语句仅删除记录，不删除表本身。如要删除表，使用drop table语句。drop table 表名;
> >

> select语句 (重点)
>
> 基本语法
>
> `SELECT [DISTINCT]  * | {column1, column2, column3 ...}`
>
> ​	`FROM tablename;`
>
> • 注意事项（创建测试表学生表）
> 1. Select 指定查询哪些列的数据。
> 2. column指定列名。
> 3. *号代表查询所有列。
> 4. From指定查询哪张表。
> 5. DISTINCT可选，指显示结果时，是否去掉重复数据
>
> ```mysql
> # 测试数据
> ****创建新的表（student）********
> create table student (
> id int not null default 1,
> name varchar (20) not null default '',
> chinese float not null default 0.0,
> english float not null default 0.0,
> math float not null default 0.0
> );
> insert into student (id, name, chinese, english, math) values (1,'韩顺平', 89, 78, 90);
> insert into student (id, name, chinese, english, math) values (2,'张飞', 69, 98, 56);
> insert into student (id, name, chinese, english, math) values (3,'宋江', 87, 78, 77);
> insert into student (id, name, chinese, english, math) values (4,'关羽', 88, 84, 67);
> insert into student (id, name, chinese, english, math) values (5,'赵云', 82, 84, 67);
> insert into student (id, name, chinese, english, math) values (6,'欧阳锋', 55, 85, 45);
> insert into student (id, name, chinese, english, math) values (7,'黄蓉', 75, 65, 30);
> ```
>
> ```mysql
> -- 查询表中所有学生的信息。
> SELECT * FROM student；
> -- 查询表中所有学生的姓名和对应的英语成绩。
> SELECT 'name'， english FROM student；
> -- 过滤表中重复数据 distinct。
> SELECT DISTINCT * FROM student;
> SELECT DISTINCT english FROM stident;
> -- 要查询的记录，每个字段都相同，才会去重
> SELECT DIsTINCT `name`, english FROM student;
> ```
>
> > 使用表达式对查询的列进行运算
> >
> > ```mysql
> > SELECT *|{column1 | expression, column2 | expression, ..} FROM tablename;
> > ```
> >
> > 在select语句中可使用as语句
> >
> > `SELECT column_name as 别名 from 表名;`
>
> ```mysql
> -- 統汁每个学生的怠分
> SELECT name,(chinese+english+math) FROM student;
> -- 在所有学生总分加10分的情况
> SELECT `name`, (chinese + english + math +10) FROM student;
> -- 使用别名表示学生的总分
> SELECT `name` As '名字', (chinese + english + math + 10) As total_score FROM student;
> ```
>
> > 在where子句中经常使用的运算符
> >
> > ![image-20240228上午101749805](/Users/yannlau/Library/Application Support/typora-user-images/image-20240228上午101749805.png)
> >
> > ```mysql
> > -- select 语句
> > -- 查询姓名为赵云的学生成绩
> > SELECT * FROM student
> > 		WHERE `name`='赵云';
> > -- 查询英语成绩大于90分的同学
> > SELECT * FROM student
> > 		WHERE english > 90;
> > -- 查询总分大于200分的所有同学
> > SELECT * FROM student
> > 		WHERE (chinese + english + math) > 200;
> > 		
> > -- 查询math大于60 并且（and）id大于3的学生成绩
> > mysql> select * from student where math>60 and id>3;
> > +----+--------+---------+---------+------+
> > | id | name   | chinese | english | math |
> > +----+--------+---------+---------+------+
> > |  4 | 关羽   |      88 |      84 |   67 |
> > |  5 | 赵云   |      82 |      84 |   67 |
> > +----+--------+---------+---------+------+
> > 2 rows in set (0.00 sec)
> > 
> > -- 查询英语成绩大于语文成绩的同学
> > mysql> select * from student where english > chinese;
> > +----+-----------+---------+---------+------+
> > | id | name      | chinese | english | math |
> > +----+-----------+---------+---------+------+
> > |  2 | 张飞      |      69 |      98 |   56 |
> > |  5 | 赵云      |      82 |      84 |   67 |
> > |  6 | 欧阳锋    |      55 |      85 |   45 |
> > +----+-----------+---------+---------+------+
> > 3 rows in set (0.00 sec)
> > 
> > -- 查询总分大于200分 并且 数学成绩小于语文成绩，的姓韩的学生，
> > -- 韩% 表示名字以韩开头即可,后面不管是什么
> > mysql> select * from student where math<chinese and chinese+english+math>200 AND `name` LIKE '赵%';
> > +----+--------+---------+---------+------+
> > | id | name   | chinese | english | math |
> > +----+--------+---------+---------+------+
> > |  5 | 赵云   |      82 |      84 |   67 |
> > +----+--------+---------+---------+------+
> > 1 row in set (0.00 sec)
> > 
> > -- 查询英语分数在 80-90之间的同学。
> > SELECT * FROM student
> > 		WHERE english >= 80 AND english <= 90;
> > SELECT * FROM student;
> > 		WHERE english BETWEEN 80 AND 90;-- between .. and .. 是 闭区间
> > 		
> > -- 查询数学分数为89,90，91的同学。
> > SELECT * FROM student
> > 		WHERE math = 89 OR math = 90 OR math = 91;
> > SELECT * FROM student
> > 		WHERE math IN (89, 90, 91);
> > 		
> > -- 查询所有姓韩的学生成绩。
> > SELECT * FROM student
> > 		WHERE name LIKE '韩%';
> > ```
>
> > 使用order by 子句排序查询结果
> >
> > ```mysql
> > SELECT columnl, column2, column3..
> > 		FROM table
> > 		order by column asc(默认)|desc, ...
> > ```
> >
> > 1. Order by 指定排序的列，排序的列既可以是表中的列名，也可以是select语句后指定的列名。
> > 2.  Asc 升序［默认］、Desc 降序
> > 3. ORDER BY 子句应位于SELECT语句的结尾。
> >
> > ```mysql
> > -- 演示order by使用
> > -- 对数学成绩排序后输出【升序】。
> > SELECT * FROM student
> > 		ORDER BY math;
> > -- 对总分按从高到低的顺序输出〔降序］ 使用别名排序
> > SELECT `name`, (chinese + english + math) As total_score FROM student
> > 		ORDER BY total_score DESC;
> > 
> > -- 对姓韩的学生成绩［总分］排序输出（升序）where + order by
> > SELECT `name`,(chinese + english + math) AS total_score FROM student
> > 		WHERE `name` LIKE '韩%'
> > 		ORDER BY total_score;
> > 		
> > SELECT * FROM student
> > 		WHERE `name` LIKE '韩%'
> > 		ORDER BY (chinese + english + math);
> > ```

# SELECT加强

> 合计/统计函数- count
>
> - Count 返回行的总数
>   `Select count （*） | count（列名）from table_name`
>   		`[WHERE where_definition]`
>
> ```mysql
> -- 演示mysq1的统计函数的使用
> -- 统计一个班级共有多少学生？
> SELECT COUNT （*） FROM student；
> -- 统计数学成绩大于90的学生有多少个？
> SELECT COUNT (*) FROM student
> 		WHERE math > 90
> -- 统计总分大于250的人数有多少？
> SELECT COUNT (*) FROM student
> 		WHERE（math + english + chinese）> 250
> -- count（*）和 count（列）的区别
> -- 解释：count（*）返回满足条件的记录的行数
> -- coudt（列）：统计满足条件的某列有多少个，但是会排除 为nu11
> 
> CREATE TABLE t15 (
> 	`name` VARCHAR (20) ) ;
> 	INSERT INTO t15 VALUES ('tom') ;
> 	INSERT INTO t15 VALUES ('jack') ;
> 	INSERT INTO t15 VALUES ('mary') ;
> 	INSERT INTO t15 VALUES (NULL) ;
> 	SELECT * FROM t15;
> 	SELECT COUNT (*) FROM t15; -- 4
> 	SELECT COUNT (`name`) FROM t15; -- 3
> ```

> 合计函数-sum
>
> - Sum函数返回满足where条件的行的和---一般使用在数值列
>
>   `Select sum (列名) {  , sum（列名）... }  from tablename`
>   		`[WHERE where_definition]`
>
> ```mysql
> -- 演示sum函数的使用
> -- 统计一个班级数学总成绩？
> SELECT SUM(math) FROM student;
> -- 统计一个班级语文、英语、数学各科的总成绩
> SELECT SUM （math） As math_total_score, SUM（english）， SUM（chinese） FROM student；
> -- 统计一个班级语文、英语、数学的成绩总和
> SELECT SUM(math + english + chinese) FROM student;
> -- 统计一个班级语文成绩平均分
> SELECT SUM (chinese) / COUNT (*) FROM student;
> #注意：sum仅对数值起作用，没有意义。
> #注意：对多列求和，“，”号不能少。
> ```

> 合计函数-avg
> AVG函数返回满足where条件的一列的平均值
> `Select avg(列名) { , avg（例名）... }  from tablename`
> 	`[WHERE where_definition]`
>
> ```mysql
> -- 漢示avg的使用
> -- 练习：
> -- 求一个班级数学平均分？
> SELECT AVG (math) FROM student;
> -- 求一个班级总分平均分
> SELECT AVG (math + english + chinese) FROM student;
> ```

> 合计函数-Max/min
> Max/min函数返回满足where条件的一列的最大/最小值
> `Select max（列名）from tablename`
> 	`[WHERE where_definition]`
> 练习：求班级最高分和最低分（数值范围在统计中特别有
> 用）
>
> ```mysql
> -- 演示max 和 min的使用
> -- 求班级最高分和最低分（数值范围在统计中特别有用）
> SELECT MAX (math + english + chinese), MIN(math + english + chinese)
> 		FROM student;
> 		
> -- 求出班级数学最高分和最低分
> SELECT MAX (math) AS math_high_socre, MIN (math) As math_low__socIe
> 		FROM student;
> ```

> select语句
>
> - 使用group by子句对列进行分组［先创建测试表］
>
>   `SELECT column1, column2, column3 ... FROM table`
>   `		group by column`
>
> - 使用having 子句对分组后的结果进行过滤
>   `SELECT column1, column2, column3... FROM table`
>   `group by column having ...`
>
> ```mysql
> CREATE TABLE dept( /*部门表*/
> deptno MEDIUMINT UNSIGNED NOT NULL DEFAULT 0,
> dname VARCHAR(20) NOT NULL DEFAULT "",
> loc VARCHAR(13) NOT NULL DEFAULT ""
> );
> 
> INSERT INTO dept VALUES(10,'ACCOUNTING', 'NEW YORK'),(20,'RESEARCH','DALLAS'),(30,'SALES','CHICAGO'),(40,'OPERATIONS','BOSTON');
> 
> #创建表EMP雇员
> CREATE TABLE emp(
> 	empno MEDIUMINT UNSIGNED NOT NULL DEFAULT 0, /*编号*/
> 	ename VARCHAR(20) NOT NULL DEFAULT "", /*名字*/
> 	job VARCHAR(9) NOT NULL DEFAULT "",/*工作*/
> 	mgr MEDIUMINT UNSIGNED, /*上级编号*/
> 	hiredate DATE NOT NULL, /*入职时间*/
> 	sal DECIMAL(7, 2) NOT NULL,/*薪水*/
> 	comm DECIMAL(7,2),/*红利*/
> 	deptno MEDIUMINT UNSIGNED NOT NULL DEFAULT 0/*部门编号*/);
> 
> INSERT INTO emp VALUES
> (7369,'SMITH','CLERK',7902,'1990-12-17', 800.00, null, 20),
> (7499,'ALLEN','SALESMAN',7698,'1991-2-20',1600.00,300.00,30),
> (7521,'WARD','SALESMAN', 7698,'1991-2-22',1250.00,500.00,30),
> (7566,'JONES','MANAGER',7839,'1991-4-2',2975.00,NULL,20),
> (7654,'MARTIN','SALESMAN',7698,'1991-9-28',1250.00,1400.00,30),
> (7698,'BLAKE','MANAGER',7839,'1991-5-1',2850.00,NULL,30),
> (7782,'CLARK','MANAGER',7839,'1991-6-9',2450.00,NULL,10),
> (7788,'SCOTT','ANALYST',7566,'1997-4-19',3000.00,NULL,20),               
> (7839,'KING','PRESIDENT',NULL,'1991-11-17',5000.00,NULL,10),     
> (7844,'TURNER','SALESMAN',7698,'1991-9-8',1500.00,NULL,30),
> (7900,'JAMES','CLERK',7698,'1991-12-3',950.00,NULL,30),
> (7902,'FORD','ANALYST',7566,'1991-12-3',3000.00,NULL,20),
> (7934,'MILLER','CLERK',7782,'1992-1-23',1300.00,NULL,10);
> 
> #工资级别表
> CREATE TABLE salgrade(
> grade MEDIUMINT UNSIGNED NOT NULL DEFAULT 0, #工资级别
> losal DECIMAL(17,2) NOT NULL, # 该级别的最低工资
> hisal DECIMAL(17,2) NOT NULL # 该级别的最高工资
> );
> INSERT INTO salgrade VALUES (1,700,1200);
> INSERT INTO salgrade VALUES (2,1201,1400);
> INSERT INTO salgrade VALUES (3,1401,2000);
> INSERT INTO salgrade VALUES (4,2001,3000);
> INSERT INTO salgrade VALUES (5,3001,9999);
> 
> # 演示group by + having
> # GROUP by用于对查询的结果分组统计，（示意图）
> -- having子句用于限制分组显示结果.
> 
> -- ？如何显示每个部门的平均工资和最高工资
> -- 老韩分析：avg(sal) max(sal)
> -- 按照部分来分组查询
> SELECT AVG(sal), MAX(sal), deptno
> 		FROM emp GROUP BY deptno;
> 
> -- ？显示每个部门的每种岗位的平均工资和最低工资
> SElECT AVG(sal),MIN(sal),`job`,`deptno` FROM emp GROUP BY deptno,job;
> 
> -- ？显示平均工资低于2000的部门号和它的平均工资 // 别名
> -- 老师分析［写sq2语句的思路是化繁为简，各个击破］
> -- 1. 显示各个部门的平均工资和部门号
> -- 2. 在1的结果基础上，进行过滤，保留 AVG(sal) < 2000
> -- 3．使用别名进行过滤
> 
> SELECT AVG (sal), deptno
> 	FROM emp GROUP BY deptno
> 		HAVING AVG(sal) < 2000;
> 
> -- 使用别名
> SELECT AVG(sal) AS avg_sal, deptno
> 	FROM emp GROUP BY deptno
> 		HAVING avg_sal < 2000;
> 		
> # 使用中文别名的话,前面的定义处用单引号,后面的having语句不用单引号!
> ```
>
> ![image-20240228下午34733085](/Users/yannlau/Library/Application Support/typora-user-images/image-20240228下午34733085.png)

# 字符串相关函数

![image-20240228下午35513626](/Users/yannlau/Library/Application Support/typora-user-images/image-20240228下午35513626.png)

```mysql
-- 演示字符串相关函数的使用,使用emp表来演示

-- CHARSET(str) 返回表中每一条中的指定属性的字串字符集
SELECT CHARSET(ename) FROM emp;
-- CONCAT(string2 [,... ]) 连接字串,将多个列拼接成一列返回
select concat(ename,' job is ',job) from emp;
+------------------------------+
| concat(ename,' job is ',job) |
+------------------------------+
| SMITH job is CLERK           |
| ALLEN job is SALESMAN        |
| WARD job is SALESMAN         |
| JONES job is MANAGER         |
| MARTIN job is SALESMAN       |
| BLAKE job is MANAGER         |
| CLARK job is MANAGER         |
| SCOTT job is ANALYST         |
| KING job is PRESIDENT        |
| TURNER job is SALESMAN       |
| JAMES job is CLERK           |
| FORD job is ANALYST          |
| MILLER job is CLERK          |
+------------------------------+
13 rows in set (0.00 sec)

-- INSTR(string,substring) 返回substring在string中出现的位置，没有返回0  下表从1开始计数
-- dual 亚元表,系统表 可以作为测试表使用
SELECT INSTR('hanshunping','ping') FROM DUAL; -- 返回8

-- UCASE(string2) 转换成大写显示而已,并不会更改原来的表
SELECT UCASE(ename) FROM emp;

-- LCASE(string2) 转换成小写显示而已,并不会更改原来的表
SELECT LCASE(ename) FROM emp;

-- LEFT(string2,length) 从string2中的左边起取1ength个字符
-- RIGHT(string2,length) 从string2中的右边起取1ength个字符
SELECT LEFT(ename,2) FROM emp;

-- LENGTH(string) string长度［按照字节］
SELECT LENGTH(ename) FROM emp;
desc test01;
+-------+------------+------+-----+---------+-------+
| Field | Type       | Null | Key | Default | Extra |
+-------+------------+------+-----+---------+-------+
| cc    | char(4)    | YES  |     | NULL    |       |
| vv    | varchar(4) | YES  |     | NULL    |       |
+-------+------------+------+-----+---------+-------+
mysql> select * from test01;
+--------------+--------------+
| cc           | vv           |
+--------------+--------------+
| 1234         | 1234         |
| 滚你妈的     | 滚你妈的     |
| 滚nmd        | 滚nmd        |
+--------------+--------------+
3 rows in set (0.00 sec)
mysql> SELECT length(cc),length(vv) from test01;
+------------+------------+
| length(cc) | length(vv) |
+------------+------------+
|          4 |          4 |
|         12 |         12 |
|          6 |          6 |
+------------+------------+
3 rows in set (0.00 sec)

-- REPLACE(str,search_str,replace_str)
-- 在str中用replace_str替换search_str
-- 如果是manager 就替换成 经理
SELECT ename,REPLACE(job,'MANAGER','经理') FROM emp;

-- STRCMP(string1,string2) 逐字符比较两字串大小
SELECT STRCMP('hsp','jsp') FROM DUAL;
# 可以看出 DUAL 亚元表用的是不区分大小写的字符集

-- SUBSTRING(str,position [,length]) 
-- 从str的position开始【从1开始计算】,取length个字符
-- 如果length不写,默认取到末尾
-- 从ename列的第一个位置开始取出2个字符
SELECT SUBSTRING(ename,1,2) FROM emp;
-- LTRIM(string2) RTRIM(string2) trim 去除前端空格或后端空格
SELECT LTRIM(' 韩顺平教育') FROM DUAL;
SELECT RTRIM('韩顺平教育  ') FROM DUAL; 
SELECT TRIM('   韩顺平教育  ') FROM DUAL; 

-- 练习：以首字母小写的方式显示所有员工emp表的姓名
-- 方法1
-- 思路先取出ename 的第一个字符，转成小写的
-- 把他和后面的字符串进行拼接输出即可


select concat(lcase(left(ename,1)),substring(ename,2)) from emp;
SELECT CONCAT(LCASE (SUBSTRING (ename, 1, 1)) , SUBSTRING (ename, 2)) As new _name FROM emp;
```

# 数学函数

> ![image-20240228下午60607498](/Users/yannlau/Library/Application Support/typora-user-images/image-20240228下午60607498.png)

```mysql
-- 演示数学相关函数

-- ABS(num) 绝对值
select abs(-10) from dual;

-- BIN(decimal_numnber) 十进制转二进制
select bin(10) from dual;

-- CEILING(number2) 向上取整，得到比 num2 大的最小整数
select ceiling(1.1) from dual;

-- CONV (number2, from base, to base) 进制转换
-- 下面的含义是 8 是十进制的8 转成 2进制 输出
select conv(8,10,2) from dual;
-- 下面的含义是 8是16进制的8，转成 2进制输出
SELECT CONV(16,16,10)  FROM DUAL；

-- FLOOR (number2) 向下取整，得到比 num2 小的最大整数
SELECT floor(1.1) FROM DUAL;

-- FORMAT(number,decimal_places)  保留小数位数-四舍五入
SELECT format(8.123458,2) FROM DUAL;

-- HEX(DecimalNumber) 转十六进制

-- LEAST(number,number2 [, ...]) 求最小值
select least(0,1,-10,4) from dual;

-- MOD(numerator,denominator)  求余
SELECT MOD (10, 3) FROM DUAL;

-- RAND([seed]) RAND([seed]) 返回随机数 其范围为 0≤v≤1.0
-- 老韩说明
-- 1. 如果使用 rand（）每次返回不同的随机数，在O≤v≤ 1.0
-- 2. 如果使用 rand（seed）返回随机数，范围O≤v ≤1.0，如果seed不变，该随机数也不变了
官方文档: rand()的取值范围为[0,1)
select rand() from dual;
```

# 时间日期相关函数

![image-20240228下午70003131](/Users/yannlau/Library/Application Support/typora-user-images/image-20240228下午70003131.png)

CSDN: 在上面这个例子因为人为加入了sleep函数，让其等待5秒和10秒，可以发现sysdate返回的函数跟其他是不一样的，究其原因是这3个函数的略微区别：

1) current_timestamp是now的同义词，也就是两者是相同的

2. sysdate函数返回执行当前函数的时间，而now返回执行SQL语句时的时间。所以两次执行sysdate函数返回不同的时间是因为第二次调用执行该函数时等待了前面SLEEP函数5秒，而对于now函数，不管在sleep之前还是之后执行，返回都是执行这条sql语句的时间。

```mysql
-- 日期时间相关函数

-- CURRENT DATE() 当前日期
SELECT CURRENT DATE () FROM DUAL;
-- CURRENT TIME（ ）当前时间
SELECT CURRENT_TIME（） FROM DUAL；
-- CURRENT_TIMESTAMP（）当前时间戳
SELECT CURRENT TIMESTAMP () FROM DUAL;

-- 创建测试表 信息表
CREATE TABLE mes(
id INT ,
content VARCHAR (30),
send_time DATETIME);

-- 添加3条记录
INSERT INTO mes VALUES （1，'北京新闻'，CURRENT_TIMESTAMP （））；
INSERT INTO mes VALUES (2,'上海新闻',NOW ());
INSERT INTO mes VALUES (3,'广州新闻'，NOW（））;

-- 显示所有新闻信息，发布日期只显示 日期，不用显示时间
select id,content,date(send_time) from mes;               
                        
-- 请查询在10分钟内发布的帖子
select * from mes where date_add(send_time,interval 10 minute) >= now();

-- 请在mysql 的sql语句中求出 2011-11-11 和 1990-1-1 相差多少天
select datadiff('2011-11-11','1990-01-01') from dual;            
-- 请用mysq1 的sq2语句求出你活了多少天？［练习］
select abs(datediff('2000-06-11',now()));
                        
-- 如果你能活80岁，求出你还能活多少天.［练习］
select datediff(date_add('2000-06-11',interval 80 year),now());        
                        
上面函数的细节说明：
1.DATE_ADD() 中的 interval 后面可以是 year minute second day 等
2.DATE_SUB（） 中的 interval 后面可以是 year minute second day 等
3.DATEDIFF（date1,date2） 得到的是天数，而且是date1-date2 的天数，因此可以取负数
4.这四个函数的日期类型可以是 date, datetime 或者 timestamp
                        
-- YEAR|Month|DAY| DATE (datetime )
SELECT YEAR (NOW () ) FROM DUAL;
SELECT MONTH (NOW ()) FROM DUAL;
SELECT DAY (NOW ()) FROM DUAL;
SELECT MONTH ('2013-11-10') FROM DUAL;
select time(now()) from dual; //只返回时间部分
                        
在实际开发中，我们也经常使用int来保存一个unix时间戳，然后使用from_unixtime（ 进行转换，还是非常有实用价值的
-- unix_timestamp（）：返回的是1970-1-1 现在的秒数
SELECT UNIX TIMESTAMP () FROM DUAL;
-- FROM_UNIXTIME （）：可以把一个unix_timestamp 秒数［时间戳］，转成指定格式的日期
-- %Y-%m-%d 格式是规定好的，表示年月日
-- 意义：在开发中，可以存放一个整数，然后表示时间，通过EROM_UNIXTIME转换
SELECT FROM_UNIXTIME(1618483484,'%Y-%n-%d') FROM DUAL;
SELECT FROM_UNIXTIME(1618483100,'%Y-%m-%d %H:8%i%s') FROM DUAL;
```

# 加密函数

![image-20240228下午100404862](/Users/yannlau/Library/Application Support/typora-user-images/image-20240228下午100404862.png)

```mysql
-- 演示加密函数和系统函数
-- USER ()查询用户
-- 可以查看登录到mysq1的有哪些用户，以及登录的IP
SELECT USER() FROM DUAL；-- 用户@IP地址

-- DATABASE（） 数据库名称
select database();

-- MD5(str) 为字符串算出一个 MD5 32的字符串，常用（用户密码）加密
-- root 密码是 hsp ->加密md5 ->在数据库中存放的是加密后的密码
SELECT MD5('hsp') FROM DUAL;
SELECT LENGTH (MD5 ('hsp')) FROM DUAL;
-- 演示用户表，存放密码时，是md5
CREATE TABLE users
		(id INT
		name VARCHAR(32) NOT NULL default '',
		pwd CHAR (32) NOT NULL DEFAULT '');
INSERT INTO hsp_user VALUES(100,'韩顺平!',MD5('hsp'));
SELECT * FROM hsp_user WHERE 'name ='韩顺平，AND pwd = MD5 （'hsp'）;

-- PASSWORD (str)  8.0版本去除了这个函数,用sha2代替
-- PASSWORD（str）-- 加密函数，MySQL数据库的用户密码就是 PASSWORD函数加密
SELECT PASSWORD （'hsp'） FROM DUAL；

-- select * from mysql.user \G 从原文密码str 计算并返回密码字符串，通常用于对mysq1数据库用户密码加密
-- mysql.user 表示 数据库.表
SELECT * FROM mysql.user
该表中 的authentication string 属性便是 用户密码 的 加密过后的密文
```

# 流程控制函数

![image-20240229上午93748162](/Users/yannlau/Library/Application Support/typora-user-images/image-20240229上午93748162.png)

```mysql
＃演示流程控制语句
# IF （expr1, expr2,expr3）如果expr1为True，则返回 expr2 否则返回
expr3
SELECT IF （FALSE，'北京'，'上海'）FROM DUAL；

# IFNULL (expr1, expr2) 如果expr1不为空NUIL，则返回expr1，否则返回expr2
SELECT IFNULL （ NULL，'韩顺平教育'）EROM DUAL ；
# SELECT CASE WHEN eXPI1 THEN expr2 WHEN exPr3 THEN expr4 ELSE expr5 END；［类似多重分支.］
# 如果expr1 为TRUE，则返回expr2，如果expr2 为true，返回 expr4，否则返回 expr5

SELECT CASE
WHEN TRUE THEN 'jack'  -- jack
WHEN FALSE THEN 'tom'
ELSE 'mary' END

-- 1. 查询emp 表，如果 comm 是nul1，则显示0.0
# 老师说明，判断是否为nul1 要使用 is nul1，判断不为空 使用 is not
SELECT ename, IF (comm IS NULL, 0.0, comm)
FROM emp;
SELECT ename, IFNULI (comm, 0.0)
FROM emp;
-- 2. 如果emp 表的 job 是 CLERK 则显示 职员，如果是：MANAGER 则显示经理  如果是 SALESMAN 则显示 销售人员，其它正常显示
ISELECT ename, (SELECT CASE
WHEN job = 'CLERK' THEN，'职员',
WHEN job ='MANAGER' THEN '经理',
WHEN job ='SALESMAN' THEN '销售人员',
ELSE job END) As 'job', job
FROM emp;
```

# MySQL 查询加强

> 在前面我们讲过mysql表的基本查询，但是都是对一张表进行的
> 查询，这在实际的软件开发中，还远远的不够。
> 下面我们讲解的过程中，将使用前面创建 三张表
> （emp.dept,salgrade）为大家演示如何进行多表查询

```mysql
-- 查询加强

-- 使用where子句
-- ？如何查找1992.1.1后入职的员工
-- 老师说明：在mysql中，日期类型可以直接比较，需要注意格式
SELECT * FROM emp
WHERE hiredate >'1992-01-01';

-- 如何使用1ike操作符(模糊查询)
-- %:表示0到多个任意字符  _:表示单个字符任意字符
-- ？如何显示首字符为s的员工姓名和工资
select ename,sal from emp
		where ename like 'S%';
-- ？如何显示第三个字符为大写O的所有员工的姓名和工资
select ename,sal from emp
		where enmae like '__O%';

-- • 如何显示没有上级的雇员的情况
select ename,sal from emp where mgr is null;

-- 查询表结构
desc emp;

-- 使用order by子句
-- ？如何按照工资的从低到高的顺序［升序］，显示雇员的信息
SELECT * FROM emp
		ORDER BY sal asc
		
-- ？按照部门号升序而雇员的工资降序排列，显示雇员信息
SELECT * FROM emp
		ORDER BY sal asc, sal desc;

```

> 分页查询
>
> 1. 按雇员的id号升序取出，每页显示3条记录，请分别显示 第一页，第二页，第三页
> 2. 基本语法
>    select ... limit start, rows
>    表示从start+1行开始取，取出rows行，start 从0开始计算
>
> ```mysql
> --第1页
> SELECT * FROM emp
> ORDER BY empno
> LIMIT 0, 3;
> -- 第2页
> SELECT * FROM emp
> ORDER BY empno
> LIMIT 3, 3;
> --第3页
> SELECT * FROM emp
> ORDER BY empno
> LIMIT 6, 3;
> -- 推导一个公式
> SELECT * FROM emp
> ORDER BY empno
> LIMIT 每页显示记录数*（第几页-1），每页显示记录数
> ```

> 分组增强
>
> 使用分组函数和分组子句group by
>
> ```mysql
> -- （1）显示每种岗位的雇员总数、平均工资。
> select count(*) ,job, avg(sal)
> 	from emp
> 	group by job;
> 	-- 这里注意，group by 的条件一定要在select中.对的，如果group by 后面的数据不在要显示的列表中，他就会报错
> 
> （2） 显示雇员总数，以及获得补助的雇员数。
> -- 思路：获得补助的雇员数 就是 comm 列为 nu11，就是count（列），如果该列的值为nu11，是不会统计
> SELECT COUNT (*), COUNT (comm)
> 		FROM emp
> -- 老师的扩展要求：统计没有获得补助的雇员数
> SELECT COUNT (*), COUNT (IF (comm IS NULL, 1, NULL))
> 		FROM emp
> SELECT COUNT (*), COUNT (*) - COUNT (comm)
> 		FROM emp
> 
> （3） 显示管理者的总人数。
> SELECT COUNT (DISTINCT mgr)
> FROM emp;
> 
> （4） 显示雇员工资的最大差额。
> SELECT MAX (sal) - MIN(sal)
> FROM emp;
> ```

> 数据分组总结
>
> 如果select语句同时包含有group by ,having,limit
> order by 那么他们的顺序是group by , having, order by, limit
>
> 应用案例：请统计各个部门的平均工资，并且是大于1000的，
> 并且按照平均工资从高到低排序，取出前两行记录.
>
> ```mysql
> SELECT columnl, column2. column3 .. FROM table
>     group by column
>     having condition
>     order by column
>     limit start,rows;
>     
> -- 应用案例：请统计各个部门group by 的平均工资 avg，
> -- 并且是大于1000的 having，并且按照平均工资从高到低排序，order by
> -- 取出前两行记录 limit
> select deptno, avg(sal) as 'avg_sal' from emp
> 	group by deptno
> 	having avg_sal > 1000
> 	order by avg_sal desc
> 	limit 0,2;
> ```

# 多表查询

> •说明
> 多表查询是指基于两个和两个以上的表查询.在实际应用中.查询单个表可能不能满足你的需求，（如下面的课堂练习），需要使用到（dept表和emp表）
>
> ```mysql
> -- 多表查询练习 many_tab.sql
> 
> /*
> 	在默认情况下：当两个表查询时，规则
> 1.从第一张表中，取出一行 和第二张表的每一行
> 进行组合，返回结果［含有两张表的所有列］
> 2.一共返回的记录数 第一张表行数*第二张表的行数
> 3.这样多表查询默认处理返回的结果，称为笛卡尔集
> 4.解决这个多表的关键就是要写出正确的过滤条件 where，需要程序员进行分析
> */
> 
> -- ？显示雇员名，雇员工资及所在部门的名字【笛卡尔集）
> 1. 雇员名，雇员工资 来白 emp表
> 2.部门的名字 来自 dept表
> 3. 需求对 emp 和 dept查询
> ename, sal, dname, deptno
> 4. 当我们需要指定显示某个表的列是，需要 表.列表
> SELECT
> ename, sal, dname, emp.deptno
> FROM emp, dept
> WHERE emp.deptno = dept.deptno
> SELECT * FROM emp;
> SELECT * FROM dept;
> 
> -- 老韩小技巧：多表查询的条件不能少于表的个数-1，否则会出现笛卡尔集
> 
> -- ？如何显示部门号为10的部门名、员工名和工资
> SELECT ename, sal, dname, emp. deptno
> FROM emp, dept
> WHERE emp. deptno = dept.deptno AND emp.deptno = 10
> 
> -- ？显示各个员工的姓名，工资，及其工资的级别
> SELECT ename, sal, grade
> FROM emp , salgrade
> WHERE sal BETWEEN losal AND hisal;
> ```

> 自连接
>
> 自连接是指在同一张表的连接查询［将同一张表看做两张表］。
>
> ```mysql
> -- 多表查询的 自连接
> -- 思考题：显示公司员工名字和他的上级的名字
> -- 老韩分析：员工名字 在emp，上级的名字的名字 emp
> -- 员工和上级是通过 emp表的 mgr 列关联
> 
> select A.ename as down,B.ename as up 
> from emp as A , emp as B  # 这里的as可以省略
> where A.mgr=B.empno;
> 
> 这里老师小结： 自连接的特点 
> 1. 把同一张表当做两张表使用
> 2. 需要给表取别名 表名 表别名
> 3. 列名不明确，可以指定列的别名 列名 as 列的别名
> ```

# 子查询 (嵌套查询)

• 什么是子查询 subquery.sql
子查询是指嵌入在其它sql语句中的select语句，也叫嵌套查询
• 单行子查询
单行子查询是指只返回一行数据的子查询语句
• 多行子查询
多行子查询指返回多行数据的子查询 使用关键字 in

```mysql
-- 请思考：如何显示与SMITH同一部门的所有员工？

SELECT *
FROM emp
WHERE deptno = (
		SELECT deptno
		FROM emp
		WHERE ename = 'SMITH'
  );

-- 课堂练习：如何查询和部门10的工作相同的雇员的
-- 名字、岗位、工资、部门号，但是不含10号部门自己的雇员.
mysql> select ename,job,sal,deptno from emp
    -> where deptno!=10 and job in (
    -> select DISTINCT job from emp where deptno=10
    -> );
+-------+---------+---------+--------+
| ename | job     | sal     | deptno |
+-------+---------+---------+--------+
| SMITH | CLERK   |  800.00 |     20 |
| JONES | MANAGER | 2975.00 |     20 |
| BLAKE | MANAGER | 2850.00 |     30 |
| JAMES | CLERK   |  950.00 |     30 |
+-------+---------+---------+--------+
4 rows in set (0.01 sec)
```

```mysql
-- 子查询用于from处,形成笛卡尔积的形式解决问题

-- 查询eashop中各个类别中，价格最高的商品
-- 查询 商品表
-- 先得到 各个类别中，价格最高的商品 max + group by cat_id，当做临时表
-- 把子查询当做一张临时表可以解决很多很多复杂的查询
select good_id,ecs_goods.cat_id,goods_name,shop_price
	from (
    		select cat_id,max(shop_price) as max_price
    		from ecs+goods
    		group by cat_id
  ) as temp, ecs_goods
  where temp.cat_id = ecs_goods.cat_id
  and temp.max_price = ecs_goods.shop+price;
```

> all 和 any关键字的使用
>
> ```mysql
> -- all 和 any的使用
> 
> -- 请思考：显示工资比部门30的所有员工的工资高的员工的姓名、工资和部门号
> SELECT ename, sal, deptno
> FROM emp
> WHERE sal > ALL(
> 	SELECT sal
> 		FROM emp
> 		WHERE deptno = 30
>   )
> -- 也可以这样写
> SELECT ename, sal, deptno
> FROM emp
> WHERE sal > (
> 	SELECT MAX(sal)
> 		FROM emp
> 		WHERE deptno = 30
>   );
> 
> -- 请思考：如何显示工资比部门30的其中一个员工的工资高的员工的姓名、工资和部门号
> SELECT ename, sal, deptno
> FROM emp
> WHERE sal > ANY(
> 	SELECT sal
> 		FROM emp
> 		WHERE deptno = 30
>   )
> SELECT ename, sal, deptno
> FROM emp
> WHERE sal > (
> 	SELECT min(sal)
> 		FROM emp
> 		WHERE deptno = 30
>   )
> ```

# 多列子查询

请思考如何查询与smith的部门和岗位完全相同的所有雇员（并且不含smith本人）（字段1，字段2...）=（select 字段1，字段2 from。。。。

```mysql
-- 多列子查询

select * from emp
	where 
	(deptno,job)=(select deptno,job from emp where ename='allen') 
	and ename!='allen';
	
-- 子查询练习
-- 请思考：查找每个部门工资高于本部门平均工资的人的资料
-- 这里要用到数据查询的小技巧，把一个子查询当作一个临时表使用

select ename,sal,A.avg_sal,A.deptno from emp,(select deptno,avg(sal) as avg_sal from emp group by deptno) as A
	where emp.sal > A.avg_sal and emp.deptno=A.deptno;
	
+-------+---------+-------------+--------+
| ename | sal     | avg_sal     | deptno |
+-------+---------+-------------+--------+
| ALLEN | 1600.00 | 1566.666667 |     30 |
| JONES | 2975.00 | 2443.750000 |     20 |
| BLAKE | 2850.00 | 1566.666667 |     30 |
| SCOTT | 3000.00 | 2443.750000 |     20 |
| KING  | 5000.00 | 2916.666667 |     10 |
| FORD  | 3000.00 | 2443.750000 |     20 |
+-------+---------+-------------+--------+
6 rows in set (0.00 sec)

-- 查找每个部门工资最高的人的详细资料
select ename,sal,max_sal,temp.deptno from emp,(
	select deptno,max(sal) as max_sal from emp group by deptno
) as temp
where emp.deptno = temp.deptno and emp.sal = temp.max_sal;

-- 在from子句中使用子查询—课堂小练习
先练，再讲：
查询每个部门的信息（包括：部门名，编号，地址）和人员数量，我们一起完
思路：
1. 先将人员信息和部门信息关联显示
2. 然后统计
select dname,dept.deptno,loc,count(empno) as emp_num 
	from dept,(select empno,deptno from emp) as temp
	where dept.deptno = temp.deptno
	group by dept.deptno,dname,loc;
+------------+--------+----------+---------+
| dname      | deptno | loc      | emp_num |
+------------+--------+----------+---------+
| RESEARCH   |     20 | DALLAS   |       4 |
| SALES      |     30 | CHICAGO  |       6 |
| ACCOUNTING |     10 | NEW YORK |       3 |
+------------+--------+----------+---------+
3 rows in set (0.01 sec)

-- 老师优化
SELECT dname, dept.deptno, loc, tmp.per_num as '人数'
	FROM dept,(
		SELECT COUNT (*) As per_num, deptno
		FROM emp
		GROUP BY deptno
) tmp
WHERE tmp.deptno = dept.deptno;
	
-- 还有一种写法  表.*  表不将该表所有列都显示出来,可以简化sql语句
-- 在多表查询中,当多个表不重复时,才可以直接写列名
SELECT tmp.*, dname, loc
FROM dept, (SELECT COUNT（*） AS per_num, deptno FROM emp
GROUP BY deptno
）tmp
WHERE tmp.deptno = dept.deptno;
```

# 表复制

• 自我复制数据（蠕虫复制）
有时，为了对某个sq|语句进行效率测试，我们需要海量数据时，可以使用此法为表创建海量数据。

```mysql
-- 表的复制
-- 为了对某个sql语句进行效率测试，我们需要海量数据时，可以使用此法为表创建海量数据
CREATE TABLE my_tab01(
  id INT,
 `name` VARCHAR(32),
	sal DOUBLE,
	job VARCHAR(32),
	deptno INT);
-- 演示如何自我复制
-- 1.先把 emp 表的记录复制到 my_tab01
INSERT INTO my_tab01(id,`name`,sal,job,deptno)
	select empno,ename,sal,job,deptno from emp;
-- 2. 自我复制
INSERT INTO my_tab01
	SELECT * FROM my_tab01;
SELECT COUNT(*) FROM my_tab01;

-- 如何删除掉一张表重复记录
-- 1．先创建一张表 叫y_tab02，
-- 2. 让 my_tab02 有重复的记录
CREATE TABIE my_tab02 LIKE emp;
-- 这个语句 把emp表的结构（列），复制到my_tab02
desc my_tab02;

insert into my_tab02
	select * from emp;
	
select * from my_tab02;

-- 3. 考虑去重 my_tab02 的记录
/*
	思路
（1）先创建一张临时表 my_tmp，该表的结构和my_tab02一样
（2）把my_tmp 的记录 通过 distinct 关键字 处理后 把记录复制到my_tmp
 (3)清楚掉my_tab02记录
 (4)把my_tmp表的记录复制到my_tab02
 (5)drop掉 临时表my_tmp
*/
-- （1）先创建一张临时表 my_tmp，该表的结构和my_tab02一样
create table my_tmp like my_tab02;
--  (2)把my_tmp的记录通过distinct关键字处理后,把记录复制到my_tmp
insert into my_tmp select distinct * from my_table02;
-- (3)清除掉my_tmp记录
delete from my_tab02;
-- (4) 把my_tmp表的记录复制到my_tab02
insert into my_tab02
	select * from my_tmp;
-- (5) drop掉临时表my_tmp
drop table my_tmp;

select * from my_tab02;
```

# 合并查询

- 介绍
  有时在实际应用中，为了合并多个select语句的结果，可以使用集合操作符号
  union, union all union.sql

1. union all: 该操作符用于取得两个结果集的并集。当使用该操作符时，不会取消重复行。

2. union: 该操作赋与union all 相似，但是会自动去掉结果集中重复行

```mysql
-- 合并查询
SELECT ename,sal,job FROM emp WHERE sa1>2500 -- 5
SELECT ename,sal,job FROM emp WHERE job='MANAGER'-- 3

-- union a11 就是将两个查询结果命并，不会去重
SELECT ename,sal,job FROM emp WHERE Sa1>2500 -- 5
UNION ALL
SELECT ename, sal, job FROM emp WHERE job='MANAGER' -- 3

-- union 就是将两个查询结果合并，会去重
SELECT ename, sal, job FROM emP WHERE sal>2500 -- 5
UNION
SELECT ename, sal,job FROM emp WHERE job='MANAGER' -- 3
```

# mysql表外连接

• 外连接

1. 左外连接（如果左侧的表完全显示我们就说是左外连接）

   `select ... from 表1 left join 表2 on 条件`

   [表1 : 就是左表  表2 : 就是右表]

2. 右外连接（如果右侧的表完全显示我们就说是右外连接）
为了讲清楚，我们举例说明。

```mysql
-- 外连接
-- 比如：列出部门名称和这些部门的员工名称和工作，同时要求 显示出那些没有员工的部门，
-- 使用我们学习过的多表查询的SQL，看看效果如何？
SELECT dname,ename,job
FROM emp,dept
WHERE emp.deptno = dept.deptno
ORDER BY dname
# 无法显示null的部门

-- 演示

-- 创建 stu
create table stu (
	id INT,
  `name` VARCHAR(32);
)
insert into stu values(1,'jack'),(2,'tom'),(3,'kity'),(4,'nono');

-- 创建exam
create table exam(
	id INT,
  grade INT);
insert into exam values(1,56),(2,76),(11,8);

-- 要求
-- 使用左连接（显示所有人的成绩，如果没有成绩，也要显示该人的姓名和id号，成绩显示为空）
SELECT `name`,stu.id,grade
	FROM stu, exam
	WHERE stu.id = exam.id;
	
-- 改成外连接
select `name`,stu.id,grade
	from stu left join exam
	on stu.id = exam.id;
	
-- 使用右外连接（显示所有成绩，如果没有名字匹配，显示空）
select `name`,stu.id,grade
	from stu right join exam
	on stu.id = exam.id;
	
-- 现在解决
-- 列出部门名称和这些部门的员工信息（名字和工作），
-- 同时列出那些没有员工的部门名
-- 使用左外连接实现

select dname,ename,job
from dept left join emp
on dept.deptno = emp.deptno;
+------------+--------+-----------+
| dname      | ename  | job       |
+------------+--------+-----------+
| ACCOUNTING | MILLER | CLERK     |
| ACCOUNTING | KING   | PRESIDENT |
| ACCOUNTING | CLARK  | MANAGER   |
| RESEARCH   | FORD   | ANALYST   |
| RESEARCH   | SCOTT  | ANALYST   |
| RESEARCH   | JONES  | MANAGER   |
| RESEARCH   | SMITH  | CLERK     |
| SALES      | JAMES  | CLERK     |
| SALES      | TURNER | SALESMAN  |
| SALES      | BLAKE  | MANAGER   |
| SALES      | MARTIN | SALESMAN  |
| SALES      | WARD   | SALESMAN  |
| SALES      | ALLEN  | SALESMAN  |
| OPERATIONS | NULL   | NULL      |
+------------+--------+-----------+
14 rows in set (0.01 sec)
```

# MySQL 约束

•基本介绍
***<u>约束***</u>用于确保数据库的数据满足特定的商业规则。
在mysql中，约束包括：not null, unique, primary key, foreign key,和check五种.

> - primary key（主键）-基本使用
>   字段名 字段类型 primary key
>   用于唯一的标示表行的数据，当定义主键约束后，该列不能重复
>
> ```mysql
> -- 主键使用
> 
> -- id name email
> create table t17
> (
>   id INT primary key, -- 表示id列是主键
>   `name` varchar(32),
>   email varchar(32)
> );
> 
> -- 主键的列的值是不可以重复的
> insert into t17
> 	values(1,'jack','jack@sohu.com');
> ```
>
> > -- 主键使用的细节讨论
> > -- primary key不能重复而且不能为 null。
> > --一张表最多只能有一个主键，但可以是复合主键
> > -- 主键的指定方式 有两种
> > -- 直接在字段名后指定：字段名 primakry key
> > -- 在表定义最后写 primary key（列名）；
> > -- 使用 desc 表名，可以看到 primary key 的情况
> >
> > <u>***5． 老师提醒：在实际开发中，每个表往往都有主键.***</u>
>
> ```mysql
> -- 演示复合主键
> create table t18
> (
>   id INT,
>   `name` varchar(32),
>   email varchar(32),
>   primary key (id,`name`) -- 复合主键
>   # id和name同时相同才加不进去
> );
> ```

> - not null (非空)
>   如果在列上定义了not null，那么当插入数据时，必须为列提供数据。
>   字段名 字段类型 not null
>
> - unique（唯一）
>   当定义了唯一约束后，该列值是不能重复的。
>   字段名 字段类型 unique
>
> unique 细节(注意): unique.sql
>
> 1. 如果没有指定 not null，则 unique 字段可以有多个null
> 2. 如果一个列（字段），是 unique not nul1 使用效果类似 primary key
> 3. 一张表可以有多个unique字段
>
> ```mysql
> CREATE TABLE t21 (
>   id INT UNIQUE, -- 表示 id 列是不可以重复的.
> 	`name` VARCHAR (32),
> 	email VARCHAR (32)
> );
> ```

> foreigh key 外键
>
> 用于定义主表和从表之间的关系：外键约束要定义在从表上，主表则必须具有主键约束或是unique约束.当定义外键约束后，要求外键列数据必须在主表的主键列存在或是为null（学生/班级 图示）
>
> ![image-20240301下午91445982](/Users/yannlau/Library/Application Support/typora-user-images/image-20240301下午91445982.png)
>
> 要想删除班级表中的id=200的课程,必须在学生表中删除所有class_id=200的学生才行 , 否则会删除失败 !
>
> ```mysql
> -- 外键演示
> -- 创建 主表 my_class
> create table my_class(
> 	id int primary key,
>   `name` varchar(32) not null default ''
> );
> -- 创建从表 my_stu
> create table my_stu(
> 	id int primary key , -- 学生编号
>   `name` varchar(32) not null default '',
>   class_id int, -- 学生所在班级的编号
>   -- 下面指定一个外键关系
>   foreign key (class_id) references my_class(id)
> )
> 
> --测试数据
> INSERT INTO my_class
> 	VALUES (100,'java'), (200,'web');
> SELECT * FROM my_class;
> INSERT INTO my_stu
> 	VALUES（1，'tom'，100）;
> INSERT INTO my_stu
> 	VALUES (2, 'jack', 200) ;
> INSERT INTO my_stu
> 	VALUES (3,'hsp',300); -- 这里会失败..因为300班级不存在
> ```
>
> foreign key（外键）细节说明（创建小表演示）
>
> 1. 外键指向的表的字段，要求是primary key 或者是
>    unique
>
> 2. 表的类型是innodb，这样的表才支持外键
> 3. 外键字段的类型要和主键字段的类型一致（长度可以不同）
> 4. 外键字段的值，必须在主键字段中出现过，或者为null［前
>    提是外键字段允许为null］
> 5. 一旦建立主外键的关系，数据不能随意删除了.

> check
>
> 用于强制行数据必须满足的条件，假定在sal列上定义了check约束，并要求sal列值在1000~2000之间如果不再1000~2000之间就会提示出错。
>
> oracle 和 sql server 均支持check，但是mysql 5.7目前还不支
> 持check，只做语法校验，但不会生效。mysql8支持
>
> 基本语法：列名 类型  check  （check条件）
> user  表
> id, name, sex(man,woman), sal(大于100 小于 900)
>
> ```mysql
> CREATE TABLE t23 (
> 	id INT PRIMARY KEY,
> 	`name` VARCHAR (32),
> 	sex VARCHAR(6) CHECK ( sex IN ('man', 'woman' )),
> 	sal DOUBLE CHECK ( sal > 1000 AND sal < 2000) 
> );
> ```

# 商店表设计

- 商店售货系统表设计案例［先练，再评］

现有一个商店的数据库shop_db，记录客户及其购物情况，由下面三个表组成：
商品goods（商品号goods_id，商品名goods_name，单价unitprice，商品类别category，供应商provider）;
客户customer（客户号customer_id，姓名name，住址address，电邮email性别sex，身份证card_ld）;
购买purchase（购买订单号order_id，客户号customer_id，商品号goods_id，购买数量nums）;

建表，在定义中要求声明［进行合理设计］：
（1）每个表的主外键；
（2）客户的姓名不能为空值；
（3）电邮不能够重复；
（4）客户的性别［男|女］
（5）单价unitprice 在1.0- 9999.99之间 check

```mysql
create table goods (
  goods_id int primary key,
  goods_name varchar(32) not null default '',
  unitprice decimal(10,2) not null default 0 check (unitprice >= 1.0 and unitprice <= 9999.99),
  category int not null default 0,
  provider varchar(32) not null default ''
);

create table customer (
  customer_id int primary key,
  `name` varchar(32) not null default '',
  address varchar(32) not null default '',
  email varchar(32) unique not null,
  sex char(1) check (sex in ('男','女')),
  # sex enum('男','女') not null , -- 这里老师用的枚举类型,是生效的
  card_id varchar(18) unique
);

create table purchase (
  order_id varchar(32) primary key,
  customer_id int not null default 0,
  good_id int not null default 0,
  nums int not null default 0,
  foreign key(customer_id) references customer(customer_id),
  foreign key(goods_id) references goods(goods_id)
);
```

# 自增长

- 自增长基本介绍 一个问题

  在某张表中，存在一个id列（整数），我们希望在添加记录的时候，该列从1开始，自动的增长，怎么处理？
  字段名 整型 primary key auto_increment

  ```mysql
  -- 添加 自增长的字段方式
  insert into xxx（字段1，字段2...) values(null,
  '值'..);
  insert into xxx(字段2....) values('值1'，'值2'..);
  insert into xxx values(null, '值1',.....)
  
  -- 演示自增长的使用
  -- 创建表
  CREATE TABLE t24
  	(id INT PRIMARY KEY AUTO_ INCREMENT,
  		email VARCHAR(32) NOT NULL DEFAULT '',
  		`name` VARCHAR(32) NOT NULL DEFAULT '');
  -- 测试自增长
  insert into t24 values(null,'tom@qq.com','tom');
  
  -- 修改默认的自增长开始值
  
  CREATE TABLE t25
  (id INT PRIMARY KEY AUTO INCREMENT,
  email VARCHAR(32) NOT NULL DEFAULT '',
  name VARCHAR(32) NOT NULL DEFAULT '');
  
  ALTER TABLE t25 AUTO_INCREMENT = 100;
  
  INSERT INTO t25
  VALUES(NULL, 'tom@qg.com', 'tom');
  
  INSERT INTO t25
  VALUES (666, 'hsp@qq.com', 'hsp');
  
  SELECT * FROM t25;
  ```

> • 自增长使用细节
>
> 1. 一股来说自增长是和primary key 配合使用的
> 2. 自增长也可以单独使用［但是需要配合一个unique］
> 3. 自增长修饰的字段为整数型的（虽然小数也可以但是非常非常少这样使用）
>
> 4. 自增长默认从 1开始，你也可以通过如下命令修改
>
>   alter table 表名 auto_increment = xxx;
>
> 5. 如果你添加数据时，给目增长子段（列）指定的有值，则以捐定的值为准，如果指定了自增长，一般来说，就按照自增长的规则来添加数据

# 索引

说起提高数据库性能，索引是最物美价廉的东西了。不用加内存，不用改程序，不用调sql，查询速度就可能提高百倍干倍。

> ```mysql
> #添加8000000数据
> CALL insert_emp(100001, 8000000)$$
> #命令结束符，再重新设置为；
> DELIMITER；
> 
> SELECT COUNT (*) FROM emp;
> 
> -- 在没有创建索引时，我们的查询一条记录
> SELECT * FROM emp
> WHERE empno = 1234567;
> 
> -- 使用索引来优化一下,体验索引的牛
> -- 在没有创建索引前，，emp.ibd 文件大小 是 524MB
> -- 创建索引后 emp.ibd 文件大小 是 655m ［索引本身也会占用空间.］
> -- 创建ename列索引，emp.ibd 文件大小是827m
> -- empno_index 索引名称
> -- ON emp （empno）：表示在 emp表的 empno列创建索引
> CREATE INDEX empno index ON emp(empno);
> 
> -- 创建索引后，查询的速度如何
> SELECT * FROM emp
> 	WHERE empno = 1234568; -- 0.003秒 原来是4.5秒
> 	
> 	
> 是不是建立一个索引就能解决所有的问题？
> ename上没有建立索引会怎样？
> select * from emp where ename= 'axJxC' ;
> 
> -- 创建索引后，只对创建了索引的列有效
> SELECT * FROM emp
> WHERE ename ='PjDlwy';-- 没有在ename创建索引时，时间4.7s
> CREATE INDEX ename_index ON emp（ename）-- 在ename上创建索引
> ```

> 索引的原理
>
> 没有索引为什么会慢？因为会全表扫描
> 使用索引为什么会快？形成一个索引的数据结构，比如二又树
> 索引的代价
>
> 1. 磁盘占用
>
> 2. 对dml（update delete insert）语句的效率影响
>
> 老师告诉你，在我们项目中，select［90%］多 还是
> update,delete,insert［10%］ 操作多？
>
> ![image-20240302下午124440092](/Users/yannlau/Library/Application Support/typora-user-images/image-20240302下午124440092.png)

> 索引类型
>
> 1. 主键索引，主键自动的为主索引（类型Primary key）
>
> 2. 唯一索引（UNIQUE）
>
> 3. 普通索引（INDEX）
>
> 4. 全文索引 （FULLTEXT）［适用于MyISAM］
>     一般开发中,不使用mysql自带的全文索引,而是考虑使用：全文搜索 Solr 和 ElasticSearch （ES）
>
> 5. create table t1 (
>     id int primary key， -- 主键，同时也是索引，称为主键索引, 查询速度飞快
>     name varchar (32) );
>
> 6. create table t2(
>
>   id int unique, -- id是唯一的,同时也是索引,称为unique索引
>
>   name varchar(32) );

> 索引使用
>
> 1. 添加索引（建小表测试 id，name）index_use.sql
>    create [UNIQUE] index index name on tbi name (col name I(length)]
>    [ASC | DESC], .....);
>
>    alter table table_name ADD INDEX [index namel (index col name,...)
>
> 2. 添加主键（索引）ALTER TABLE 表名 ADD PRIMARY KEY（列名...）;
>
> 3. 删除索引
>    DROP INDEX index_name ON tbl name;
>    alter table table_name drop index index name;
>
> 4. 删除主键索引 比较特别：alter table t_b drop primary key;
>
> ```mysql
> -- 演示mysq1的索引的使用
> -- 创建索引
> CREATE TABLE t25 (
> id INT ,
> `name` VARCHAR(32));
> 
> -- 查询表是否有索引
> show index from t25;
> 
> -- 添加索引
> -- 添加唯一索引
> create unique index id_index on t25(id);
> 
> 
> -- 添加普通索引
> create index id_name on t25(id);
> 或者
> alter table t25 add index id_index (id);
> 
> -- 如何选择
> -- 1. 如果某列的值，是不会重复的，则优先考虑使用unique索引，否则使用普通索引
> 
> -- 添加主键索引
> create table t26(
> 	id int,
>   `name` varchar(32)
> );
> alter table t26 add primary key (id)
> 
> -- 删除索引
> DROP INDEX id index ON t25
> -- 删除主键索引
> ALTER TABLE t26 DROP PRIMARY KEY
> 
> -- 修改索引,就是先删除,再添加
> 
> -- 查询索引
> -- 1.方式
> SHOW INDEX FROM t25
> -- 2. 方式
> SHOW INDEXES FROM t25
> -- 3.方式
> SHOW KEYS FROM t25
> -- 4.方式
> DESC t25
> ```

> • 小结：哪些列上适合使用索引
> 1. 较频繁的作为查询条件字段应该创建索引
> select * from emp where empno = 1
> 2. 唯一性太差的字段不适合单独创建索引，即使频繁作为查询
> select * from emp where sex ='男，
> 3. 更新非常频繁的字段不适合创建索引
> select * from emp where logincount = 1
> 4. 不会出现在WHERE子句中字段不该创建索引

# 事务

• 什么是事务
事务用于保证数据的一致性，它由一组相关的dml语句组成，该组的dml语句要么全部成功，要么全部失败。如：转账就要用事务来处理，用以保证数据的一致性。

![image-20240302下午32436789](/Users/yannlau/Library/Application Support/typora-user-images/image-20240302下午32436789.png)

- 事务和锁

当执行事务操作时（dml语句），mysq|会在表上加锁.防止其它用户改表的数据.这对用户来讲是非常重要的

- mysql 数据库控制台事务的几个重要操作（基本操作 transaction.sql） 

1. start transaction -- 开始一个事务
2. savepoint 保存点名-- 设置保存点
3. rollback to 保存点名- 回退事务
4. rollback- 回退全部事务
5. commit -- 提交事务，所有的操作生效，不能回退

- 细节

  没有设置保存点
  多个保存点
  存储引擎
  开始事务方式

```mysql
-- 事务的一个重要的概念和具体操作
-- 一张图说明

-- 演示
-- 1. 创建一张测试表
CREATE TABLE t27
(id INT,
`name` VARCHAR (32)) ;
-- 2. 开始事务
START TRANSACTION
-- 3.设置保存点
savepoint a
-- 执行dml操作
insert into t27 values(10,'tom');
select * from t27;

savepoint b

-- 执行dml操作
insert into t27 values(200,'jack');

-- rollback to操作回滚到某个保存点时，该保存点之后的所有保存点都会被无效化，无法再次回滚到这些保存点。
-- 回退至 b
rollback to b
-- 回退到 a
rollback to a
-- 如果直接执行,不指定点,则直接回退到事务开始的状态!
rollback

-- 提交
commit
```

- 回退事务
  在介绍回退事务前，先介绍一下保存点（savepoint）.保存点是事务中的点.用于取消部分事务，当结束事务时 （commit），会自动的删除该事务所定义的所有保存点.当执行回退事务时，通过指定保存点可以回退到指定的点，这里我们作图说明
- 提交事务
  它会话［其他连接］将可以查看到事务变化后的新数据［所有数据就正式生效.］

> 事务细节
>
> 1. 如果不开始事务，默认情况下，dml操作是自动提交的，不能回滚
> 2. 如果开始一个事务，你没有创建保存点.你可以执行 rollback，默认就是回退到你事务开始的状态.
> 3. 你也可以在这个事务中（还没有提交时），创建多个保存点.比如：savepoint aaa; #7 dml, savepoint bbb;
> 4. 你可以在事务没有提交前，选择回退到哪个保存点.
> 5. mysql的事务机制需要innodb的存储引擎还可以使用，myisam不好使.
> 6. 开始一个事务 start transaction，
>     set autocommit=off;
>
> ```mysql
> -- 讨论 事务细节
> -- 1. 如果不开始事务，默认情况下，dm1操作是自动提交的，不能回滚
> INSERT INTO t27 VALUES （300，'milan'）；-- 自动提交 commit
> SELECT * FROM t27
> 
> -- 2. 如果开始一个事务，你没有创建保存点.你可以执行 rol1back，
> -- 默认就是回退到你事务开始的状态
> START TRANSACTION
> INSERT INTO t27 VALUES (400, 'king');
> INSERT INTO t27 VALUES (500, 'scott');
> ROLLBACK -- 表示直接回退到事务开始的的状态
> 
> -- 3. 你也可以在这个事务中（还没有提交时），创建多个保存点.
> 
> -- 4. 你可以在事务没有提交前，选择回退到哪个保存点.
> 
> -- 5. mysql的事务机制需要innodb的存储引擎还可以使用，myisam不好使.
> 
> -- 6.开始一个事务 , 以下二选一
> start transaction，
> set autocommit=off;
> ```

# 事务-隔离级别

![image-20240302下午44426459](/Users/yannlau/Library/Application Support/typora-user-images/image-20240302下午44426459.png)

> - 事务隔离级别介绍
>   1. 多个连接开启各自事务操作数据库中数据时，数据库系统要负责隔离操作，以保证各个连接在获取数据时的准确性。（通俗解釋）
>   2. 如果不考虑隔离性，可能会引发如下问题：
>      ＞脏读
>      ＞不可重复读
>      ＞幻读
>
> - 查看事务隔级别 查看当前会话隔离级别 select @@tx isolation；
>   - 脏读（dirty read）：当一个事务读取另一个事务尚未提交的修改(update insert delete)时，产生脏读
>   - 不可重复读 （nonrepeatable read）：同一查询在同一事务中多次进行，由于其他提交事务所做的修改或删除，每次返回不同的结果集，此时发生不可重复读。
>   - 幻读 （phantom read）：同一查询在同一事务中多次进行，由于其他提交事务所做的插入操作，每次返回不同的结果集，此时发生幻读。
>
> ```mysql
> -- 演示mysq1的事务隔离级别
> -- 1. 开了两个mysq1的控制台
> -- 2. 查看当前mysq1的隔离级别
> mysql> select @@transaction_isolation;
> +-------------------------+
> | @@transaction_isolation |
> +-------------------------+
> | REPEATABLE-READ         |
> +-------------------------+
> 1 row in set (0.00 sec)
> 
> -- 3. 把其中一个控制台的隔离级别设置 Read uncommitted
> set session transaction isolation level read uncommited;
> 
> -- 4．创建表
> CREATE TABLE `account`(
> 	id INT,
> 	name VARCHAR (32) ,
> 	money INT);
> ```

> Mysql 事务隔离级别
>
> 1. 查看当前会话隔离级别
>    select @@tx_isolation;
>    select @@transaction_isolation
>
> 2. 查看系统当前隔离级别
>     select @@global.tx_isolation;
>     select  @@global.transaction_isolation
> 3. 设置当前会话隔离级别
>     set session transaction isolation level [repeatable read];
> 4. 设置系统当前隔离级别
>     set global transaction isolation level [repeatable read];
> 5. mysql 默认的事务隔离级别是 repeatable read , 一般情况下，没有特殊要求，没有必要修改（因为该级别可以满足绝大部分项目需求）

> mysq|事务ACID
> • 事务的acid特性
>
> 1. 原子性 （Atomicity）
>    原子性是指事务是一个不可分割的工作单位，事务中的操作要么都发生，要么都不发生。
> 2. 一致性（Consistency）
>    事务必须使数据库从一个一致性状态变换到另外一个一致性状态
> 3. 隔离性 （Isolation）
>    事务的隔离性是多个用户并发访问数据库时，数据库为每一个用户开启的事务，不能被其他事务的操作数据所干扰，多个并发事务之间要相互隔离。
> 4. 持久性（Durability）
>    持久性是指一个事务一旦被提交，它对数据库中数据的改变就是永久性的，接下来即使数据库发生故障也不应该对其有任何影响

# 表类型和存储引擎

• 基本介绍
1. MySQL的表类型由存储引擎 （Storage Engines）决定，主要包括MyISAM、innoDB、Memory等。
2. MySQL 数据表主要支持六种类型，分别是：CSV \ Memory、ARCHIVE \ MRG_MYISAM \ MYISAM \ InnoBDB.
3. 这六种又分为两类，一类是”事务安全型"（transaction-safe），比如：InnoDB：其余都属于第二类，称为”非事务安全 型"（non-transaction-safe) [mysiam 和 memory].

![image-20240302下午80808001](/Users/yannlau/Library/Application Support/typora-user-images/image-20240302下午80808001.png)

![image-20240302下午81214195](/Users/yannlau/Library/Application Support/typora-user-images/image-20240302下午81214195.png)

```mysql
-- 表类型和存储类型

-- 查看所有的存储引擎

show engines;

-- innodb 存储引擎，是前而使用过.
-- 1. 支持事务 2. 支持外键 3. 支持行级锁

-- myisam 存储引擎
CREATE TABLE t28 (
	id INT,
	`name` VARCHAR(32)) 
	ENGINE MYISAM;
-- 1. 添加速度快 2. 不支持外键和事务 3. 支持表级锁

-- memory 存储引擎
-- 1．数据存储在内存中［关闭了Mysql服务，数据丢失，但是表结构还在］
-- 2. 执行速度很快（没有IO读写）3. 默认支持索引（hash表）
CREATE TABLE t29(
	id INT,
	`name` VARCHAR (32)) ENGINE MEMORY;
DESC t29
INSERT INTO t29 VALUES (1, 'tom'), (2, 'jack'), (3, 'hsp');
SELECT * FROM t29;

-- 指令修改存储引擎
ALTER TABLE t29 ENGINE = INNODB
```

> 细节
>
> 1. MyISAM不支持事务、也不支持外键，但其访问速度快，对事务完整性没有要求
> 2. InnoDB存储引擎提供了具有提交、回滚和崩溃恢复能力的事务安全。但是比起MyISAM存储引擎，InnoDB写的处理效率差一些并且会占用更多的磁盘空间以保留数据和索引。
> 3. MEMORY存储引擎使用存在内存中的内容来创建表。每个MEMORY表只实际对应一个磁盘文件。MEMORY类型的表访问非常得快，因为它的数据是放在内存中的，并且默认使用HASH索引。但是一旦mysql服务关闭，表中的数据就会丢失掉，表的结构还在。

> • 如何选择表的存储引擎
> 1. 如果你的应用不需要事务，处理的只是基本的CRUD操作，那么MyISAM是不二选择，速度快
> 2. 如果需要支持事务，选择InnoDB。
> 3. Memory 存储引擎就是将数据存储在内存中，由于没有磁盘1./0的等待，速度极快。但由于是内存存储引擎，所做的任何修改在服务器重启后都将消失。（经典用法 用户的在线状态（）.）

# 视图原理

![image-20240303上午91023809](/Users/yannlau/Library/Application Support/typora-user-images/image-20240303上午91023809.png)

• 视图的基本使用

1. create view 视图名 as select语句

2. alter view 视图名 as select语句--更新成新的视图
3. SHOW
SHOW PREATE VIEW 视图名
4. drop view 视图名1，视图名2

```mysql
-- 创建视图
CREATE VIEW emp_view01 AS
SELECT empno, ename, job, deptno FROM emp;
--查看视图
DESC emp view01;
SELECT * FROM emp view01;
SELECT empno, job FROM emp;
-- 查看创建视图的指令
SHOW CREATE VIEW emp_view01;
-- 删除视图
DROP VIEW emp view01;
```

```mysql
-- 视图的细节
-- 1. 创建视图后，到数据库去看，对应视图只有一个视图结构文件（形式：视图名.frm）
-- 2.视图的数据变化会影响到基表，基表的数据变化也会影响到视图 ［insert update delete ］

-- 修改视图 会影响到基表
UPDATE emp_view01
	SET job = 'MANAGER'
	WHERE empno = 7369

SELECT * FROM emp;-- 查询基表

select * from emo_view01;

-- 修改基本表，会影响到视图
UPDATE emp
	SET job = 'SALESMAN'
	WHERE empno = 7369

-- 3. 视图中可以再使用视图，比如从emp_view01 视图中，选出empno，和ename做 出！新视图

DESC emp_view01

CREATE VIEW emp_view02 AS
	SELECT empno, ename FROM emp_view01

SELECT * FROM emp_view02
```

> • 视图最佳实践
> 1. 安全。一些数据表有着重要的信息。有些字段是保密的，不能让用户直接看到。这时就可以创建一个视图，在这张视图中只保留一部分字段。这样，用户就可以查询自己需要的字段，不能查看保密的字段。
> 2. 性能。关系数据库的数据常常会分表存储，使用外键建立这些表的之间关系。这时，数据库查询通常会用到连接（JOIN）。这样做不但麻烦，效率相对也比较低。如果建立一个视图，将相关的表和字段组合在一起，就可以避免使用JOIN查询数据。
> 3. 灵活。如果系统中有一张旧的表，这张表由于设计的问题，即将被废弃。然而，很多应用都是基于这张表，不易修改。这时就可以建立一张视图，视图中的数据直接映射到新建的表。这样，就可以少做很多改动，也达到了升级数据表的目的。

```mysql
-- 针对 emp, dept, 和 salgrade 张三表.创建一个视图emp_view03，可以显示雇员编号，雇员名，雇员部门名称 和 薪水级别

/*
分析：使用三表联合查询，得到结果
将得到的结果，构建成视图
*/
create view emp_view03 as
	SELECT empno, ename, dname, grade
		FROM emp, dept, salgrade
			WHERE emp. deptno = dept. deptno AND
			(sal BETWEEN losal AND hisal);
	
desc emp_view03;
```

# MySQL管理

![image-20240303上午94415168](/Users/yannlau/Library/Application Support/typora-user-images/image-20240303上午94415168.png)

```mysql
-- Mysql 用户的管理
-- 原因：当我们做项目开发时，可以根据不同的开发人员，赋给他相应的Mysg操作权限
-- 所以，Mysql数据库管理人员（root），根据需要创建不同的用户，赋给相应的权限，供人员使用

-- 1. 创建新的用户
-- 解读（1）'hsp_edu'e'1ocalhost，表示用户的完整信息'hsp_edu'用户名'1ocalhost！登录的IP
-- （2） 123456 密码，但是注意 存放到 mysql.user表时，是password（'123456'）加密后的密码
-- *6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9
CREATE USER 'hsp_edu'@ 'localhost' IDENTIFIED BY '123456';
SELECT 'host', user, autentication_string
		FROM mysql.user;

-- 2. 删除用户
DROP USER 'hsp_edu'@'localhost'

-- 3. 登录
		
-- 4. 修改自己的密码,没问题
set password = password('abcdef');

-- 5.修改其他人的密码，需要权限
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('123456');

-- root用户修改 hsp_edu@localhost密码,是可以成功的
SET PASSWORD FOR 'hsp_edu'@'localhost' = PASSWORD ('123456')


```

![image-20240303上午95342397](/Users/yannlau/Library/Application Support/typora-user-images/image-20240303上午95342397.png)

> - 给用户授权
>   - 基本语法：
>     grant 权限列表 on 库.对象名 to ‘用户名’@’登录位置'
>     [identified by ‘密码’]
>   - 说明：
>     - 权限列表，多个权限用逗号分开
>       grant select on.......
>       grant select, delete, create on ....
>       grant all 【privileges】 on .... //表示赋予该用户在该对象上的所有权限
>     - 特别说明
>       - '' \* ''：代表本系统中的所有数据库的所有对象（表，视图，存储过程)
>       - 库.*：表示某个数据库中的所有数据对象（表，视图，存储过程等）
>       - identified by可以省略，也可以写出.
>         - 如果用户存在，就是修改该用户的密码。
>         - 如果该用户不存在，就是创建该用户！
> - 回收用户授权
>   基本语法：
>   revoke 权限列表 on 库.对象名 from ‘用户名"@"登录位置’；
> - 权限生效指令
>   如果权限没有生效，可以执行下面命令.
>   基本语法：FLUSH PRIVILEGES;

```mysql
-- 演示 用户权限的管理
-- 创建用户 shunping 密码 123，从本地登录
CREATE USER 'shunping'@'localhost' IDENTIFIED BY '123'
-- 使用root 用户创建 testdb，表 news
CREATE DATABASE testdb
CREATE TABLE news（
	id INT ,
	content VARCHAR (32) ) ;
-- 添加一条测试数据
INSERT INTO news VALUES（100，'北京新闻'）；
SELECT * FROM news ;

-- -- 给 shunping 分配查看 news 表和 添加news的权限
GRANT SELECT , INSERT ON testdb.news
TO 'shunping'@'localhost'

-- shunping能否修改，能否delete? 不行,没有update权限
UPDATE news SET content ='成都新闻'，WHERE id = 100

-- 回收 shunping 用户在 testdb.news 表的所有权限
REVOKE SELECT , UPDATE, INSERT ON testdb news FROM 'shunping'@'localhost';
REVOKE ALL ON testdb.news FROM 'shunping'@'localhost';

-- -- 删除 shunping
DROP USER 'shunping'@'localhost'
```

```mysql
-- 说明 用户管理的细节
-- 在创建用户的时候，如果不指定Host，则为%，%表示表示所有IP都有连接权限
-- create user xxx：
CREATE USER jack;
SELECT host,'user' FROM mysql.user;

-- 你也可以这样指定
-- create user 'XXX'@'192.168.1.% 表示 xxx用户在 192.168.1.*的ip可以登录mysql
CREATE USER 'smith'@'192.168.1.%';

-- 在删除用户的时候，如果 host 不是 %，需要明确指定
-- '用户'@'host值'
DROP USER 'smith'@'192.168.1.%'
```

# 课后题

last_day(日期)  可以返回该日期所在月份的最后一天
