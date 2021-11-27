### while

````mysql
delimiter //                            #定义标识符为双斜杠
drop procedure if exists test;          #如果存在test存储过程则删除
create procedure test()                 #创建无参存储过程,名称为test
begin
    declare i int;                      #申明变量
    set i = 0;                          #变量赋值
    WHILE i < 10 DO                     #结束循环的条件: 当i大于10时跳出while循环
        insert into test values (i);    #往test表添加数据
        set i = i + 1;                  #循环一次,i加一
    END WHILE;                          #结束while循环
    select * from test;                 #查看test表数据
end
//                                      #结束定义语句
call test();                            #调用存储过程
````



### repeat

````mysql
delimiter //                            #定义标识符为双斜杠
drop procedure if exists test;          #如果存在test存储过程则删除
create procedure test()                 #创建无参存储过程,名称为test
begin
    declare i int;                      #申明变量
    set i = 0;                          #变量赋值
    REPEAT
        insert into test values (i);    #往test表添加数据
        set i = i + 1;                  #循环一次,i加一
    UNTIL i > 10 END REPEAT;            #结束循环的条件: 当i大于10时跳出repeat循环
    select * from test;                 #查看test表数据
end
//                                      #结束定义语句
call test();                            #调用存储过程
````



### loop

```mysql
delimiter //                            #定义标识符为双斜杠
drop procedure if exists test;          #如果存在test存储过程则删除
create procedure test()                 #创建无参存储过程,名称为test
begin
    declare i int;                      #申明变量
    set i = 0;                          #变量赋值
    lp : LOOP                           #lp为循环体名,可随意 loop为关键字
        insert into test values (i);    #往test表添加数据
        set i = i + 1;                  #循环一次,i加一
        if i > 10 then                  #结束循环的条件: 当i大于10时跳出loop循环
            LEAVE lp;
        end if;
    END LOOP;
    select * from test;                 #查看test表数据
end
//                                      #结束定义语句
call test(); 
```



感谢：https://blog.csdn.net/qq_19865749/article/details/101004935

