# Oracle实战 语句

19c官网：https://docs.oracle.com/en/database/oracle/oracle-database/19/admin/index.html



## oracle Regular Language

## Syntax

The syntax for the REGEXP_LIKE condition in Oracle/PLSQL is:

```
REGEXP_LIKE ( expression, pattern [, match_parameter ] )
```

### Parameters or Arguments

- expression

  A character expression such as a column or field. It can be a VARCHAR2, CHAR, NVARCHAR2, NCHAR, CLOB or NCLOB data type.

- pattern

  The regular expression matching information. It can be a combination of the following:

  | Value  | Description                                                  |
  | :----- | :----------------------------------------------------------- |
  | ^      | Matches the beginning of a string. If used with a *match_parameter* of 'm', it matches the start of a line anywhere within *expression*. |
  | $      | Matches the end of a string. If used with a *match_parameter* of 'm', it matches the end of a line anywhere within *expression*. |
  | *      | Matches zero or more occurrences.                            |
  | +      | Matches one or more occurrences.                             |
  | ?      | Matches zero or one occurrence.                              |
  | .      | Matches any character except NULL.                           |
  | \|     | Used like an "OR" to specify more than one alternative.      |
  | [ ]    | Used to specify a matching list where you are trying to match any one of the characters in the list. |
  | [^ ]   | Used to specify a nonmatching list where you are trying to match any character except for the ones in the list. |
  | ( )    | Used to group expressions as a subexpression.                |
  | {m}    | Matches m times.                                             |
  | {m,}   | Matches at least m times.                                    |
  | {m,n}  | Matches at least m times, but no more than n times.          |
  | \n     | n is a number between 1 and 9. Matches the nth subexpression found within ( ) before encountering \n. |
  | [..]   | Matches one collation element that can be more than one character. |
  | [::]   | Matches character classes.                                   |
  | [==]   | Matches equivalence classes.                                 |
  | \d     | Matches a digit character.                                   |
  | \D     | Matches a nondigit character.                                |
  | \w     | Matches a word character.                                    |
  | \W     | Matches a nonword character.                                 |
  | \s     | Matches a whitespace character.                              |
  | \S     | matches a non-whitespace character.                          |
  | \A     | Matches the beginning of a string or matches at the end of a string before a newline character. |
  | \Z     | Matches at the end of a string.                              |
  | *?     | Matches the preceding pattern zero or more occurrences.      |
  | +?     | Matches the preceding pattern one or more occurrences.       |
  | ??     | Matches the preceding pattern zero or one occurrence.        |
  | {n}?   | Matches the preceding pattern n times.                       |
  | {n,}?  | Matches the preceding pattern at least n times.              |
  | {n,m}? | Matches the preceding pattern at least n times, but not more than m times. |

- match_parameter

  Optional. It allows you to modify the matching behavior for the REGEXP_LIKE condition. It can be a combination of the following:

  | Value | Description                                                  |
  | ----- | ------------------------------------------------------------ |
  | 'c'   | Perform case-sensitive matching.                             |
  | 'i'   | Perform case-insensitive matching.                           |
  | 'n'   | Allows the period character (.) to match the newline character. By default, the period is a wildcard. |
  | 'm'   | *expression* is assumed to have multiple lines, where ^ is the start of a line and $ is the end of a line, regardless of the position of those characters in *expression*. By default, *expression* is assumed to be a single line. |
  | 'x'   | Whitespace characters are ignored. By default, whitespace characters are matched like any other character. |

  

## Note

