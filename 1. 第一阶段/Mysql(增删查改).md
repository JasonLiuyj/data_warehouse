#### 第四章：检索数据

* 检索指定表所有列：SELECT * FROM table_name
* 检索指定列的唯一值：SELECT DISTINCT column FROM table_name
* 检索指定行数的数据：…… LIMIT 5, 5或者 …… LIMIT 5 OFFSET 5（推荐）;

#### 第五章：排序检索数据

* 检索的数据进行排序
  * 升序：SELECT * FROM table_name ORDER BY ……
  * 降序：SELECT * FROM table_name ORDER BY …… DESC（其中DESC可以插入到指定列后面，实现先降序再升序的效果。**如果想在多个列上进行降序排序，必须对每个列指定DESC关键字**）

#### 第六章：过滤数据

* 对指定数据进行过滤：

  * WHERE指定具体值：SELCET * FROM table_name WHERE column = …… ORDER BY ……(注意ORDER BY只能在WHERE的后面)

  * BETWEEN …… AND指定范围：SELCET * FROM table_name BETWEEN …… AND ……
  * 空值检查IS NULL：SELCET * FROM table_name WHERE column IS NULL

#### 第七章：数据过滤

* AND操作符：使用多个过滤条件进行约束。
* OR操作符：满足一个过滤条件即可。
* 计算次序：在Mysql中AND的优先级大于OR，我们可以使用圆括号来满足我们的需求。
* IN操作符：当只是对一个列进行多个数值过滤时可以使用IN操作符。例：SLECT * FROM table_name WHERE  column IN (1,2,3)
* NOT操作符：MySQL支持使用NOT对IN、BETWEEN和EXISTS子句取反。

#### 第八章：用通配符进行过滤

* %：表示任何字符出现任意次数。
* _：匹配单个字符。

#### 第十一章：使用数据处理函数

* 常见的文本处理函数：
  * Locate()：找出串的一个子串。
  * Soundex()：找到相似发音的字符
  * SubString()：返回子串的字符
* 日期和时间处理函数：
  * 

* 数值处理函数：主要包括一些代数运算。

#### 第十二章：汇总数据

* 聚集函数：
  * AVG()
  * COUNT()
  * MAX()
  * MIN()
  * SUM()

* 可以在一个语句中执行多个聚集函数。

#### 第十三章：分组数据

* GROUP BY：对指定列进行分组后计算。
* 过滤分组：针对已经分组过的数据不能再使用WHERE，而应该使用HAVING。

#### 第十四章：使用子查询

* 子查询：通过子查询将几张不同的表结合起来，使用一个子查询得到的结果作为外查询的条件。

#### 第十五章：联结表（join）

* 联结多个表取出指定列：SELECT …… FROM table_A , table_B WHERE table_a.columm = talbe_b.column（等值联结）或者SELECT …… FROM table_A INNER JOIN table_B ON table_a.columm = talbe_b.column（内部联结）
* 如果不适用WHERE语句，上述代码将返回table_A和table_B的笛卡尔积，即两个表每一行都进行匹配。

#### 第十六章：创建高级联结

<img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241025113510812.png" alt="image-20241025113510812" style="zoom:40%;" />

* 自联结：使用同一个表多次，分别其不同的名字。

  <img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20241029185150229.png" alt="image-20241029185150229" style="zoom:33%;" />

* 自然联结：两个表中具有相同的列并且返回唯一的数据。

* 外联结：外联结会包含左(右)表中不匹配的数据，默认为NULL。

#### 第十七章：组合查询

* 综合多个查询的结果：将UNION（删除重复的行）或UNION ALL（不删除）放在每一个SELECT语句中间。

#### 第十八章：全文本搜索

用于在大量文本中找出包含某个词语。

#### 第十九章：插入数据	

* 向表中添加元素：INSERT INTO table(……)  VALUES(……) （……）
* 向表中插入查询的数据：INSERT INTO table_a(……)  SELECT …… FROM table_b

#### 第二十章：更新和删除数据

* 更新数据：UPDATE table SET column = "……" WHERE column = "……"

* 删除数据：DELETE FROM table WHERE column = ……

#### 第二十一章：创建和操纵表

* 创建表：CREATE TABLE table()
* 修改表：ALTER TABLE table ADD/DROP
* 删除表：DROP TABLE table
* 重命名表：RENAME TABLE table_a TO table_b
