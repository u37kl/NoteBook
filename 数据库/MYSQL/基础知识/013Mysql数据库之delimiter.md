# delimiter介绍

用来定义结束符，默认sql语句以“ **分号 **” 作为结束符，mysql客户端在接收用户输入的sql语句时，当遇到分号时，客户端认为用户输入完成，直接执行。但是在定义一些复杂的操作语句时(例如存储过程)，其中包含多个sql语句，需要将客户端将其当作整体一起接收，然后再让DBMS执行。

使用delimiter定义sql语句结束符。
```sql
-- 表示sql结束符为$$
delimiter $$ 
select * from Student $$

-- 后面不要跟分号，mysql会认为“ $$; ”为结束符，而不是“ $$ ”。
delimiter $$;
select * from Student $$;

```

最后记得将结束符修改回分号。