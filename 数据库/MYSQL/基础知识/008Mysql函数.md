

[TOC]

# 函数介绍

函数是数据库提供的扩展SQL功能的方式，分为内置函数和自定义函数，最常用的内置函数为聚合函数。

# 创建、调用和删除自定义函数

## 创建函数

```mysql
# 创建函数的两种方式
-- 方式一：函数体由一个表达式组成
CREATE FUNCTION 函数名(参数 类型,[参数 类型,...])
RETURNS 返回类型 RETURN 表达式值

-- 方式二：使用begin-end
CREATE FUNCTION 函数(参数 类型,[参数 类型,...])
RETURNS 返回类型
BEGIN
END;

# 案例
-- 函数体为表达式
create function SchoolDataBase.sum_text12(a TINYINT, b TINYINT) returns TINYINT return a+b;

-- begin-end
delimiter $$
create function sum_test(a char(5), b TINYINT) returns TINYINT
BEGIN
  declare temp TINYINT;
  select count(1) into temp from SchoolDataBase.Student where stu_name like a and stu_age >= b;
  return temp;
END $$
delimiter ;

# 调用，自定义函数与内置函数使用方法一致，在select语句中使用，不能直接调用会报错。
select sum_text12(1,22);
select sum_test('李%',10);

```

> 第一种方式创建自定义函数时，函数体中只能是表达式，并能使用SQL语句。







https://www.cnblogs.com/kerrycode/p/7641835.html

ERROR 1418 (HY000): This function has none of DETERMINISTIC, NO SQL, or READS SQL DATA in its declaration and binary logging is enabled (you *might* want to use the less safe log_bin_trust_function_creators variable)

```
set global log_bin_trust_function_creators=1;

show variables like 'log_bin';

show variables like 'log_bin_trust_function_creators';
```

# 函数和存储过程区别

# 常用的内置函数

作用范围，数据库、表？

直接使用函数名()就可以调用【虽然这么说，但返回的是一个结果，sql中不使用select的话任何结果都无法显示出来（所以单纯调用会报错），】

查看函数创建语句：show create function 函数名;
查看所有函数：show function status [like 'pattern'];

函数的修改只能修改一些如comment的选项，不能修改内部的sql语句和参数列表。
alter function 函数名 选项；

drop function 函数名;

```
内置函数和自定义函数

三、函数与存储过程的区别
存储过程可以有多个in,out,inout参数，而函数只有输入参数类型，而且不能带in.
存储过程实现的功能要复杂一些；而函数的单一功能性（针对性）更强。
存储过程可以返回多个值；存储函数只能有一个返回值。
存储过程一般独立的来执行；而存储函数可以作为其它sql语句的组成部分来出现。
存储过程可以调用存储函数。函数不能调用存储过程。

作者：唯老
链接：https://www.jianshu.com/p/ffa593181ead
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

两种格式
CREATE FUNCTION 函数名(参数 类型,[参数 类型,...])
RETURNS 返回类型 RETURN 表达式值
-- 注意
-- 这种方式不能使用任何SQL语句

CREATE FUNCTION 函数(参数 类型,[参数 类型,...])
RETURNS 返回类型
BEGIN
END;
-- 如果要在函数体中可以使用更为复杂的语法，比如复合结构/流程控制/任何SQL语句/定义变量等。带复合结构的函数体的自定义函数的

自定义函数必须有返回值？
```


```sql
5. 随机生成三位名字
​```mysql
DROP FUNCTION IF EXISTS create_name;
CREATE FUNCTION create_name()
    RETURNS varchar(3)
