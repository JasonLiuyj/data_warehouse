#### DDL

* 数据库操作：

  ```mysql
  show databases;
  create database 数据库名 if no exists 数据库名;
  use 数据库名;
  select database(); 查询目前使用数据库
  drop database 数据库名 if exists 数据库名;
  ```

* 表操作：

  ```mysql
  show tables;
  create table 表名(字段 字段类型, 字段 字段类型);
  desc 表名;
  show create table 表名;  查询该表的建表语句
  alter table 表名 add/modify/change/drop/rename to;
  drop table 表名;
  ```

#### DML

* 添加数据：

  ```mysql
  insert into 表名(字段1, 字段2) values(值1, 值2);
  ```

* 修改数据：

  ```mysql
  update 表名 set 字段1=值1, 字段2=值2 [where 条件];
  ```

* 删除数据：

  ```mysql
  delete from 表名 [where 条件];
  ```

#### DQL

```mysql
select
	字段列表 [字段名 as 别名]
from
	表名
where  分组前过滤
	条件列表
group by
	分组字段列表
having  分组后过滤
	分组后的条件列表
order by
	排序字段列表
limit 行数 offset 起始行
```

#### DCL

##### 管理用户

* 查询用户：

  ```mysql
  USE mysql;
  SELECT * FROM user;
  ```

* 创建用户：

  ```mysql
  create user "itcast"@'localhost' identified by '123456';
  ```

* 修改用户密码：

  ```mysql
  ALTER USER "itcast"@"localhost" identified with mysql_native_password by '1234';
  ```

* 删除用户：

  ```mysql
  drop user 'itcast'@'localhost';
  ```

主机名可以使用 % 进行通配。

##### 权限控制

* 查询权限：

  ```mysql
  show grants for 'iscast'@'localhost';
  ```

* 授予权限：

  ```mysql
  grant 权限列表 on 数据库名.表名 to '用户名'@'主机名';
  ```

* 撤销权限：

  ```mysql
  revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';
  ```

多个权限之间使用逗号分隔；授权时数据库名和表名都可以使用 * 进行通配。

#### 事务

##### 事务操作

* 方式一：

  1. 事务手动提交：

     ```mysql
     select @@autocommit;
     set @@autocommit = 0; // 设置手动提交数据
     ```

  2. 提交事务：

     ```mysql
     commit;
     ```

  3. 回滚事务：

     ```mysql
     rollback;
     ```

* 方式二：

  1. 开启事务：

     ```mysql
     start transaction 或 begin;
     ```

  2. 提交事务：

     ```mysql
     commit;
     ```

  3. 回滚事务：

     ```mysql
     rollback;
     ```

##### 四大特性

* 原子性：事务是不可分割的最小操作单元，要么全部成功，要么全部失败。
* 一致性：事务完成时，必须使所有的数据都保持一致状态。
* 隔离性：数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行。
* 持久性：事务一旦提交或回滚，它对数据库的数据的改变是永久的。

##### 并发事务的问题

* 脏读：一个事务可以查到另外一个未提交的事务。
* 不可重复读：同一个事务在查询的过程中前后不一致。
* 幻读：无法插入无法查到的数据。

##### 事务隔离级别

* 查看事务隔离级别：

  ```mysql
  select @@transaction_isolation;
  ```

* 设置事务隔离级别：

  ```mysql
  set [session | global] transaction isolation level {read uncommited|read commited|repeatable read|serializable}
  ```

#### 存储引擎

##### MySQL体系结构

<img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241030161915133.png" alt="image-20241030161915133" style="zoom:50%;" />

* 连接层：最上层是一些客户端和链接服务，主要完成一些类似于连接处理、授权认证、及相关的安全方案。服务器也会为安全接入的每个客户端验证它所具有的操作权限。
* 服务层：第二层架构主要完成大多数的核心服务功能，如SQL接口，并完成缓存的查询，SOL的分析和优化，部分内置函数的执行。所有跨存储引擎的功能也在这一层实现，如 过程、函数等。
* 引擎层：存储引擎真正的负责了MySOL中数据的存储和提取，服务器通过API和存储引擎进行通信。不同的存储引擎具有不同的功能，这样我们可以根据自己的需要，来选取合适的存储引擎。（**不同的存储引擎索引的方式不同**）
* 存储层：主要是将数据存储在文件系统之上，并完成与存储引擎的交互。

