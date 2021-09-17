###Rank( ) over( )

The syntax of the `RANK()` function is as follows

```sql
RANK() OVER (
	PARTITION BY <expr1>[{,<expr2>...}]
	ORDER BY <expr1> [ASC|DESC], [{,<expr2>...}]
)
```

In this syntax:

- First, the `PARTITION BY` clause distributes the rows in the result set into partitions by one or more criteria.
- Second, the `ORDER BY` clause sorts the rows in each a partition.
- The `RANK()` function is operated on the rows of each partition and re-initialized when crossing each partition boundary.



### 分区表索引相关问题

https://www.modb.pro/db/21860

分区表建议统一使用本地索引来进行操作

```sql
alter table part_table truncate partition p1 update global indexes;	
```

以上操作会导致数据库dml操作进入阻塞blokc状态



####1. truncate分区

SQL操作命令：

```sql
alter table part_table truncate partition p1;	
```

- 全局索引：失效
- 分区索引：正常、没影响

如何避免失效：

```sql
alter table part_table truncate partition p1 update global indexes;	
```

####2. drop分区

SQL操作命令：

```sql
alter table part_table drop partition p1;	
```

- 全局索引：失效
- 分区索引：正常、没影响

#### 如何避免失效：

```sql
alter table part_table drop partition p1 update global indexes;	 
```

####3. add分区

SQL操作命令：

```sql
alter table part_table add partition p5 values less than(37210);	 	
```

- 全局索引：正常、没影响
- 分区索引：正常、没影响

####4. split分区

SQL操作命令：

```sql
alter table part_table split partition p_max at(10086)  into (partition p6,partition p_max); 	 	
```

- 全局索引：失效
- 分区索引：如果max区中已经有记录了，这个时候split就会导致有记录的新增分区的局部索引失效。

#### 如何避免失效：

- 针对全局索引：

```sql
alter table part_table split partition p_max at (10086) into (partition p6,partition p_max) update global indexes;	
```

- 针对分区索引，需要重建局部索引：

```sql
alter index idx_part_split_col1 rebuild; 
	 
```

####5. exchange分区

#### SQL操作命令：

```sql
alter table part_table exchange partition p1 with table normal_table including indexes;	 	
```

- 全局索引：失效
- 分区索引：正常、没影响

#### 如何避免失效：

```sql
alter table part_table exchange partition p1 with table normal_table including indexes update global indexes;	 	 
```



### Oracle 删除重复数据

https://www.php.cn/oracle/467906.html

删除全部重复数据

```sql
delete from nayi224_180824 t where t.rowid in (select rid 
from (select t1.rowid rid,
row_number() over(partition by t1.col_2, t1.col_3 order by 1) rn 
from nayi224_180824 t1) t1 
where t1.rn > 1);
```



删除除最新的一条以外全部重复数据

```sql
delete from nayi224_180824 t where t.rowid not in
(select max(rowid) from nayi224_180824 t1 group by t1.col_2, t1.col_3);
```





### spool用法

官方文档：https://docs.oracle.com/cd/E11882_01/server.112/e16604/ch_twelve043.htm#SQPUG126

Syntax

SPO[OL] [*file_name*[.*ext*] [CRE[ATE] | REP[LACE] | APP[END]] | OFF | OUT]

Stores query results in a file, or optionally sends the file to a printer.

```sql
SET SERVEROUTPUT ON FORMAT WRAPPED
SET VERIFY OFF

SET FEEDBACK OFF
SET TERMOUT OFF

column date_column new_value today_var
select to_char(sysdate, 'yyyymmdd') date_column
  from dual
/
DBMS_OUTPUT.ENABLE(1000000);

SPOOL C:\output_&today_var..txt

DECLARE
   ab varchar2(10) := 'Raj';
   cd varchar2(10);
   a  number := 10;
   c  number;
   d  number; 
BEGIN
   c := a+10;
   --
   SELECT ab, c 
     INTO cd, d 
     FROM dual;
   --
   DBMS_OUTPUT.put_line('cd: '||cd);
   DBMS_OUTPUT.put_line('d: '||d);
END; 

SPOOL OFF

SET TERMOUT ON
SET FEEDBACK ON
SET VERIFY ON

PROMPT
PROMPT Done, please see file C:\output_&today_var..txt
PROMPT
```