BEGIN
    DECLARE ln VARCHAR(300);
    DECLARE mn VARCHAR(500);
    DECLARE fn VARCHAR(500);
    DECLARE ln_n INT;
    DECLARE mn_n INT;
    DECLARE fn_n INT;
    SET ln = '郑罗宋谢唐韩曹许邓萧冯曾程蔡彭潘袁李王张刘陈杨黄赵周吴徐孙朱马胡郭林何高梁于董余苏叶吕魏蒋田杜丁沈姜范江傅钟卢汪戴崔任陆廖姚方金邱夏谭韦贾邹石熊孟秦阎薛侯雷白龙段郝孔邵史毛常万顾赖武康贺严尹钱施牛洪龚';
    SET mn =
            '庆磊民友玉萍红娥玲芬芳燕彩兰凤洁梅伟刚勇毅俊云莲真环雪荣爱妹霞香月莺媛艳瑞凡佳嘉琼勤珍贞莉桂娣叶璧才发武丽琳轮翰朗伯宏言若鸣朋斌梁栋维启克伦翔旭鹏泽晨辰士以建家致树炎德河哲江超浩璐娅琦晶裕华慧巧美婕馨影荔枝思心邦承乐绍功松善厚庆磊民友玉萍红娥玲芬芳燕彩兰凤洁梅秀娟英行时泰盛雄琛钧冠策腾楠榕风航弘峰强军平保东文辉力明永健世广志义兴良海山仁波宁贵福生龙元全国胜学祥才发武新利清飞彬富顺信子杰涛昌成康星光天达安岩中茂进林有坚和彪博诚先敬震振壮会思群豪心邦承乐绍功松善厚庆磊民友裕河哲江超浩亮政谦亨奇固之轮翰朗伯宏言若鸣朋斌梁栋维启克伦翔旭鹏泽晨辰士以建家致树炎德行时泰盛雄琛钧冠策腾楠榕风航弘';
    SET fn =
            '莉桂娣叶璧才发武丽琳轮翰朗伯宏言若鸣朋斌梁栋维启克伦翔旭鹏泽晨辰士以建家致树炎德河哲江超浩璐娅琦晶裕华慧巧美婕馨影荔枝思心邦承乐绍功松善厚秀娟伟刚勇毅俊云莲真环雪荣爱妹霞香月莺媛艳瑞凡佳嘉琼勤珍贞英行时泰盛雄琛钧冠策腾楠榕风航弘峰强军平保东文辉力明永健世广志义兴良海山仁波宁贵福生龙元全国胜学祥才发武新利清飞彬富顺信子杰涛昌成康星光天达安岩中茂进林有坚和彪博诚先敬震振壮会思群豪心邦承乐绍功松善厚庆磊民友裕河哲江超浩亮政谦亨奇固之轮翰朗伯宏言若鸣朋斌梁栋维启克伦翔旭鹏泽晨辰士以建家致树炎德行时泰盛雄琛钧冠策腾楠榕风航弘';
    SET ln_n = CHAR_LENGTH(ln);
    SET mn_n = CHAR_LENGTH(mn);
    SET fn_n = CHAR_LENGTH(fn);
 RETURN CONCAT(substring(ln, ceil(rand() * ln_n), 1), substring(MN, ceil(rand() * mn_n), 1),
                  substring(fn, ceil(rand() * fn_n), 1));
END
-- 调用函数
SELECT create_name()
```

```sql
# 1. 如果函数存在则先删除函数
DROP FUNCTION IF EXISTS rand_str;
--  2. 创建函数
CREATE FUNCTION rand_str(str_len SMALLINT) RETURNS VARCHAR(255)
BEGIN -- 相当于{'
    -- 3. 定义一个函数,名称'rand_str' ,参数名 str_len 参数类型  smallint ,
    -- 返回值类型 varchar(255) ，特别 注意下 这里的 是returns 下面的是 return
    DECLARE rand_str VARCHAR(255) DEFAULT 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890';
    --  4. 定义循环条件
    DECLARE i SMALLINT DEFAULT 0;
    -- 5. 定义 返回的的变量
    DECLARE result_str VARCHAR(255) DEFAULT '';
    -- 6. 循环
    WHILE i < str_len DO
    -- 7. 内置函数说明
    -- 7.1 rand() : 产生 0-1之间的小数
    -- 7.2 rand() :产生 0-1之间的小数,简称种子.rand()*10 产生的数 在 0- 10 之间,不包括10
    -- 7.3 substring(str,pos,len): 截取字符串，
    -- 第一个参数:待截取的字符串，第二个参数:开始的位置(这里有些不同，下标开始位置为1，可以试试下),第三个参数:截取的长度.
    -- 7.4 floor(val): 生成最近接val的最大整数
    -- 7.5 CONCAT 生成连接的字符串
        SET result_str = CONCAT(SUBSTR(rand_str, FLOOR(RAND() * LENGTH(rand_str)) + 1, 1), result_str);
        SET i = i + 1;
    END WHILE; -- 8.结束语句
    -- 9. 返回 结果对象   这里的是  return
    RETURN result_str;
END -- 相当于'}'
```

```sql
CREATE FUNCTION my_date_format(p_date date) RETURNS CHAR(10)
 BEGIN
   IF p_date is null THEN
    RETURN NULL;
   END IF;
    RETURN date_format(now(), '%Y-%m-%d');
 END;
```

```sql
CREATE FUNCTION uuid_32() RETURNS CHAR(32)
BEGIN
 RETURN replace(uuid(), '-', '');
END;

select uuid_32( );
```