##### 存储引擎简介

* 创建表时指定存储引擎：

  ```mysql
  create table my_myisam(
  	id int,
      name varchar(10)
  ) engine = MyISAM;
  ```

* 查看当前数据库支持的存储引擎：

  ```mysql
  show engines;
  ```

##### 存储引擎特点

* InnoDB的逻辑存储结构：

  <img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241030164159090.png" alt="image-20241030164159090" style="zoom:50%;" />

* MyISAM

* Memory

##### 存储引擎选择

* InnoDB：是Mysql的默认存储引擎，支持事务、外键。如果应用对事务的完整性有比较高的要求，在并发条件下要求数据的一致性，数据操作除了插入和查询之外，还包含很多的更新、删除操作，那么InnoDB存储引擎是比较合适的选择。

* MyISAM：如果应用是以读操作和插入操作为主，只有很少的更新和删除操作，并且对事务的完整性、并发性要求不是很高，那么选择这个存储引擎是非常合适的。

* MEMORY：将所有数据保存在内存中，访问速度快，通常用于临时表及缓存。MEMORY的缺陷就是对表的大小有限制，太大的表无法缓存在内存中，而且无法保障数据的安全性。

#### 索引

* B-Tree：中间元素向上分裂；在索引的位置同时存放数据。
* B+Tree：中间元素向上分裂；所有的数据都会出现叶子节点，并且形成一个单向链表。
* Hash（Memory引擎）：查询效率高；只能使用对等比较，不支持范围查询。

##### 索引分类

* 聚集索引：有且只有一个；叶子节点存放行数据。

  选取原则：

  1. 主键。
  2. 第一个unique。
  3. 自动生成一个rowid作为隐藏的聚集索引。

* 二级索引：可以有多个；叶子节点存放主键。

回表查询：先从二级索引查到主键，再到聚集索引中找到对应的行数据。

##### 索引语法

* 创建索引：

  ```mysql
  create [unique|fulltext] index index_name on table_name (index_col, ……)
  ```

* 查看索引：

  ```mysql
  show index from table_name;
  ```

* 删除索引：

  ```mysql
  drop index index_name on table_name;
  ```

##### SQL性能分析

* SQL执行频率：

  ```mysql
  show [session|global] status like 'Com_______';
  ```

* 慢查询日志：

* explain执行计划：

  ```mysql
  explain select * from table_name where ……;
  ```

  id：select查询中表的执行顺序（id相同，从上到下；id不同，值越大，越先执行）

  type：表示连接类型，性能由好到差的连接类型为：NULL、system、const、eq_ ref、ref、range、index、all。

  possible_key：可能用到的索引。

  Key：实际用到的索引。

##### 索引使用

* 最左前缀法则：如果索引了多列(联合索引)，要遵守最左前缀法则。最左前缀法则指的是查询从索引的最左列开始，并且不跳过索引中的列。如果跳跃某一列，索引将部分失效(后面的字段索引失效)。

* 范围查询：联合索引中，出现范围查询(>, <)，范围查询右侧的列索引失效。

* 索引列运算：不要在索引列上进行运算，否则**索引失效**。

* 字符串不加引号：字符串类型字符使用时，不加引号，**索引将失效**。

* 模糊查询：如果仅仅是尾部模糊匹配，索引不会失效。如果是头部模糊匹配，**索引失效**。

* or连接的条件：用or分割开的条件，如果or前的条件中的列有索引， 而后面的列中没有索引，那么涉及的索引都不会被用到。

* 数据分布影响：如果MySQL评估使用索引比全表更慢，则不使用索引（is null和is not null）。

* SQL提示：SQL提示，是优化数据库的一个重要手段，简单来说，就是在SQL语句中加入一些人为的提示来达到优化操作的目的。

  * use index
  * ignore index
  * force index