- The REGEXP_LIKE condition uses the input character set to evaluate strings.
- If you specify *match_parameter* values that conflict, the REGEXP_LIKE condition will use the last value to break the conflict.
- If the *match_parameter* is omitted, the REGEXP_LIKE condition will use the case-sensitivity as determined by the NLS_SORT parameter.
- See also the Oracle [LIKE condition](https://www.techonthenet.com/oracle/like.php).



## instr和like的模糊查询性能比较

使用instr函数取代like查询，可提高效率，在海量数据中效果尤其明显。

参考：http://m.111com.net/art-112284.htm｜https://developer.aliyun.com/article/43639



url : https://www.techonthenet.com/oracle/functions/

| SQL Element       | Category     | Description                                                  |
| :---------------- | :----------- | :----------------------------------------------------------- |
| `REGEXP_LIKE `    | `Condition ` | Searches a character column for a pattern. Use this function in the `WHERE` clause of a query to return rows matching a regular expression. The condition is also valid in a constraint or as a PL/SQL function returning a boolean. The following `WHERE` clause filters employees with a first name of Steven or Stephen:`WHERE REGEXP_LIKE(first_name, '^Ste(v|ph)en$') ` |
| `REGEXP_REPLACE ` | `Function `  | Searches for a pattern in a character column and replaces each occurrence of that pattern with the specified string. The following function puts a space after each character in the `country_name` column:`REGEXP_REPLACE(country_name, '(.)', '\1 ') ` |
| `REGEXP_INSTR  `  | `Function  ` | Searches a string for a given occurrence of a regular expression pattern and returns an integer indicating the position in the string where the match is found. You specify which occurrence you want to find and the start position. For example, the following performs a boolean test for a valid email address in the `email` column:`REGEXP_INSTR(email, '\w+@\w+(\.\w+)+') > 0 ` |
| `REGEXP_SUBSTR `  | `Function `  | Returns the substring matching the regular expression pattern that you specify. The following function uses the `x` flag to match the first string by ignoring spaces in the regular expression:`REGEXP_SUBSTR('oracle', 'o r a c l e', 1, 1, 'x')` |

## regexp_like

https://docs.oracle.com/en/database/oracle/oracle-database/19/sqlrf/Pattern-matching-Conditions.html#GUID-3FA7F5AB-AC64-4200-8F90-294101428C26



##regexp_instr

https://docs.oracle.com/cd/B12037_01/server.101/b10759/functions114.htm



## regexp_replace

https://docs.oracle.com/cd/B12037_01/server.101/b10759/functions115.htm



## regexp_substr

https://docs.oracle.com/cd/B12037_01/server.101/b10759/functions116.htm



### 变型使用 - 行转列

```sql
select regexp_substr('SMITH,ALLEN,WARD,JONES', '[^,]+', 1, level)
from dual
connect by regexp_substr('SMITH,ALLEN,WARD,JONES', '[^,]+', 1, level) is not null
```

### 🤔️当需要查询条件查询不出来，仍需要展示列记录为0 ？

可以采用***外链接***和***nvl语句***



##decode

含义解释：
**decode**(条件,值1,返回值1,值2,返回值2,...值n,返回值n,缺省值)

该函数的含义如下：
IF 条件=值1 THEN
　　　　RETURN(翻译值1)
ELSIF 条件=值2 THEN
　　　　RETURN(翻译值2)
　　　　......
ELSIF 条件=值n THEN
　　　　RETURN(翻译值n)
ELSE
　　　　RETURN(缺省值)
END IF

# DBA工作

```sql
select * from DBA_SEGMENT;
--查询对象
```





## Tablespace扩充

```sql
--0、以oracle用戶登陸數據庫對應的IP

--1、以sysdba登陸數據庫
sqlplus / as sysdba

--2、查詢出表空間數據文件列表
select file_name from dba_data_files where tablespace_name='表空間名稱';

--3、可另開一窗口查看某一表空間數據文件的大小，以便增加指定大小的表空間。
ll /TABLESPACE/ORADATA/XXXXXXXXXXXX.dbf

--4、對指定文件進行RESIZE擴充大小
alter database datafile '/TABLESPACE/ORADATA/XXXXXXXXXXXX.dbf' RESIZE 1G; 

--附表空間管理語句：
--增加數據文件
alter tablespace 表空間名稱 add datafile '表空間文件路徑如：/path/file_name.dbf' size 128M;

--刪除數據文件
alter tablespace 表空間名稱 drop datafile '表空間文件路徑';

--刪除整個表空間
drop tablespace 表空間名稱 including contents(同時刪除表空間文件增加' and datafiles ') cascade constraints;

--自動擴展表空間格式， 需要定義最大擴展到多少
alter database datafile '表空間文件全路徑' autoextend on next 每次自動擴展的大小  MAXSIZE 定義最大擴展到多少;
```

参考：https://www.itread01.com/articles/1478161554.html







## Oracle 递归查询语句

参考：https://www.cnblogs.com/Soprano/p/10659127.html



初始化表的数据：

```sql
CREATE TABLE DISTRICT
(
  ID         NUMBER(10)                  NOT NULL,
  PARENT_ID  NUMBER(10),
  NAME       VARCHAR2(255 BYTE)          NOT NULL
);

ALTER TABLE DISTRICT ADD (
  CONSTRAINT DISTRICT_PK
 PRIMARY KEY
 (ID));

ALTER TABLE DISTRICT ADD (
  CONSTRAINT DISTRICT_R01 
 FOREIGN KEY (PARENT_ID) 
 REFERENCES DISTRICT (ID));
 
 
 insert into DISTRICT (id, parent_id, name)
values (1, null, '四川省');
insert into DISTRICT (id, parent_id, name)
values (2, 1, '巴中市');
insert into DISTRICT (id, parent_id, name)
values (3, 1, '达州市');
insert into DISTRICT (id, parent_id, name)
values (4, 2, '巴州区');
insert into DISTRICT (id, parent_id, name)
values (5, 2, '通江县');
insert into DISTRICT (id, parent_id, name)
values (6, 2, '平昌县');
insert into DISTRICT (id, parent_id, name)
values (7, 3, '通川区');
insert into DISTRICT (id, parent_id, name)
values (8, 3, '宣汉县');
insert into DISTRICT (id, parent_id, name)
values (9, 8, '塔河乡');
insert into DISTRICT (id, parent_id, name)
values (10, 8, '三河乡');
insert into DISTRICT (id, parent_id, name)
values (11, 8, '胡家镇');
insert into DISTRICT (id, parent_id, name)
values (12, 8, '南坝镇');
insert into DISTRICT (id, parent_id, name)
values (13, 6, '大寨乡');
insert into DISTRICT (id, parent_id, name)
values (14, 6, '响滩镇');
insert into DISTRICT (id, parent_id, name)
values (15, 6, '龙岗镇');
insert into DISTRICT (id, parent_id, name)
values (16, 6, '白衣镇');
commit;
```



### 一、start with connect by prior 语句

https://docs.oracle.com/cd/B19306_01/server.102/b14200/queries003.htm

`START` `WITH` specifies the root row(s) of the hierarchy.

`CONNECT` `BY` specifies the relationship between parent rows and child rows of the hierarchy.



***注意：*** 语句中的 `condition` 将会在递归完毕后，进行最终结果筛选



1. 查询所有的子节点：

```sql
SELECT *
FROM district
START WITH NAME ='巴中市'
CONNECT BY PRIOR ID=parent_id
```

2. 查询所有父节点：

```sql
SELECT *
FROM district
START WITH NAME ='巴中市'
CONNECT BY ID= PRIOR parent_id
```

3. 查询指定节点的，根节点

```sql
SELECT d.*,
connect_by_root(d.id),
connect_by_root(NAME)
FROM district d
WHERE NAME='平昌县'
START WITH d.parent_id=1    --d.parent_id is null 结果为四川省
CONNECT BY PRIOR d.ID=d.parent_id
```

4. 查询巴中市下行政组织递归路径

```sql
SELECT ID,parent_id,NAME,
sys_connect_by_path(NAME,'->') namepath,
LEVEL
FROM district 
START WITH NAME='巴中市'
CONNECT BY PRIOR ID=parent_id
```



### 二、with语句实现递归

code:

1. 递归子类：

```sql
WITH t (ID ,parent_id,NAME) --要有列名
AS(
SELECT ID ,parent_id,NAME FROM district WHERE NAME='巴中市'
UNION ALL
SELECT d.ID ,d.parent_id,d.NAME FROM t,district d --要指定表和列表，
WHERE t.id=d.parent_id
)
SELECT * FROM t;
```

2. 递归父类：

```sql
WITH t (ID ,parent_id,NAME) --要有表
AS(
SELECT ID ,parent_id,NAME FROM district WHERE NAME='通江县'
UNION ALL
SELECT d.ID ,d.parent_id,d.NAME FROM t,district d --要指定表和列表，
WHERE t.parent_id=d.id
)
SELECT * FROM t;
```



## rownum And order by 使用的情况

在单一语句中，rownum会优先比order by 先进行排序

```sql
/*
rowrun=1 和 order by之间的问题
需要根据先后顺序进行:
a\如果默认rownum = 1 在order by 之前，则相当于如下：
*/
select *
from (select *
      from MS_VIS_RECORD a
      where ROWNUM = 1) T1
order by T1.FD_VIS_TIME desc;
/*
b\如果默认rownum = 1 在order by 之后，需要如下修改：
*/
select *
from (select *
      from MS_VIS_RECORD a
      order by a.FD_VIS_TIME desc)
where ROWNUM = 1;
```





## 日志清理策略计划

可以通过自增分区，进行特定分区的交换，实现对应的数据清理策略；

策略：

1. 实现自增的数据库表结构；

2. 实现备份表与目标表的分区交换操作；

   【通过数据库视图，循环♻️处理，保证数据交换安全；同时源表不能设计主键，及global索引，会导致索引失效】



分区交换语法：

基本语法：ALTER TABLE...EXCHANGE PARTITION



## 1、交换一个间隔分区表

Oracle11g中一种新的分区类型很好的解决了这个问题–interval partition，它是传统范围分区的扩展，使得分区表的使用和维护更加灵活。

```sql
LOCK TABLE interval_sales
PARTITION FOR (TO_DATE('01-JUN-2007','dd-MON-yyyy'))
IN SHARE MODE;

ALTER TABLE interval_sales
EXCHANGE PARTITION FOR (TO_DATE('01-JUN-2007','dd-MON-yyyy'))
WITH TABLE interval_sales_jun_2007
INCLUDING INDEXES;
```



## 创建DBLink操作

```sql
create database  link #{db_link_name} connect to #{user} identified by #{password} using '(DESCRIPTION =(ADDRESS_LIST =(ADDRESS =(PROTOCOL = TCP)(HOST = 192.168.101.5)(PORT = 1521)))(CONNECT_DATA =(SERVICE_NAME = search)))';
```





#EXP和IMP命令

ep ：

1、（**10g或以前**）导出指定表 exp 'sys/pwd@server1 as sysdba' file=c:\temp\tables.dmp tables=(schema1.table1,schema1.table2) 

​		（**10g或以后**）导出指定表 exp 'sys/pwd@server1 as sysdba' file=c:\temp\tables.dmp tables=schema1.table1,schema1.table2

2、（**10g或以前**）导入指定表 imp 'sys/pwd@server2 as sysdba' file=c:\temp\tables.dmp fromuser=schema1 touser=schema1 tables=(table1,table2) ignore=Y

​	（**10g或以后**）导入指定表 imp 'sys/pwd@server2 as sysdba' file=c:\temp\tables.dmp fromuser=schema1 touser=schema1 tables=table1,table2 ignore=Y



参数的使用：

### 导出exp参数说明

| 参数名               | 说明                       | 默认值       |
| -------------------- | -------------------------- | ------------ |
| USERID               | 用户名/口令                |              |
| FULL                 | 导出整个文件               | N            |
| BUFFER               | 数据缓冲区的大小           |              |
| OWNER                | 导出指定的所有者用户名列表 |              |
| FILE                 | 输出文件                   | (EXPDAT.DMP) |
| TABLES               | 导出指定的表名列表         |              |
| COMPRESS             | 是否压缩导出的文件         | Y            |
| RECORDLENGTH         | IO 记录的长度              |              |
| GRANTS               | 导出权限                   | Y            |
| INCTYPE              | 增量导出类型               |              |
| INDEXES              | 导出索引                   | Y            |
| RECORD               | 跟踪增量导出               | Y            |
| ROWS                 | 导出数据行                 | Y            |
| PARFILE              | 参数文件名                 |              |
| CONSTRAINTS          | 导出限制                   | Y            |
| CONSISTENT           | 交叉表一致性               |              |
| LOG                  | 屏幕输出的日志文件         |              |
| STATISTICS           | 分析对象(ESTIMATE)         |              |
| DIRECT               | 直接路径                   | N            |
| TRIGGERS             | 导出触发器                 | Y            |
| FEEDBACK             | 显示每 x 行 (0) 的进度     |              |
| FILESIZE             | 各转储文件的最大尺寸       |              |
| QUERY                | 选定导出表子集的子句       |              |
| TRANSPORT_TABLESPACE | 导出可传输的表空间元数据   | N            |
| TABLESPACES          | 导出指定的表空间列表       |              |

### 导入imp参数说明



| 参数名                | 说明                           | 默认值       |
| --------------------- | ------------------------------ | ------------ |
| USERID                | 用户名/口令                    |              |
| FULL                  | 导入整个文件                   | N            |
| BUFFER                | 数据缓冲区大小                 |              |
| FROMUSER              | 所有人用户名列表               |              |
| FILE                  | 输入文件                       | (EXPDAT.DMP) |
| TOUSER                | 用户名列表                     |              |
| SHOW                  | 只列出文件内容                 | N            |
| TABLES                | 表名列表                       |              |
| IGNORE                | 忽略创建错误                   | N            |
| RECORDLENGTH          | IO记录的长度                   |              |
| GRANTS                | 导入权限                       | Y            |
| INCTYPE               | 增量导入类型                   |              |
| INDEXES               | 导入索引                       | Y            |
| COMMIT                | 提交数组插入                   | N            |
| ROWS                  | 导入数据行                     | Y            |
| PARFILE               | 参数文件名                     |              |
| LOG                   | 屏幕输出的日志文件             |              |
| CONSTRAINTS           | 导入限制                       | Y            |
| DESTROY               | 覆盖表空间数据文件             | N            |
| INDEXFILE             | 将表/索引信息写入指定的文件    |              |
| SKIP_UNUSABLE_INDEXES | 跳过不可用索引的维护           | N            |
| FEEDBACK              | 每 x 行显示进度                |              |
| TOID_NOVALIDATE       | 跳过指定类型 ID 的验证         |              |
| FILESIZE              | 每个转储文件的最大大小         |              |
| STATISTICS            | 始终导入预计算的统计信息       |              |
| RESUMABLE             | 在遇到有关空间的错误时挂起     |              |
| RESUMABLE_NAME        | 用来标识可恢复语句的文本字符串 |              |
| RESUMABLE_TIMEOUT     | RESUMABLE 的等待时间           |              |
| COMPILE               | 编译过程, 程序包和函数         | Y            |
| STREAMS_CONFIGURATION | 导入 Streams 的一般元数据      | Y            |
| STREAMS_INSTANITATION | 导入 Streams 的实例化元数据    | N            |
| TRANSPORT_TABLESPACE  | 导入可传输的表空间元数据       |              |
| TABLESPACES           | 将要传输到数据库的表空间       |              |
| DATAFILES             | 将要传输到数据库的数据文件     |              |
| TTS_OWNERS            | 拥有可传输表空间集中数据的用户 |              |



**1.6****使用参数文件导出数据**

exp system/manager@服务命名 parfile=bible_tables.par

bible_tables.par（参数示例文件）：

\#Export the sample tables used for the Oracle8i Database Administrator‘s Bible.

file=bible_tables（文件存储的路径以及名称）

log=bible_tables（日志存储的路径以及名称）

tables=(

amy.artist

amy.books

seapark.checkup

seapark.items

)



参考：https://blog.csdn.net/bsjhclhj/article/details/88928001

一、描述
前天系统对个别表进行分区，采用间隔分区，第二天凌晨exp备份间隔分区表报错，报错关键字EXP-00006

------

二、现象

When exporting a composite. interval, or system partitioned table using conventional export utility, it fails with EXP-6 and EXP-0 errors:

**EXP-00006: internal inconsistency error
EXP-00000: Export terminated unsuccessfully**

------

三、解决方案
Use `**Data Pump**` to perform exports of composite and interval partitioning and system partitioned tables as this is the recommended method.

------

四、建议
1、11g 尽量不要用exp对数据库进行备份，使用数据泵
2、慎重使用数据库新特性，熟悉新特性
3、开发部门和要大数据部门测试完整，防止测试不到位，引起生产问题



#DATA DUMP

https://oracle-base.com/articles/10g/oracle-data-pump-10g

##expdp数据泵工具导出的步骤：

 1、创建DIRECTORY
create directory dir_dp as 'D:/oracle/dir_dp';
2、授权
Grant read,write on directory dir_dp to zftang;
--查看目录及权限
SELECT privilege, directory_name, DIRECTORY_PATH FROM user_tab_privs t, all_directories d
 WHERE t.table_name(+) = d.directory_name ORDER BY 2, 1;
3、执行导出	
expdp zftang/zftang@fgisdb schemas=zftang directory=dir_dp dumpfile =expdp_test1.dmp logfile=expdp_test1.log;

```shell
1、按表模式导出：
expdp zftang/zftang@fgisdb  tables=zftang.b$i_exch_info,zftang.b$i_manhole_info dumpfile =expdp_test2.dmp logfile=expdp_test2.log directory=dir_dp job_name=my_job

2、按查询条件导出：
expdp zftang/zftang@fgisdb  tables=zftang.b$i_exch_info dumpfile =expdp_test3.dmp logfile=expdp_test3.log directory=dir_dp job_name=my_job query='"where rownum<11"'

3、按表空间导出：
Expdp zftang/zftang@fgisdb dumpfile=expdp_tablespace.dmp tablespaces=GCOMM.DBF logfile=expdp_tablespace.log directory=dir_dp job_name=my_job

4、导出方案
Expdp zftang/zftang DIRECTORY=dir_dp DUMPFILE=schema.dmp SCHEMAS=zftang,gwm

5、导出整个数据库：
expdp zftang/zftang@fgisdb dumpfile =full.dmp full=y logfile=full.log directory=dir_dp job_name=my_job
```

##impdp导入模式：

```shell
1、按表导入
p_street_area.dmp文件中的表，此文件是以gwm用户按schemas=gwm导出的：
impdp gwm/gwm@fgisdb  dumpfile =p_street_area.dmp logfile=imp_p_street_area.log directory=dir_dp tables=p_street_area job_name=my_job

2、按用户导入（可以将用户信息直接导入，即如果用户信息不存在的情况下也可以直接导入）
impdp gwm/gwm@fgisdb schemas=gwm dumpfile =expdp_test.dmp logfile=expdp_test.log directory=dir_dp job_name=my_job

3、不通过expdp的步骤生成dmp文件而直接导入的方法：
--从源数据库中向目标数据库导入表p_street_area
impdp gwm/gwm directory=dir_dp NETWORK_LINK=igisdb tables=p_street_area logfile=p_street_area.log  job_name=my_job
igisdb是目的数据库与源数据的链接名，dir_dp是目的数据库上的目录

4、更换表空间
  采用remap_tablespace参数
  --导出gwm用户下的所有数据
expdp system/orcl directory=data_pump_dir dumpfile=gwm.dmp SCHEMAS=gwm
注：如果是用sys用户导出的用户数据，包括用户创建、授权部分，用自身用户导出则不含这些内容
--以下是将gwm用户下的数据全部导入到表空间gcomm（原来为gmapdata表空间下）下
impdp system/orcl directory=data_pump_dir dumpfile=gwm.dmp remap_tablespace=gmapdata:gcomm

5、源库和目标库对应的表空间不一样：
impdp 'sys/pwd@server2 as sysdba' directory=dbbak dumpfile=tables.dmp tables=schema1.table1,schema1.table2  remap_schema=schema1:schema2 table_exists_action=tablespace1:tablespace2 
#remap_schema=schema1:schema2，源库shema1:目标库schema2
#remap_tablespace=tablespace1:tablespace2，源表空间：目标表空间

如果impdp使用remap_schema，且指定要导入的表时，要指定表的schema。
$ impdp username/password directory=DPUMPDIR dumpfile=scott220.DMP tables=scott.prod_201203_26  remap_schema=scott:liangwei logfile=impdp.log
```

###***IMPORTANT**

Remap_tablespace如果需要转换多个表空间，如A1转换成B1，A2转换成B1，有如下两种方式

remap_tablespace=A1:B1 remap_tablespace=A2:B1

remap_tablespace= ‘(A1:B1, A2:B1)'





##数据库修改用户名称

1、用sysdba账号登入数据库，然后查询到要更改的用户信息：

```plsql
   SELECT user#,name FROM user$;
```



2、更改用户名并提交：

```sql
 UPDATE USER$ SET NAME='PORTAL' WHERE user#=88;
 COMMIT;
```

3、强制刷新：

```sql
 ALTER SYSTEM CHECKPOINT;
 ALTER SYSTEM FLUSH SHARED_POOL;
```

4、更新用户的密码：

```sql
 ALTER USER PORTAL IDENTIFIED BY 123;
```





Oracle Chr()函数可以根据数字代码返回字符，其功能和ASCII函数相反。本教程将为大家带来Chr()函数的语法和示例。

## Chr()函数语法

```
CHR( number_code )
```

## 参数

number_code：用于检索对应字符的NUMBER代码。

## 返回值

返回一个字符串值。

chr(10)是换行符

chr(13)是回车



在Oracle中，Ascii()函数可以返回代表指定字符的数字值代码，那么Ascii()函数具体该如何实用呢？

## Ascii()函数语法

```
ASCII( single_character )
```

## **参数**

single_character：指定的字符来检索NUMBER代码。 如果输入多个字符，则ASCII函数将返回第一个字符的值，并忽略第一个字符后的所有字符。

## **返回值**

ASCII函数返回一个数值。





##ora-01722 ：**invalid number**

http://www.orafaq.com/wiki/ORA-01722

当数据库的字段或者查询结果集里面，存在无法转换成数字匹配的***字符串***或者***null***

原因：由于数字的自动转换优先级高，***null***和***字符串***转换成数字匹配就会报错，以下为数据类型转换的优先级列表：



- ***\*数据类型优先级\****

Datetime and interval 类型

BINARY_DOUBLE

BINARY_FLOAT

NUMBER

字符类型

所有其它内置类型



### [Oracle删除当前用户下的所有表、视图、序列、函数、存储过程、包](http://uu011.iteye.com/blog/1627692)

**博客分类：** [oracle](http://uu011.iteye.com/category/210906)

```sql
--delete tables 
Sql代码 
select 'drop table ' || table_name ||';'||chr(13)||chr(10) from user_tables;  

select 'drop table ' || table_name ||';'||chr(13)||chr(10) from user_tables; 

--delete views 
Sql代码 
select 'drop view ' || view_name||';'||chr(13)||chr(10) from user_views;  

select 'drop view ' || view_name||';'||chr(13)||chr(10) from user_views; 

--delete seqs 
Sql代码 
select 'drop sequence ' || sequence_name||';'||chr(13)||chr(10) from user_sequences; 

select 'drop sequence ' || sequence_name||';'||chr(13)||chr(10) from user_sequences; 

--delete functions 
Sql代码 
select 'drop function ' || object_name||';'||chr(13)||chr(10) from user_objects where object_type='FUNCTION';  

select 'drop function ' || object_name||';'||chr(13)||chr(10) from user_objects where object_type='FUNCTION'; 

--delete procedure 
Sql代码 
select 'drop procedure ' || object_name||';'||chr(13)||chr(10) from user_objects where object_type='PROCEDURE';  

select 'drop procedure ' || object_name||';'||chr(13)||chr(10) from user_objects where object_type='PROCEDURE'; 

--delete package 
Sql代码 
select 'drop package ' || object_name||';'||chr(13)||chr(10) from user_objects where object_type='PACKAGE';  

select 'drop package ' || object_name||';'||chr(13)||chr(10) from user_objects where object_type='PACKAGE'; 
```

 

##SQLPlus

`／`命令的作用是执行缓冲区中**刚刚输入**的或者**已经执行**内容。

需要屏蔽密码中的转义或特殊字符，需要将其用` \"\" `包裹起来



## UNDO问题

问题说明：2020/12/7号，通过shell批量执行迁移脚本时，执行线索表迁移1000w数据时，出现undo空间不足报错ORA-30036；

***ps : 通过单独执行的方式，是可以迁移1000w的数据量。***



尝试解决：相关的截图及信息参考/光大/BAK/20201207

> ALTER SYSTEM SET undo_retention=0 SCOPE=BOTH;

​		修改后重新执行，失败；





*UNDO*段与*UNDO*表空间：

​    *UNDO*段中的内容存储在*UNDO*表空间

​    任意给定时刻只能使用一个*UDNO*表空间

​    *UNDO*表空间必须被创建为持久的、本地管理、可自动扩展的表空间

​    正在使用的*UNDO*表空间不能撤销或删除

​    *UNDO*表空间使用循环写的方式，与联机日志文件写相似，不同的是*UNDO*中可以设置了*undo_retention* 保留时间

 

 *UNDO*段的两种管理方式：

​    *AUTO*  自动管理*(*推荐*)*

​    *MANUAL* 手动管理*(*仅保留*)*



*--*查看和*UNDO*相关的参数

​    *SQL> SHOW PARAMETER undo;*

​    *NAME                 TYPE    VALUE*

​    *------------------------------------ ----------- ------------------------------*

​    *undo_management           string   MANUAL*

​    *undo_retention            integer   900*

​    *undo_tablespace           string   UNDOTBS1*



瞎逼操作如下：

```sql
alter tablespace undotbs1 retention noguarantee;

--查看undo使用空间
Select Sum(bytes / (1024 * 1024)), a.status
  From dba_undo_extents a
  Group By a.status
```



# oracle undo表空间的清理记录

#### 1、创建临时undo表空间

```
create undo tablespace undotemp datafile ‘/u01/oradata/undotemp.dbf’ size 500M;
```

#### 2、修改undo表空间到刚刚创建的表空间

```
alter system set undo_tablespace=undotemp;
```

#### 3、删除老的表空间文件

```
drop tablespace undotbs including contents and datafiles;
```

#### 4、创建一个新的undo表空间文件

```
create undo tablespace undotbs datafile ‘/u01/oradata/undotbs01.dbf’ size 32700M;
```

#### 5、修改系统默认的undo表空间

```
alter  system set undo_tablespace=undotbs;
```

#### 6、删除第一步创建的临时Undo表空间

```
drop tablespace undotemp including contents and datafiles;
```

#### 7、检查

```
SQL> show parameter undo

NAME                                   TYPE VALUE
———————————— ———– ———————–
undo_management string                  AUTO
undo_retention integer                  900
undo_tablespace string                  UNDOTBS1
```

undo_management 参数：指定系统管理undo的模式。参数有AUTO/Manual

undo_retention参数：指定undo数据保留时长，系统默认为900秒。

undo_tablespace参数：undo使用的表空间。



## 将sqlplus中的参数输出到bash中

参考：https://stackoverflow.com/questions/56676299/how-can-i-capture-a-result-from-eof-and-put-into-a-variable-in-bash





## Oracle数据库空间问题

###--Oracle查看表空间大小(单位不是GB)
SELECT
a.tablespace_name, --表空间名
total, --表空间大小
free, --表空间剩余大小
(total-free), --表空间使用大小
Round((total-free)/total,4)*100 --使用率
FROM (SELECT tablespace_name,Sum(bytes) free
FROM DBA_FREE_SPACE
GROUP BY tablespace_name) a,
(SELECT tablespace_name,Sum(bytes) total
FROM DBA_DATA_FILES
GROUP BY tablespace_name)b
WHERE
a.tablespace_name=b.tablespace_name;

###--Oracle查看表空间当前用户
select username,default_tablespace
from user_users;

###--Oracle查看表所属表空间
SELECT TABLE_NAME,TABLESPACE_NAME
FROM USER_TABLES
WHERE TABLE_NAME='test_table';

###--Oracle查看表空间大小-单位GB
SELECT
a.tablespace_name,
total,
free,
(total-free),
total/(1024*1024*1024),
free/(1024*1024*1024),
(total-free)/(1024*1024*1024),
round((total-free)/total,4)*100
FROM (SELECT tablespace_name,SUM(bytes) free
FROM dba_free_space
GROUP BY tablespace_name)a,
(SELECT tablespace_name,SUM(bytes) total
FROM dba_data_files
GROUP BY tablespace_name)b
WHERE a.tablespace_name=b.tablespace_name;



## Oracle 设置表空间自动扩展

```sql
--设置文件的自动增长
Alter database datafile ‘数据文件存放路径‘ autoextend on next 每次增加的大小 maxsize 数据文件大小的最大值

--增加数据文件
Alter tablespace 表空间名 add datafile ‘数据文件存放的路径’ size 数据文件大小M autoextend on next 每次自增长大小M Maxsize UNLIMITED；（后半部分为设置自增长）

--修改某数据文件为不限制大小
ALTER DATABASE DATAFILE 'D:\oracle_data\xxx.DBF' AUTOEXTEND ON NEXT 500M MAXSIZE UNLIMITED;

--增加新的数据文件
alter tablespace 表空间名 add datafile 'E:/oracle_data/data/xxx.dbf' size 1000M AUTOEXTEND ON NEXT 500M MAXSIZE UNLIMITED;

--删除指定的表空间文件
ALTER TABLESPACE 表空间名  DROP DATAFILE  'D:\oracle_data\xxx.DBF';

 
```





## 外键何去何从？

**到底该不该使用外键Foreign Key**

外键目的是为了保证数据完整性和一致性，避免产生脏数据，设置外键有啥缺点呢。

1. **影响写入性能**：对于insert来说，每次都要判断从表的外键列是否在主表中存在（例如每次插入orders表，都要判断下user_id是否在users中存在），会降低数据库的写入性能，对于MySQL本来就只有Master输出写能力的数据库，就不太合适了，MySQL开发规范规定不允许使用外键也是有一定道理的。
2. **并发问题**：在使用外键的情况下，每次修改数据都需要去另外一个表检查数据，需要获取额外的锁。在高并发大场景，使用外键造成死锁或锁等几率更大。

实际开发中，更多的是不靠外键来保证数据的完整性和一致性，而是通过的业务逻辑，比如用户要下单，必须先登录系统，下单只需要将登录的用户编号写入到订单表，这个用户必然是存在于用户表的，对于一个用户友好的系统来说，尽量让用户选择，不要人工输入，这样可以保证数据一致性，避免脏数据的产生。



###UNION ALL和UNION区别

union和union all的区别是,union会自动压缩多个结果集合中的重复结果，而union all则将所有的结果全部显示出来，不管是不是重复。



### insert 返回 rowid

通过insert语句，插入并返回***rowid***

```plsql
declare
type   tt_rowids is table of rowid index by binary_integer;
vIds   tt_rowids;
begin
update mytab
   set cust_id = 'RUN2'
 where id between 2 and 10
returning rowid bulk collect into vIds;

for i in vIds.first..vIds.last loop
  dbms_output.put_line('ROWID of updated row '||i||' is '||vIds(i));
end loop;
commit;
end;
/
```



####bulk批量形式

```plsql
declare
type   tt_rowids is table of rowid index by binary_integer;
vIds   tt_rowids;
begin
update mytab
   set cust_id = 'RUN2'
 where id between 2 and 10
returning rowid bulk collect into vIds;

for i in vIds.first..vIds.last loop
  dbms_output.put_line('ROWID of updated row '||i||' is '||vIds(i));
end loop;
commit;
end;
/
```



###设置单行查看数据sqlPlus

```sql
set linesize 120;
column status foramt a20;
```



###[Oracle函数——MINUS](https://www.cnblogs.com/zuiyue_jing/p/12019766.html)

```sql
select 字段名称 from 表1 minus select 字段名称 from 表2；
```

- “minus”直接翻译为中文是“减”的意思，在Oracle中也是用来做减法操作的，只不过它不是传统意义上对数字的减法，而是对查询结果集的减法。A minus B就意味着将结果集A去除结果集B中所包含的所有记录后的结果，即在A中存在，而在B中不存在的记录。
- racle的minus是按列进行比较的，所以A能够minus B的前提条件是结果集A和结果集B需要有相同的列数，且相同列索引的列具有相同的数据类型。
- Oracle会对minus后的结果集进行去重，即如果A中原本多条相同的记录数在进行A minus B后将会只剩一条对应的记录。



### Oracle 分区分割操作导致索引失效问题

分割分区的情况下：如果分区分割有数据，会导致global索引失效；

衍生：删除，分割交换操作都会导致索引失效;

修复如下：

ps：该语句只能将***indexes***保持为原样；

​		如果索引已经失效，需要先***rebuid***后才可以保持；

```sql
alter table `table` drop partition `partition` update GLOBAL INDEXES
```



DBA建议：

>怎么解决索引失效问题，那就是加上update indexes
>加上update indexes，以上任何操作不会引起glocal索引失效;
>加上update indexes，以上操作中除了EXCHANGE PARTITION操作以外，不会引起local 索引失效。
>*EXCHANGE PARTITION操作是个很特殊的操作，加上update indexes参数，EXCHANGE PARTITION依然会造成local 索引失效。
>
>需要注意的是，如果分区中不含数据，上面的操作都不会引起索引失效（EXCHANGE PARTITION除外）。
>
>
>如何避免在交换分区过程中local索引失效？ 在临时表上创建和分区表相同列的索引，再exchenge时加上关键字including indexes



### online 和 非online方式创建索引

>(1) online和非online方式创建索引，效果相同。 
>(2) online方式创建索引，由于使用了一张临时表，以ROW SHARE锁表，不会阻塞原表DML的语句，非online方式创建索引，则会以SHARE NOWAIT锁表，阻塞原表DML语句。 
>(3) 由于online方式创建索引，Oracle执行工作复杂，因此比非online方式创建索引用时要久。 
>(4) 一句话“不能什么便宜均占着”，要么选择可以快速创建索引的非online方式但创建期间会锁表阻塞DML语句，要么选择不会阻塞原表DML语句的online方式创建索引但用时较久。从实际来看，我理解，若小表选择任何一种均可，大表，尤其是生产系统，找不着非高峰时间，选择online更合理一些，若不关注是否影响DML操作，则两种方式均可以了。

```
ORA-00054: resource busy and acquire with NOWAIT specified or timeout expired
```

使用非online创建索引，如果表上存在未提交事务，则无法执行，提示错误ORA-00054，直到所有事务已经提交。



### merge 语句

The Oracle `MERGE` statement [selects](https://www.oracletutorial.com/oracle-basics/oracle-select/) data from one or more source tables and [updates](https://www.oracletutorial.com/oracle-basics/oracle-update/) or [insert](https://www.oracletutorial.com/oracle-basics/oracle-insert/)s it into a target table. 

```sql
MERGE INTO target_table 
USING source_table 
ON search_condition
    WHEN MATCHED THEN
        UPDATE SET col1 = value1, col2 = value2,...
        WHERE <update_condition>
        [DELETE WHERE <delete_condition>]
    WHEN NOT MATCHED THEN
        INSERT (col1,col2,...)
        values(value1,value2,...)
        WHERE <insert_condition>;
```



### with 语句

```sql
with_clause:
    WITH [RECURSIVE]
        cte_name [(col_name [, col_name] ...)] AS (subquery)
        [, cte_name [(col_name [, col_name] ...)] AS (subquery)] ...
```



### lpad &&  rpad

oracle函数中有不足位数补足的函数

```sql
RPAD( string1, padded_length [, pad_string] )

LPAD (text-exp , length [, pad-exp])
```



### 两表关联更新

--内联视图更新

```sql
UPDATE (
select t1.fmoney  fmoney1,t2.fmoney  fmoney2 from t1,t2 where t1.fname = t2.fname
)t
set fmoney1 =fmoney2;
```

   用A表去更新B表的数据，A表的关联条件必须为主键，Oracle这样做的目的是保证表A的满足关联条件的数据是唯一的，

   这样在更新B表数据时才有意义（自己都不确定怎么影响别人，是吧，hehe），

   当然，如果两表关联的字段都为主键，则无论谁更新谁都没有问题。

   结论：用A表数据更新B表数据，则A与B的对应关系为：1：1 或 1：n。

***使用唯一性的group by 语句操作亦可以***

***通过视图创建主键***

>Creating a View with Constraints: Example The following statement creates a
>restricted view of the sample table hr.employees and defines a unique constraint on
>the email view column and a primary key constraint for the view on the emp_id
>view column:
>CREATE VIEW emp_sal (emp_id, last_name,
>email UNIQUE RELY DISABLE NOVALIDATE,
>CONSTRAINT id_pk PRIMARY KEY (emp_id) RELY DISABLE NOVALIDATE)
>AS SELECT employee_id, last_name, email FROM employees;





```sql
update TA a set(name, remark)=(select b.name, b.remark from TB b where b.id=a.id)   
where exists(select 1 from TB b where b.id=a.id)  
```

***注意如果不添加后面的exists语句，TA关联不到的行name, remark栏位将被更新为NULL值， 如果name, remark栏位不允许为null，则报错。 这不是我们希望看到的。***





### Pivot 和 Unpivot - 行列转制

pivot语句

```sql
pivot 
  (
     count(state_code)
     for state_code in ('NY','CT','NJ','FL','MO')
  )
  å
 Puchase Frequency   New York       Conn New Jersey    Florida   Missouri
----------------- ---------- ---------- ---------- ---------- ----------
                1      33048        165          0          0          0
                2      33151        179          0          0          0
                3      32978        173          0          0          0
                4      33109        173          0          1          0
```

ps : `for`语句中的可以对列名进行更名

unpivot语句

```sql
select *
  from cust_matrix
unpivot
(
  state_counts
    for state_code in ("New York","Conn","New Jersey","Florida","Missouri")
)
order by "Puchase Frequency", state_code
/

Puchase Frequency STATE_CODE STATE_COUNTS
----------------- ---------- ------------
                1 Conn                165
                1 Florida               0
                1 Missouri              0
                1 New Jersey            0
                1 New York          33048
                2 Conn                179
                2 Florida               0
                2 Missouri              0
```





### 视图相关

oracle视图实际上就是封装sql语句，对外提供一个别名，使用者不需要关心复杂的sql，视图执行之后会将执行的结果当做一个表来使用，相当于一个虚拟的表，如果想在视图上进行DML的操作，在创建时候有两个选项

（1）选择项WITH CHECK OPTION表示对视图进行UPDATE INSERT DELETE操作时，要保证操作的数据满足视图定义的谓词条件，也就是视图子查询中的WHERE子句的条件

（2）选项WITH READ ONLY 只读视图，不允许通过本视图更新本表

```sql
Create [or  Replace] VIEW VIEW_NAME AS 子查询【WITH CHECK OPTION】【WITH READ ONLY】
DROP  VIEW  VIEW_NAME
```



关于交换分区的相关事宜：

​		交换分区下面的这两个参数表明 - 使用 `EXCLUDING INDEXES`只有源表和目标表索引一致，交换后索引才不会失效；

If the INCLUDING INDEXES clause is specified with EXCHANGE PARTITION, then matching indexes in the *target_partition* and *source_table* are swapped. Indexes in the *target_partition* with no match in the *source_table* are rebuilt and vice versa (that is, indexes in the *source_table* with no match in the *target_partition* are also rebuilt).

If the EXCLUDING INDEXES clause is specified with EXCHANGE PARTITION, then matching indexes in the *target_partition* and *source_table* are swapped, but the *target_partition* indexes with no match in the *source_table* are marked as invalid and vice versa (that is, indexes in the *source_table* with no match in the *target_partition* are also marked as invalid).

```sql
ALTER TABLE target_table 
  EXCHANGE PARTITION target_partition 
  WITH TABLE source_table 
  [(INCLUDING | EXCLUDING) INDEXES]
  [(WITH | WITHOUT) VALIDATION]
```

参考：https://www.enterprisedb.com/edb-docs/d/edb-postgres-advanced-server/user-guides/database-compatibility-for-oracle-developers-guide/9.6/Database_Compatibility_for_Oracle_Developers_Guide_v9.6.1.109.html





### Oracle 周函数

相关周的一些参数说明：

```sql
WW  Week of year (1-53) where week 1 starts on the first day of the year and continues to the seventh day of the year.
W   Week of month (1-5) where week 1 starts on the first day of the month and ends on the seventh.
IW  Week of year (1-52 or 1-53) based on the ISO standard.
```

鸣谢：Tom https://asktom.oracle.com/pls/apex/asktom.search?tag=week-of-year-in-sql-confusing



### AWR报告怎么看？

鸣谢：smileNicky https://www.jianshu.com/p/e73ec7ec67d6

#### load_profile



**load_profile**：指标主要用来显示当前系统的一些指示性能的总体参数

**Redo_size**：用来显示平均每秒的日志尺寸和平均每个事务的日志尺寸



如图，平均每秒的事务数Transactions非常小，说明系统压力非常小，一般来说Transactions不超过200都是正常的，或者200左右都是正常的，超过1000就是非常繁忙了，再看看平均每秒的日志尺寸是4位数的，平均每个事务的日志尺寸是5位数的，说明了系统访问不是很频繁，而单个业务是比较复杂的，如果反过来，平均每秒日志尺寸比平均每秒事务日志尺寸大很多，说明系统访问很频繁，而业务比较简单，不需要响应很久



### efficiency percentages



efficiency percentages是一些命中率指标。Buffer Hint、Library Hint等表示SGA(System global area)的命中率；Soft Parse指标表示共享池的软解析率，如果小于90%，就说明存在未绑定变量的情况







### Oracle如果没有数据，如何展示为一个特定的数据呢？

通过 ***外链接*** 的方式；

用 **一个绝对存在的数值** 左连接可能为空的数值；

```sql
select t1.month, count(D_DATEMONTH) as sum,decode('YC','YC','测试')as type
from 
(select  to_char(sysdate, 'YYYY-MM' )as month  from dual )t1 left join YCDIMISSION t2
on t1.month=t2.D_DATEMONTH
group by t1.month
```





### Oracle 对象最长字段

 >##### Long Identifiers
 >
 >The maximum length of identifiers is increased to 128 bytes for most identifiers, up from 30 bytes in previous releases.
 >
 >Providing longer identifiers gives customers greater flexibility in defining their naming schemes, such as longer and more expressive table names. Having longer identifiers also enables object name migration between databases with different character sets, such as Thai to Unicode.

参考：https://docs.oracle.com/en/database/oracle/oracle-database/12.2/newft/new-features.html#GUID-E82CA1F1-09C0-47DC-BC78-C984EC62BAF2





# ROWID



确切的说，当你是全新的数据库时，ROWID是递增的
而当一旦有了删除动作，ROWID就不会全是递增的



[oracle文档]: https://docs.oracle.com/cd/B19306_01/server.102/b14200/pseudocolumns008.htm	"rowid相关"
[jdbc文档]: https://docs.oracle.com/database/121/JAJDB/oracle/sql/ROWID.html	"rowid类"

[stack overflow]: https://stackoverflow.com/questions/33629074/how-to-find-the-partition-by-rowid-in-oracle	"rowid获取分区名称"





### AND 和 OR的优先级

| 运算符                                     | 级别 |
| ------------------------------------------ | ---- |
| 算术运算符(即‘+’,‘-’，‘*’,‘/’)             | 1    |
| 连接运算符（即‘\|\|’）                     | 2    |
| 比较运算符（即‘>’，‘>=’，‘<’，‘<=’，‘<>’） | 3    |
| Is [not] null,[not] like,[not] in          | 4    |
| [not] between-and                          | 5    |
| not                                        | 6    |
| and                                        |      |
| or                                         |      |

参考：https://docs.oracle.com/cd/E57185_01/ESTUG/apds03s01.html





### 删除分区操作



删除或清理分区操作

```sql
ALTER TABLE BUSTB_TESTRESULT DROP PARTITION RESULT_PART_201412; 
ALTER TABLE BUSTB_TESTRESULT TRUNCATE PARTITION RESULT_PART_201412; 


ALTER TABLE BUSTB_TESTRESULT DROP SUBPARTITION RESULT_PART_201412_0; 
ALTER TABLE BUSTB_TESTRESULT TRUNCATE SUBPARTITION RESULT_PART_201412_0; 

ALTER TABLE sales DROP PARTITION FOR(TO_DATE('01-SEP-2007','dd-MON-yyyy'));
```



># Dropping Interval Partitions
>
>You can drop interval partitions in an interval-partitioned table. This operation drops the data for the interval only and leaves the interval definition in tact. If data is inserted in the interval just dropped, then the database again creates an interval partition.
>
>You can also drop range partitions in an interval-partitioned table. The rules for dropping a range partition in an interval-partitioned table follow the rules for dropping a range partition in a range-partitioned table. If you drop a range partition in the middle of a set of range partitions, then the lower boundary for the next range partition shifts to the lower boundary of the range partition you just dropped. You cannot drop the highest range partition in the range-partitioned section of an interval-partitioned table.
>
>The following example drops the September 2007 interval partition from the `sales` table. There are only local indexes so no indexes are invalidated.

https://docs.oracle.com/database/121/VLDBG/GUID-09F5641F-821D-4971-81F8-583F7CD9CAA2.htm