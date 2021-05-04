![](./media/15855492064558.jpg)

[TOC]

## 数据的添加
#### 添加语句格式
```sql

# 格式
-- 插入一条数据
insert into 表名[(列名1,列名2,列名3...)] value(常量1,[常量2,常量3,]);

-- 插入多条数据
 insert into 表名[(列名1,列名2,列名3...)] values
                                   (常量1,[常量2,常量3,]),
                                   (常量1,[常量2,常量3,]),
                                   (常量1,[常量2,常量3,])
                                   ...;
                                   
# 案例
-- 没有设置列名
insert into SchoolDataBase.Student value('200215121','李勇','男',20,'CS');

-- 设置列名
insert into SchoolDataBase.Student(stu_name, stu_sex, stu_dept, stu_id) value('李勇','男','CS','200215121', null);

-- 插入多条数据
insert into SC values('200215121','1',92),
										 ('200215121','2',88),
										 ('200215121','3',88),
										 ('200215122','2',90),
										 ('200215122','3',80),
										 ('200215123','3',88);
```
>注意问题：
>1. 表名后面跟不跟列名区别在于插入数据时<u>是否设置所有属性</u>，并且不跟列名，则属性值需要按照<u>表定义的属性顺序填写</u>，而表名后跟列名，填写的属性值顺序按照<u>表名后列名顺序填写</u>。
>2. 表名后没有列举出的列名的值，自动设置为null。
>3. 插入数据时需要满足数据表的完整性约束，非空、取值范围、外键(设置外键值时，需要先设置主表的主键值)等问题。

#### 插入子查询结果
```sql
# 格式
insert into 表名[(列名1,列名2,列名3...)] 子查询;

#案例
-- 统计各个专业学生的平均年龄，两个表都是记录平均年龄，只是一个使用整数、一个使用小数表示平均值，整数会对小数部分四舍五入。
insert into DeptAgeInt select stu_dept, avg(stu_age) from Student group by stu_dept;

insert into DeptAgeDouble select stu_dept, avg(stu_age) from Student group by stu_dept;
```

## 数据更新
#### 更新语句格式
```sql
# 格式
update 表名 set 列名=表达式[, 列名=表达式, 列名=表达式] [where 子句]。

# 案例
-- 更新学生年龄，赋值方式常量
update Student set Student.stu_age=10 where Student.stu_id='200215126';

-- 更新学生年龄，赋值方式表达式
update Student set Student.stu_age=2019-Student.stu_age where Student.stu_id='200215126';

-- 修改元组的多个属性，并且对同一个属性多次修改。
update Student set Student.stu_age=10, Student.stu_age=2019-Student.stu_age, Student.stu_sex='女' where Student.stu_id='200215126';
```
#### 带子查询的修改
```sql
# 格式
update set 表名 列名=表达式[, 列名=表达式, 列名=表达式] 子查询;

#案例
-- 将学生表中所有为男性的学生年龄改为19岁
update Student set Student.stu_age=19 where Student.stu_id in (select stu_id from Student where Student.stu_sex='男');
```

## 数据的删除
#### 删除语句格式
```sql
# 格式
delete from 表名 [where 子句];

# 案例
-- 清空学生表中所有记录
delete from Student;

-- 删除表中专业为‘CS’的学生信息。
delete from Student where stu_dept=‘CS’;
```
>注意：删除时需要遵循表规定的完整性约束进行删除，否则删除失败。例如删除外键对应的主键所在的元组会失败，因为违反参照完整性约束。

#### 带子查询的删除
```sql
# 格式
delete from 表名 子查询;

#案例
-- 删除计算机专业学生的所有课程成绩，使用不相关子查询实现
delete from SC where s_id in (select stu_id from Student where stu_dept='CS');

-- 使用相关子查询实现
delete from SC where 'CS'=(select stu_dept from Student where SC.s_id=Student.stu_id);
```