* 覆盖索引：尽量使用覆盖索引(查询使用了索引，并且需要返回的列，在该索引中已经全部能够找到，减少select *。

  * using index condition：查找使用了索引，但是需要回表查询数据。
  * using where; using index：查找使用了索引，但是需要的数据都在索引列中能找到，所以不需要回表查询数据。

* 前缀索引：当字段类型为字符串(varchar, text等)时,有时候需要索引很长的字符串，这会让索引变得很大,查询时，浪费大量的磁盘l0，影响查询效率。此时可以只将字符串的一部分前缀,建立索引，这样可以大大节约索引空间，从而提高索引效率。

  ```mysql
  create index idx_ on tb_(column(5));
  ```

##### 索引设计原则

1. 针对于数据量较大，且查询比较频繁的表建立索引。
2. 针对于常作为查询条件(where) 、排序(order by) 、分组(group by)操作的字段建立索引。
3. 尽量选择区分度高的列作为索引，尽量建立唯一索引， 区分度越高，使用索引的效率越高。
4. 如果是字符串类型的字段，字段的长度较长，可以针对于字段的特点，建立前缀索引。
5. 尽量使用联合索引，减少单列索引，查询时，联合索引很多时候可以覆盖索引，节省存储空间，避免回表,提高查询效率。
6. 要控制索引的数量，索引并不是多多益善，索引越多,维护索引结构的代价也就越大，会影响增删改的效率。
7. 如果索引列不能存储NULL值，请在创建表时使用NOT NULL约束它。当优化器知道每列是否包含NULL值时，它可以更好地确定哪个索引最有效地用于查询。

#### SQL优化

##### 插入数据

* insert优化：

  * 批量插入
  * 手动提交事务
  * 主键顺序插入

* 大批量插入数据：如果一次性需要插入大批量数据,使用insert语句插入性能较低，此时可以使用MySQL数据库提供的**load指令**进行插入。

  ```mysql
  #客户端连接服务端时，加上参数--local-infile
  mysql --local-infile -u root -p
  #设置全局参数local infile为， 开启从本地加载文件导入数据的开关
  set global local_infile = 1;
  #执行load指令将准备好的数据，加载到表结构中
  load data local infile '/root/sql1.log' into table 'tb_ _user' fields terminated by ',' lines terminated by \n';
  ```

##### 主键优化

* 页分裂：页中的数据溢出时发生页分裂。
* 页合并
* 主键设计原则：
  * 满足业务需求的情况下，尽量降低主键的长度。
  * 插入数据时，尽量选择顺序插入，选择使用AUTO_INCREMENT自增主键。
  * 尽量不要使用UUID做主键或者是其他自然主键，如身份证号。
  * 业务操作时，避免对主键的修改。

##### order by优化

* 根据排序字段建立合适的索引，多字段排序时，也遵循最左前缀法则。
* 尽量使用覆盖索引。
* 多字段排序,一个升序，一个降序，此时需要注意联合索引在创建时的规则(ASC/DESC)。
* 如果不可避免的出现filesort,大数据量排序时，可以适当增大排序缓冲区大小sort_ buffer_ size(默认256k)。

排序时的两种方式：

* Using filesort：通过表的索引或全表扫描，读取满足条件的数据行，然后在排序缓冲区sort buffer中完成排序操作,所有不是通过索引直接返回排序结果的排序都叫FileSort排序。
* Using index：通过有序索引顺序扫描直接返回有序数据，这种情况即为using index, 不需要额外排序，操作效率高。

##### group by优化

* 在分组操作时，可以通过索引来提高效率。
* 分组操作时，索引的使用也是满足最左前缀法则的。

##### limit优化

一般分页查询时，通过创建覆盖索引能够比较好地提高性能，可以通过覆盖索引加子查询形式进行优化。

##### count优化

优化方式：自己进行计数操作。

count(*)的效率最高，数据库专门做了优化。

##### update优化

执行update时使用索引进行查找，否则行锁将升级为表锁。

#### 存储对象

##### 视图

* 创建视图：

  ```mysql
  CREATE [OR REPLACE] VIEW 视图名称[(列名列表)] AS SELECT语句 [ WITH[ CASCADED| LOCAL] CHECK OPTION ]
  ```

* 查询视图：

  ```mysql
  查看创建视图语句: SHOW CREATE VIEW 视图名称;
  查看视图数据: SELECT * FROM 视图名称;
  ```

* 修改视图：

  ```mysql
  方式一: CREATE [OR REPLACE] VIEW 视图名称[(列名列表)] AS SELECT语句 [ WITH[ CASCADED| LOCAL ] CHECK OPTION]
  方式二: ALTER VIEW视图名称[(列名列表)] AS SELECT语句 [ WITH[ CASCADED | LOCAL ] CHECK OPTION]
  ```

* 删除视图：

  ```mysql
  DROP VIEW [IF EXISTS] 视图名称;
  ```

* 视图检查选项：当使用WITH CHECK OPTION子句创建视图时, MySQL会通过视图检查正在更改的每个行，例如插入，更新，删除，以使其符合视图的定义。MySQL允许基于另一个视图创建视图，它还会检查依赖视图中的规则以保持一致性。为了确定检查的范围，mysql提供了两个选项：CASCADED和LOCAL，默认值CASCADED（级联）。

  * LOCAL：查看插入数据是否满足当前以及所依赖的有check option选项的视图。
  * CASCADED：查看插入数据是否满足当前以及所依赖的视图。

* 视图的更新：要使视图可更新，视图中的行与基础表中的行之间必须存在一对一的关系。

* 作用：

  * 使用简单：
  * 数据安全：
  * 数据独立：

##### 存储过程

相当于一个函数。

* 创建：

  ```mysql
  CREATE PROCEDURE 存储过程名称([参数列表])
  BEGIN
  	-- SQL语句
  END; -- 在命令行中使用delimiter设置结束符
  ```

* 调用：

  ```mysql
  CALL 名称([参数]);
  ```

* 查看：

  ```mysql
  SELECT * FROM INFORMATION_SCHEMA.ROUTINES WHERE ROUTINE_SCHEMA= 'xx'; --查询指定数据库的存储过程及状态信息
  SHOW CREATE PROCEDURE 存储过程名称; --查询某个存储过程的定义
  ```

* 删除：

  ```mysql
  DROP PROCEDURE [IF EXISTS] 存储过程名称;
  ```

* 变量：

  * 查看系统变量

    ```mysql
    SHOW [SESSION |GLOBAL ] VARIABLES; --查看所有系统变量
    SHOW [SESSION |GLOBAL] VARIABLES LIKE '...'; --可以通过LKE模糊匹配方式查找变量
    SELECT @@[SESSION.|GLOBAL.] 系统变量名; --查看指定变量的值
    ```

  * 设置系统变量

    ```mysql
    SET [SESSION | GLOBAL]系统变量名=值;
    SET @@[SESSION | GLOBAL]系统变量名=值;
    ```

  * 用户自定义变量：用户定义的变量无需对其进行声明或初始化，只不过获取到的值为NULL。

    * 赋值

      ```mysql
      SET @var_name = expr [ @var_ name = expr] ... ;
      SET @var_name := expr [ @var_ name := expr] ... ; -- 推荐使用
      SELECT @var_name := expr [ @var_ name := expr] .. ;
      SELECT 字段名 INTO @var_ name FROM 表名;
      ```

    * 使用

      ```mysql
      SELECT @var_name[,@var_name];
      ```

  * 局部变量：是根据需要定义的在局部生效的变量，访问之前，需要DECLARE声明。可用作存储过程内的局部变量和输入参数，局部变量的范围是在其内声明的BEGIN...END块。

    * 声明

      ```mysql
      DECLARE 变量名变量类型 [DEFAULT ...];
      ```

    * 赋值

      ```mysql
      SET 变量名 = 值;
      SET 变量名 := 值;
      SELECT 字段名 INTO 变量名 FROM 表名;
      ```

* if条件语法：

  ```mysql
  IF 条件1 THEN
  	.....
  ELSEIF 条件2 THEN -- 可选
  	.....
  ELSE --可选
  	.....
  END IF;
  ```

* 参数：

  ```mysql
  CREATE PROCEDURE 存储过程名称([IN/OUT/INOUT 参数名 参数类型])
  ```

* case：

  * 语法一：

    ```mysql
    CASE case_value
    	WHEN when_value1 THEN statement list1
    	[WHEN when_value2 THEN statement _list 2] ...
    	[ELSE statement _list]
    END CASE;
    ```

  * 语法二：

    ```mysql
    CASE
    	WHEN search_condition1 THEN statement_list1
    	[WHEN search_condition2 THEN statement_list2]
    	[ELSE statement_list]
    END CASE; 
    ```

* while：

  ```mysql
  # 先判定条件，如果条件为true, 则执行逻辑，否则，不执行逻辑
  WHILE 条件 DO
  	SQL逻辑..
  END WHILE;
  ```
  
* repeat：满足条件时退出循环。

  ```mysql
  #先执行一次逻辑，然后判定逻辑是否满足，如果满足，则退出。如果不满足，则继续下一次循环
  REPEAT
  	SQL逻辑...
  	UNTIL 条件
  END REPEAT;
  ```

* loop：LOOP实现简单的循环，如果不在SQL逻辑中增加退出循环的条件，可以用其来实现简单的死循环。LOOP可以配合一下两个语句使用：

  * LEAVE：配合循环使用，退出循环。
  * ITERATE：必须用在循环中，作用是跳过当前循环剩下的语句，直接进入下一次循环。

  ```mysql
  [begin_label:] LOOP
  	SQL逻辑...
  END LOOP [end_label];
  ```

* 游标：游标(CURSOR)是用来存储查询结果集的数据类型，在存储过程和函数中可以使用游标对结果集进行循环的处理。

  * 声明游标：

    ```mysql
    DECLARE 游标名称 CURSOR FOR 查询语句;
    ```

  * 打开游标：

    ```mysql
    OPEN 游标名称;
    ```

  * 获取游标记录：

    ```mysql
    FETCH 游标名称 INTO 变量[,变量];
    ```

  * 关闭游标：

    ```mysql
    CLOSE 游标名称;
    ```

* 条件处理程序：可以用来定义在流程控制结构执行过程中遇到问题时相应的处理步骤。

  ```mysql
  DECLARE handler_action HANDLER FOR condition_value [,condition_value] .. statement;
  
  handler_action
  	CONTINUE:继续执行当前程序
  	EXIT:终止执行当前程序
  condition_value
  	SQLSTATE sqlstate_ _value:状态码，如02000
  	SQLWARNING:所有以01开头的SQL STATE代码的简写
  	NOT FOUND:所有以02开头的SQLSTATE代码的简写
  	SQLEXCEPTION:所有没有被SQLWARNING或NOT FOUND捕获的SQLSTATE代码的简写
  ```

##### 存储函数

存储函数是有返回值的存储过程，存储函数的参数只能是IN类型的。具体语法如下:

```mysql
CREATE FUNCTION 存储函数名称([参数列表])
RETURNS type [characteristic ..]
BEGIN
	-- SQL语句
	RETURN ...
END;

characteristic说明:
	DETERMINISTIC:相同的输入参数总是产生相同的结果
	NO SQL:不包含SQL语句。
	READS SQL DATA:包含读取数据的语句，但不包含写入数据的语句。
```

##### 触发器

是与表有关的数据库对象，指在insert/update/delete之前或之后，触发并执行触发器中定义的SQL语句集合。触发器的这种特性可以协助应用在数据库端确保数据的完整性，日志记录，数据校验等操作。

| 触发器类型     | NEW和OLD                                             |
| -------------- | ---------------------------------------------------- |
| INSERT型触发器 | NWE表示将要或者已经新增的数据                        |
| UPDATE型触发器 | OLD表示修改之前的数据，NEW表示将要或意见修改后的数据 |
| DELETE型触发器 | OLD表示将要或者已经删除的数据                        |

* 创建：

  ```mysql
  CREATE TRIGGER trigger_name
  BEFORE/AFTER INSERT/UPDATE/DELETE
  ON tbl_name FOR EACH ROW -- 行级触发器
  BEGIN
  	trigger_stmt ;
  END;
  ```

* 查看：

  ```mysql
  SHOW TRIGGERS;
  ```

* 删除：

  ```mysql
  DROP TRIGGER [schema_name.]trigger_name; --如果没有指定schema name,默认为当前数据库。
  ```

#### 锁

* 全局锁：对整个数据库实例加锁，加锁后整个实例就处于只读状态，后续的DML的写语句，DDL语句，已经更新操作的事务提交语句都将被阻塞。其典型的使用场景是做全库的逻辑备份,对所有的表进行锁定，从而获取一致性视图，保证数据的完整性。

  <img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241102093205771.png" alt="image-20241102093205771" style="zoom:50%;" />

* 表级锁：表级锁，每次操作锁住整张表。锁定粒度大,发生锁冲突的概率最高，并发度最低。

  * 表锁：

    * 表共享读锁
    * 表独占写锁

    ```mysql
    加锁: lock tables 表名... read/write。
    释放锁: unlock tables /客户端断开连接。
    ```

  * 元数据锁：MDL加锁过程是系统自动控制，无需显式使用，在访问一张表的时候会自动加上。MDL锁主要作用是维护表元数据的数据一致性，防止DDL和DML语句的冲突。

    <img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241102100100210.png" alt="image-20241102100100210" style="zoom:45%;" />

  * 意向锁：为了避免DML在执行时，加的行锁与表锁的冲突,在InnoDB中引入了意向锁，使得表锁不用检查每行数据是否加锁,使用意向锁来减少表锁的检查。

* 行级锁：行级锁，每次操作锁住对应的行数据。锁定粒度最小，发生锁冲突的概率最低,并发度最高。应用在InnopB存储引擎中。

  * 行锁：如果不通过索引对数据进行操作，行锁升级为表锁。
    * 共享锁
    * 排他锁
  * 间隙锁/临键锁：间隙锁唯一目的是防止其他事务插入间隙。间隙锁可以共存，一个事务采用的间隙锁不会阻止另一个事务在同一间隙上采用间隙锁。

#### InnoDB引擎

##### 逻辑存储结构

<img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241102112558968.png" alt="image-20241102112558968" style="zoom:50%;" />

##### 架构

* 内存架构

  * Buffer Pool：缓冲池是主内存中的-一个区域，里面可以缓存磁盘上经常操作的真实数据,在执行增删改查操作时，先操作缓冲池中的数据(若缓冲池没有数据,则从磁盘加载并缓存)，然后再以一定频率刷新到磁盘，从而减少磁盘IO，加快处理速度。

    缓冲池以Page页为单位，底层采用链表数据结构管理Page。根据状态,将Page分为三种类型:
    ● free page:空闲page,未被使用。
    ● clean page:被使用page,数据没有被修改过。
    ● dirty page: 脏页,被使用page,数据被修改过,也中数据与磁盘的数据产生了不一致。

  * Change Buffer：更改缓冲区(针对于非唯一二级索引页) ，在执行DML语句时，如果这些数据Page
    没有在Buffer Pool中,不会直接操作磁盘，而会将数据变更存在更改缓冲区Change Buffer中,在未
    来数据被读取时，再将数据合并恢复到Buffer Pool中,再将合并后的数据刷新到磁盘中。

  * Adaptive Hash Index：自适应hash索引，用于优化对Buffer Pool数据的查询。InnoDB存储引擎会监控对表上各索引页的查询，如果观察到hash索引可以提升速度,则建立hash索引,称之为自适应hash索引。

  * Log Buffer:日志缓冲区，用来保存要写入到磁盘中的log日志数据(redo log、undo log)，默认大
    小为16MB，日志缓冲区的日志会定期刷新到磁盘中。如果需要更新、插入或删除许多行的事务,增加
    日志缓冲区的大小可以节省磁盘I/O。

* 磁盘结构

  * System Tablespace: 系统表空间是更改缓冲区的存储区域。如果表是在系统表空间而不是每个表文件或通用表空间中创建的，它也可能包含表和索引数据。(在MySQL5.x版本中还包含InnoDB数据字典、undolog等)
  * File-Per-Table Tablespaces:每个表的文件表空间包含单个InnoDB表的数据和索引，并存储在文件系统上的单个数据文件中。
  * General Tablespaces：通用表空间，需要通过CREATE TABLESPACE语法创建通用表空间，在创建表时，可以指定该表空间。
  * Undo Tablespaces:撤销表空间，MySQL实例在初始化时会自动创建两个默认的undo表空间(初始大小
    16M)， 用于存储undo log日志。
  * Temporary Tablespaces: InnoDB 使用会话临时表空间和全局临时表空间。存储用户创建的临时表等数据。
  * Doublewrite Buffer Files:双写缓冲区，innoDB引擎将数据页从Buffer Pool刷新到磁盘前，先将数据页写入双写缓冲区文件中，便于系统异常时恢复数据。
  * Redo Log:重做日志，是用来实现事务的持久性。该日志文件由两部分组成:重做日志缓冲(redo log buffer)以及重做日志文件(redo log) ,前者是在内存中，后者在磁盘中。当事务提交之后会把所有修改信息都会存到该日志中,用于在刷新脏页到磁盘时,发生错误时,进行数据恢复使用。

* 后台线程：将内存的数据写入到磁盘中。

  * Master Thread：核心后台线程，负责调度其他线程，还负责将缓冲池中的数据异步刷新到磁盘中,保持数据的一致性，还包括脏页的刷新、合并插入缓存、undo页的回收。

  * IO Thread：在InnoDB存储引擎中大量使用了AIO来处理IO请求,这样可以极大地提高数据库的性能，而IO Thread主要负责这些IO请求的回调。

    <img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241102115454258.png" alt="image-20241102115454258" style="zoom:50%;" />

  * Purge Thread：主要用于回收事务已经提交了的undo log,在事务提交之后，undo log可能不用了，就用它来回收。

  * Page Cleaner Thread：协助Master Thread刷新脏页到磁盘的线程，它可以减轻Master Thread的工作压力，减少阻塞。

##### 事务原理

* redo log（持久性）：重做日志，记录的是事务提交时数据页的物理修改，是用来实现事务的持久性。
  该日志文件（顺序写入，效率更高）由两部分组成:重做日志缓冲(redo log buffer)以及重做日志文件(redo log file) ,前者是在内存中，后者在磁盘中。当事务提交之后会把所有修改信息都存到该日志文件中,用于在刷新脏页到磁盘,发生错误时，进行数据恢复使用。
* undo log（原子性）：回滚日志，用于记录数据被修改前的信息,作用包含两个:提供回滚和MVCC(多版本并发控制)。

##### MVCC（多版本并发控制）

* 基本概念

  * 当前读：读取的是记录的最新版本，读取时还要保证其他并发事务不能修改当前记录,会对读取的记录进行加锁。
  * 快照读：简单的select (不加锁)就是快照读，快照读,读取的是记录数据的可见版本,有可能是历史数据，不加锁,是非阻塞读。

* 实现原理

  * 记录总的隐藏字段：

    <img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241102205123772.png" alt="image-20241102205123772" style="zoom:50%;" />

  * undo log版本链：不同事务或相同事务对同一条记录进行修改，会导致该记录的undolog生成-条记录版本链表, 链表的头部是最新的旧记录,链表尾部是最早的旧记录。

  * readview：ReadView (读视图)是快照读SQL执行时MVCC提取数据的依据，记录并维护系统当前活跃的事务(未提交的) id。

    <img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241102204827607.png" alt="image-20241102204827607" style="zoom:50%;" />

    <img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241102205208932.png" alt="image-20241102205208932" style="zoom:50%;" />

#### MySQL管理

##### 系统数据库

自带有四个数据库：

<img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241102211911590.png" alt="image-20241102211911590" style="zoom:50%;" />

##### 常用工具

* mysql：

  <img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241102212000768.png" alt="image-20241102212000768" style="zoom:50%;" />

* mysqladmin：是一个执行管理操作的客户端程序。可以用它来检查服务器的配置和当前状态、创建并删除数据库等。
* mysqlbinlog：由于服务器生成的二进制日志文件以二进制格式保存，所以如果想要检查这些文本的文本格式，就会使用到mysqlbinlog日志管理工具。

* mysqlshow：mysqlshow客户端对象查找工具，用来很快地查找存在哪些数据库、数据库中的表、表中的列或者索引。

  ```mysql
  mysqlshow [options] [db_name [table_name [col name]]]
  -count 显示数据库及表的统计信息(数据库，表均可以不指定)
  -i 显示指定数据库或者指定表的状态信息
  ```

* mysqldump：mysqldump客户端工具用来备份数据库或在不同数据库之间进行数据迁移。备份内容包含创建表，及插入表的SQL语句。

  ```shell
  mysqldump [options] db_ name [tables]
  mysqldump [options] --database/-B db1 [db2 d3...]j.
  mysqldump [options] --all-databases/-A
  
  输出选项:
  	--add-drop-database 在每个数据库创建语句前加.上drop database语句
  	--add-drop-table 在每个表创建语句前加上drop table语句,默认开启;不开启(--skip-add- -drop -table)
  	-n, --no-create-db 不包含数据库的创建语句
  	-t, --no-create -info 不包含数据表的创建语句
  	-d --nq-data 不包含数据
  	-T, --tab=name 自动生成两个文件:一个.sql文件，创建表结构的语句; - -个.txt文件，数据文件
  ```

* mysqlimport：mysqlimport是客户端数据导入工具，用来导)l:mysqldump加-T参数后导出的文本文件。

  ```shell
  mysqlimport [options] db_name textfile1 [textfile1 ...]
  或者
  source /rotx/xxxx.sql -- 在mysql中命令行导入sql文件
  ```

  
