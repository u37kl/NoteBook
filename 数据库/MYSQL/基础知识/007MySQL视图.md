![](./media/15855489877537.jpg)

[TOC]

## 视图介绍和视图的作用
视图：数据库中一个虚拟表，只有视图定义没有实际的数据，视图中的数据需要通过每次查询基本表获得，可以理解为保存在数据库中待执行的SQL语句。

作用：
1. 因为视图的数据是从基本表获取的，因此可以为不同的用户建立不同的视图，限制他们查看的数据，让用户只能查看它们权限内的数据，无法看到数据库的基本表，保护了数据库核心数据。
2. 实现数据逻辑的独立性，应用程序所需要的数据是基于数据库的，如果对数据库表结构进行修改，必然导致应用程序的修改，如果应用程序基于视图，基本表改变时，只要修改视图，让其符合应用程序需求就行。

缺点：
1. 视图是建立在基本表上的，因此基于视图的查询的性能无法估计，毕竟视图的消解是数据库自身完成的。
2. 基本表改变，则视图也需要跟着做相应的调整。

## 视图创建、更新与删除
```sql
# 模版
-- 创建视图或者替换重名视图
CREATE [OR REPLACE] 
      [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}]
      [definer = 用户名]
    VIEW view_name [(column_list)]
    AS select_statement
   [WITH [CASCADED | LOCAL] CHECK OPTION]
   

-- 更新视图
ALTER
    [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}]
    [DEFINER = { user | CURRENT_USER }]
    [SQL SECURITY { DEFINER | INVOKER }]
VIEW view_name [(column_list)]
AS select_statement
    [WITH [CASCADED | LOCAL] CHECK OPTION]

-- 删除视图
DROP VIEW [IF EXISTS]   
view_name [, view_name] ... [Cascaded]

# 案例
-- 创建简单视图, select语句的目标列可以添加属性名、常量、表达式以及函数
create or replace view StuView1 as select * from Student;
create or replace view StuView2 as select stu_id, stu_name, '学生', 2019-Student.stu_age from Student where stu_sex='男';
create or replace view StuView3 as select stu_dept, avg(stu_age) from Student GROUP BY stu_dept;

-- 创建多表连接的视图
create or replace view StuView4 as select stu_id, stu_name,stu_sex,stu_age,stu_dept,s_id,SC.c_id as c_id,grade,c_name,c_credit from Student, SC, Cource where stu_id=s_id and SC.c_id=Cource.c_id;

-- 创建从视图中导出的视图
create or replace view StuView5 as select stu_id, stu_name,stu_sex,stu_age,stu_dept,c_id, c_name, grade, c_credit from StuView4;

-- 创建可以更新数据的视图(单表视图、多表视图、从视图中导出的视图)
create or replace view UpdateStuView2 as select stu_id, stu_name, '学生'as identity, 2019-Student.stu_age as birthday from Student order by stu_id with check option;

create or replace view UpdateStuView5 as select stu_id, stu_name,stu_sex,stu_age,stu_dept,s_id,SC.c_id as c_id,grade,c_name,c_credit from Student, SC, Cource where stu_id=s_id and SC.c_id=Cource.c_id with check option;

create or replace view UpdateStuView6 as select stu_id, stu_name,stu_sex,stu_age,stu_dept,c_id, c_name, grade, c_credit from UpdateStuView5 with check option;


```
>创建视图的细节说明：
>1. 视图列名称只有两种写法，或者全部省略使用select中目标列，或者全部指定。
>2. 视图语句中的select语句目标列也可以使用基本表列名、常量、函数、表达式。
>3. or replace表示，如果存在该视图，则替换成新的视图定义，尝尝用这个关键字进行对视图的定义语句进行更新，不用alter。
>4. with check option参数用来规定，对视图数据的更新需要遵循基本表和定义视图时的select语句where条件。
>5. local与cascaded区别在于对嵌套视图的操作，前置只会检查当前视图，后者不仅检查当前视图，还会检查导出该视图的视图，一直到底层视图。
>6. with check option使用是有限制的，下面是不能使用的情况:
> 1). select语句中出现distinct时。
> 2). select语句中出现group by子句时。
>7. algorithm参数表示，在进行视图消解时，是通过将创建视图的select语句和用户编写的语句合并形成新的SQL语句，然后对基本表进行更新，还是先将视图变成一个临时表，然后在表基础上运行用户编写的sql语句；temptable表示生成临时表模式。
>8. definer参数表示，该视图的创建者，不指定就是当前用户。

视图删除语句中使用cascaded表示，不仅删除当前视图，而且删除由该视图导出的所有视图。


## 视图内容的更新限制
数据库支持通过视图来更新基本表的数据，但是视图更新数据还是有限制的。
>下列视图无法进行更新的情况：
>1. 通过视图更新基本表需要遵循基本表的所有完整性约束，例如从单表中生成的视图，没有包含该表的非空列，则无法通过视图添加新数据。
>2. 通过视图更新基本表数据时，只能添加或者修改视图中出现的列，对于没有出现的系统自动设置为null，删除对应主键的数据。
>2. 从相关子查询中生成的视图，无法进行数据的更新，有的视图可以进行更新，还是都默认为不能更新。
>3. 多个表连接导出的视图，无法更新多表数据，只能更新单表数据，例如修改属性时，只能修改一个表的属性，无法添加。
>4. 对于导出的视图没有包含基本表的主键时，无法进行更新。
>5. 对于使用并集生成的视图，无法更新，因为添加和修改时无法确定应该修改哪个表，例如学生表和学生课程关系表进行并集时，并集列为student(stu_id, stu_age)和SC(s_id, grade)，如果修改并集后的第二列数据时，那最终应该修改哪个基本表。

不建议对视图进行增、删和改操作，因为它是一个虚表，没有完整性约束，有时可能连个主键都没有，有时多个表连接无法确定更新哪个基本表。

## 视图消解
视图是一个虚拟表，只有定义没有实际的数据，因此在视图上进行数据操作时，数据库提供了三种操作，SQL语句合并、生成中间表和undefinded，默认不设置使用undefinded，表示让数据库自动选择SQL语句合并还是生成中间表。

merge(SQL语句合并): 数据库会将用户输入的SQL语句与视图定义中的select语句进行合并，生成一个新SQL然后运行在基本表中。
```sql
-- 视图定义
create view StudentView as select * from stu_dept='CS';

-- 用户SQL
select * from StudentView where stu_age > 19;

-- 视图SQL与用户SQL合并
select * from Student where stu_dept='CS' and stu_age > 19;

```
temptable(临时表): 数据库会将运行视图定义语句生成一个临时表，然后用户SQL语句运行在这个临时表上，就像子查询一样。


