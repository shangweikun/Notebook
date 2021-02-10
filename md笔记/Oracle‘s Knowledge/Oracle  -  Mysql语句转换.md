# Oracle 和 MySQL语句对照

## 基础类型区别

* **自增主键**

| Oracle                 | Mysql                                 |
| :--------------------- | ------------------------------------- |
| oracle主键自带自增长； | mysql可以声明自增长：auto_increment； |



* **类型区别**

| 编号 | ORACLE                                                       | MYSQL                                                        | 注释                                                         |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1    | NUMBER                                                       | int / DECIMAL                                                | DECIMAL就是NUMBER(10,2)这样的结构INT就是是NUMBER(10)，表示整型； MYSQL有很多类int型，tinyint mediumint bigint等，不同的int宽度不一样 |
| 2    | Varchar2（n）                                                | varchar(n)                                                   |                                                              |
| 3    | Date                                                         | DATA，TIME                                                   | 日期字段的处理 MYSQL日期字段分DATE和TIME两种，ORACLE日期字段只有DATE，包含年月日时分秒信息，用当前数据库的系统时间为 SYSDATE, 精确到秒，或者用字符串转换成日期型函数TO_DATE(‘2001-08-01’,’YYYY-MM-DD’)年-月-日 24小时:分钟:秒的格式YYYY-MM-DD HH24:MI:SS TO_DATE()还有很多种日期格式, 可以参看ORACLE DOC.日期型字段转换成字符串函数TO_CHAR(‘2001-08-01’,’YYYY-MM-DD HH24:MI:SS’)  日期字段的数学运算公式有很大的不同。<br>MYSQL找到离当前时间7天用DATE_FIELD_NAME ＞ SUBDATE（NOW（），INTERVAL 7 DAY）ORACLE找到离当前时间7天用 DATE_FIELD_NAME ＞SYSDATE - 7; <br>MYSQL中插入当前时间的几个函数是：NOW()函数以'YYYY-MM-DD HH:MM:SS'返回当前的日期时间，可以直接存到DATETIME字段中。<br>CURDATE()以’YYYY-MM-DD’的格式返回今天的日期，可以直接存到DATE字段中。<br>CURTIME()以’HH:MM:SS’的格式返回当前的时间，可以直接存到TIME字段中。<br>例：insert into `tablename` (`fieldname`) values (now())  而oracle中当前时间是sysdate |
| 4    | INTEGER                                                      | int / INTEGER                                                | Mysql中INTEGER等价于int                                      |
| 5    | EXCEPTION                                                    | SQLEXCEPTION                                                 | 详见<<2009001-eService-O2MG.doc>>中2.5 Mysql异常处理         |
| 6    | CONSTANT VARCHAR2(1)                                         | mysql中没有CONSTANT关键字                                    | 从ORACLE迁移到MYSQL,所有CONSTANT常量只能定义成变量           |
| 7    | TYPE `g_grp_cur` IS REF CURSOR;                              | 光标 : mysql中有替代方案                                     | 详见<<2009001-eService-O2MG.doc>>中2.2 光标处理              |
| 8    | TYPE `unpacklist_type` IS TABLE OF VARCHAR2(2000) INDEX BY BINARY_INTEGER; | 数组: mysql中借助临时表处理 或者直接写逻辑到相应的代码中， 直接对集合中每个值进行相应的处理 | 详见<<2009001-eService-O2MG.doc>>中2.4 数组处理              |
| 9    | 自动增长的序列                                               | 自动增长的数据类型                                           | MYSQL有自动增长的数据类型，插入记录时不用操作此字段，会自动获得数据值。ORACLE没有自动增长的数据类型，需要建立一个自动增长的序列号，插入记录时要把序列号的下一个值赋于此字段。 |
| 0    | NULL                                                         | NULL                                                         | 空字符的处理 MYSQL的非空字段也有空的内容，ORACLE里定义了非空字段就不容许有空的内容。<br>按MYSQL的NOT NULL来定义ORACLE表结构, 导数据的时候会产生错误。<br>因此导数据时要对空字符进行判断，如果为NULL或空字符，需要把它改成一个空格的字符串。<br>MySQL中null值与空字符串不等价，而在Oracle中，两者是等价的；在MySQL中匹配null和空字符串是不同的：null查询用 is null/ is not null,空字符''查询用 =''/<>'' |



##DDL(Data Definition Language)

* **建表语句**

| Oracle                                                       | MySql                                                        |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| create table `_table_name`(<br/>`column1` varchar2(10) primary key,<br/>`column2` number(5) not null,<br/>memo  varchar2(100)  );<br/>comment on column `column1`is '这是column1的注释'; | create table `_table_name` ( <br/>`column1`   VARCHAR(32) primary key COMMENT '注释',<br/>`column2` VARCHAR(30) not null COMMENT '注释', <br/>PRIMARY KEY (`column1`)  )<br/>ENGINE=InnoDB  DEFAULT CHARSET=utf8; |



* **字段新增语句**

  * Oracle

    alter table `表名` add `字段` `数据类型`;

    alter table `表名` add (`字段` `数据类型`);

    alter table `表名` add (`字段1` `数据类型`, `字段2` `数据类型`); 

  * MySql

    alter table `表名` add column `字段` `数据类型`;

    alter table `表名` add column `字段1` `数据类型`, add column `字段2` `数据类型`;    

    注：其中关键字column可有可无。



* ** 字段修改语句**

  * Oracle

    alter table `表名` rename column `原字段` to `新字段`;

    注：不能有字段类型         

    * 修改字段类型：alter table `表名` modify(`字段` `数据类型` `约束条件`);

  * MySql

    alter table `表名` change column `原字段` `新字段` `字段类型(必须)`;

    * 修改字段类型：alter table `表名` modify column `字段名` `类型`; 

    

    <strong>PS:但是当有数据时，直接修改列类型都可能对数据造成丢失等，所以一般需要结合具体的业务来对列数据做处理后，再修改列类型类型。所以修改列的类型并非使用SQL语句进行一步到位的修改，而是通过以下流程：</strong>

  * A. 添加临时列     

  * B. 将需要更改的列的值经过类型转换的验证后，赋值给临时列    

  *  C. 删除原有列    

  *  D. 将临时列的列名修改为原有列列名



* **索引变更语句**

  * Oracle

     -- 主键索引

     alter table `_table_name` add constraint `index_name` primary key (`column_name`) using index tablespace `URMSPK`;

    -- 唯一索引

      create **unique index** `unique_index01` on `search_result_tmp` ( `deal_id` , `compare_flag` );

     -- 普通列索引

     create index `index_name$cl2` on `_table_name` (`column1_name`,`column2_name` DESC) tablespace `URMSIDX`;

     -- 删除索引

     drop index `index_name`;

  

  * MySql

     -- 普通索引

     ALTER TABLE `_table_name` ADD INDEX `index_name`  (`column1_name`,`column2_name` DESC);

     -- 唯一索引

     ALTER TABLE `_table_name` ADD UNIQUE `index_name`  (`column_list`);

     -- 主键索引

     ALTER TABLE `_table_name` ADD PRIMARY KEY `index_name`  (`column_list`);

     -- 删除索引

     ALTER TABLE `_table_name` DROP INDEX `index_name`;

    

* **修改表名**

  * Oracle

     alter table `_table_name1` rename to `_table_name2`;

  * MySql

    ALTER TABLE `_table_name1` RENAME TO `_table_name2`;

    

* **删除表语句**

  * Oracle(注：Oracle没有if exists关键字，也没用类似if exists的SQL语法 )

     drop table `table_name`;

  * MySql

    DROP TABLE `table_name` ;

    DROP TABLE IF EXISTS `table_name` ;

    

    

##DML

* **插入语句**

  * Oracle

     insert into `_table_name` ( `column_list` )  values  (  `value_list`  );

  * MySql   (可以插入多条记录)

    INSERT INTO `UMFRAMESET` (`column_list`) VALUES (`value_list`),(`value_list2`);

* **查询语句（同）**

  select `column_name|*` from `_table_name` [where `条件`] ;

* **删除语句（同）**

  delete from `_table_name` [where `条件`] ;



##函数区别

| 编号 | 类别                                                    | ORACLE                                                       | MYSQL                                                        | 注释                                                         |
| ---- | ------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1    | 数字函数                                                | round(1.23456,4)                                             | round(1.23456,4)                                             | 一样： ORACLE：select round(1.23456,4) value from dual MYSQL：select round(1.23456,4) value |
| 2    | abs(-1)                                                 | abs(-1)                                                      | 功能: 将当前数据取绝对值 用法: oracle和mysql用法一样 mysql: select abs(-1) value oracle: select abs(-1) value from dual |                                                              |
| 3    | ceil(-1.001))                                           | ceiling(-1.001)                                              | 功能: 返回不小于 X 的最小整数 用法: mysqls: select ceiling(-1.001) value oracle: select ceil(-1.001) value from dual |                                                              |
| 4    | floor(-1.001)                                           | floor(-1.001)                                                | 功能: 返回不大于 X 的最大整数值 用法: mysql: select floor(-1.001) value oracle: select floor(-1.001) value from dual |                                                              |
| 5    | Max(expr)/Min(expr)                                     | Max(expr)/Min(expr)                                          | 功能:返回 expr 的最小或最大值。MIN() 和 MAX() 可以接受一个字符串参数；在这 种情况下，它们将返回最小或最大的字符串传下。 用法:  ROACLE: select max(user_int_key) from sd_usr; MYSQL: select max(user_int_key) from sd_usr; |                                                              |
| 6    | 字符串函数                                              | ascii(str)                                                   | ascii(str)                                                   | 功能:返回字符串 str 最左边的那个字符的 ASCII 码值。如果 str 是一个空字符串， 那么返回值为 0。如果 str 是一个 NULL，返回值也是 NULL. 用法:  mysql:select ascii('a') value oracle:select ascii('a') value from dual |
| 7    | CHAR(N,...)                                             | CHAR(N,...)                                                  | 功能:CHAR() 以整数类型解释参数，返回这个整数所代表的 ASCII 码值给出的字符 组成的字符串。NULL 值将被忽略. 用法:  mysql:select char(97) value oracle:select chr(97) value from dual |                                                              |
| 8    | REPLACE(str,from_str,to_str)                            | REPLACE(str,from_str,to_str)                                 | 功能: 在字符串 str 中所有出现的字符串 from_str 均被 to_str 替换，然后返回这个字符串. 用法:  mysql: SELECT REPLACE('abcdef', 'bcd', 'ijklmn') value  oracle: SELECT Replace('abcdef', 'bcd', 'ijklmn') value from dual |                                                              |
| 9    | INSTR('sdsq','s',2)                                     | INSTR('sdsq','s')                                            | 参数个数不同 ORACLE: select INSTR('sdsq','s',2) value from dual（要求从位置2开始） MYSQL: select INSTR('sdsq','s') value（从默认的位置1开始） |                                                              |
| 10   | SUBSTR('abcd',2,2)                                      | substring('abcd',2,2)                                        | 函数名称不同： ORACLE: select substr('abcd',2,2) value from dual MYSQL: select substring('abcd',2,2) value |                                                              |
| 11   | instr(‘abcdefg’,’ab’)                                   | locate(‘ab’,’abcdefg’)                                       | 函数名称不同： instr -> locate（注意：locate的子串和总串的位置要互换） ORACLE: SELECT instr('abcdefg', 'ab') VALUE FROM DUAL MYSQL: SELECT locate('ab', 'abcdefg') VALUE |                                                              |
| 12   | length（str）                                           | char_length()                                                | 函数名称不同： ORACEL: SELECT length('AAAASDF') VALUE FROM DUAL MYSQL: SELECT char_length('AAAASDF') VALUE |                                                              |
| 13   | REPLACE('abcdef', 'bcd', 'ijklmn')                      | REPLACE('abcdef', 'bcd', 'ijklmn')                           | 一样： ORACLE: SELECT REPLACE('abcdef', 'bcd', 'ijklmn') value from dual MYSQL: SELECT REPLACE('abcdef', 'bcd', 'ijklmn') value |                                                              |
| 14   | LPAD('abcd',14, '0')                                    | LPAD('abcd',14, '0')                                         | 一样： ORACLE: select LPAD('abcd',14, '0') value from dual MYSQL: select LPAD('abcd',14, '0') value from dual |                                                              |
| 15   | UPPER(iv_user_id)                                       | UPPER(iv_user_id)                                            | 一样： ORACLE: select UPPER(user_id) from sd_usr; MYSQL: select UPPER(user_id) from sd_usr; |                                                              |
| 16   | LOWER(iv_user_id)                                       | LOWER(iv_user_id)                                            | 一样： ORACLE: select LOWER(user_id) from sd_usr; MYSQL: select LOWER(user_id) from sd_usr; |                                                              |
| 17   | 控制流函数                                              | nvl(u.email_address, 10)                                     | IFNULL(u.email_address, 10) 或 ISNULL(u.email_address)       | 函数名称不同（根据不同的作用进行选择）： ORACLE: select u.email_address, nvl(u.email_address, 10) value from sd_usr u (如果u.email_address=NULl,就在DB中用10替换其值) MYSQL: select u.email_address, IFNULL(u.email_address, 10) value from sd_usr u(如果u.email_address=NULl,显示结果中是10，而不是在DB中用10替换其值) select u.email_address, ISNULL(u.email_address) value from sd_usr u（如果u.email_address是NULL, 就显示1<true>,否则就显示0<false>） |
| 18   | DECODE(iv_sr_status,g_sr_status_com, ld_sys_date, NULL) | 无，请用IF或CASE语句代替. IF语句格式:(expr1,expr2,expr3)     | 说明:  1. decode(条件,值1,翻译值1,值2,翻译值2,...值n,翻译值n,缺省值) 该函数的含义如下： IF 条件=值1 THEN 　　　　RETURN(翻译值1) ELSIF 条件=值2 THEN 　　　　RETURN(翻译值2) 　　　　...... ELSIF 条件=值n THEN 　　　　RETURN(翻译值n) ELSE 　　　　RETURN(缺省值) END IF  2. mysql If语法说明 功能: 如果 expr1 是TRUE (expr1 <> 0 and expr1 <> NULL)，则IF()的返回值为expr2; 否则返回值则为 expr3。IF() 的返回值为数字值或字符串值，具体情况视其所在 语境而定。 用法:  mysql: SELECT IF(1>2,2,3); |                                                              |
| 19   | 类型转换函数                                            | TO_CHAR(SQLCODE)                                             | date_format/ time_format                                     | 函数名称不同 SQL> select to_char(sysdate,'yyyy-mm-dd') from dual; SQL> select to_char(sysdate,'hh24-mi-ss') from dual; mysql> select date_format(now(),'%Y-%m-%d'); mysql> select time_format(now(),'%H-%i-%S'); |
| 20   | to_date(str,format)                                     | STR_TO_DATE(str,format)                                      | 函数名称不同： ORACLE:SELECT to_date('2009-3-6','yyyy-mm-dd') VAULE FROM DUAL MYSQL: SELECT STR_TO_DATE('2004-03-01', '%Y-%m-%d') VAULE |                                                              |
| 21   | trunc(-1.002)                                           | cast(-1.002 as SIGNED)                                       | 函数名称不同： TRUNC函数为指定元素而截去的日期值。 ORACLE： select trunc(-1.002) value from dual MYSQL：select cast(-1.002 as SIGNED) value MYSQL： 字符集转换 :  CONVERT(xxx USING  gb2312) 类型转换和SQL Server一样,就是类型参数有点点不同 : CAST(xxx AS  类型) ,  CONVERT(xxx,类型)，类型必须用下列的类型：    可用的类型　   二进制,同带binary前缀的效果 : BINARY    字符型,可带参数 : CHAR()    日期 : DATE    时间: TIME    日期时间型 : DATETIME    浮点数 : DECIMAL     整数 : SIGNED    无符号整数 : UNSIGNED |                                                              |
| 22   | TO_NUMBER(str)                                          | CAST("123" AS SIGNED INTEGER)                                | 函数名称不同 ORACLE:SELECT TO_NUMBER('123') AS VALUE FROM DUAL; MYSQL: SELECT CAST("123" AS SIGNED INTEGER) as value; SIGNED INTEGER:带符号的整形 |                                                              |
| 23   | 日期函数                                                | SYSDATE                                                      | now() / SYSDATE()                                            | 写法不同： ORACLE:select SYSDATE value from dual MYSQL:select now() value select sysdate() value |
| 24   | Next_day(sysdate,7)                                     | 自定义一个函数:F_COMMON_NEXT_DAY(date,int)                   | 函数名称不同： ORACLE: SELECT Next_day(sysdate,7) value FROM DUAL MYSQL: SELECT F_COMMON_NEXT_DAY(SYSDATE(), 3) value from DUAL; (3:指星期的索引值)返回的指定的紧接着下一个星期的日期 |                                                              |
| 25   | ADD_MONTHS(sysdate, 2)                                  | DATE_ADD(sysdate(), interval 2 month)                        | 函数名称不同: ORACLE: SELECT ADD_MONTHS(sysdate, 2) as value from DUAL; MYSQL: SELECT DATE_ADD(sysdate(), interval 2 month) as value from DUAL; |                                                              |
| 26   | 2个日期相减(D1-D2)                                      | DATEDIFF(date1,date2)                                        | 功能: 返回两个日期之间的天数。 用法: mysql: SELECT DATEDIFF('2008-12-30','2008-12-29') AS DiffDate oracle: 直接用两个日期相减（比如d1-d2=12.3） |                                                              |
| 27   | SQL函数                                                 | SQLCODE                                                      | MYSQL中没有对应的函数，但JAVA中SQLException。getErrorCode()函数可以获取错误号 | Oracle内置函数SQLCODE和SQLERRM是特别用在OTHERS处理器中，分别用来返回Oracle的错误代码和错误消息。 MYSQL: 可以从JAVA中得到错误代码，错误状态和错误消息 |
| 28   | SQLERRM                                                 | MYSQL中没有对应的函数，但JAVA中SQLException。getMessage()函数可以获取错误消息 | Oracle内置函数SQLCODE和SQLERRM是特别用在OTHERS处理器中，分别用来返回Oracle的错误代码和错误消息。 MYSQL: 可以从JAVA中得到错误代码，错误状态和错误消息 |                                                              |
| 29   | SEQ_BK_DTL_OPT_INT_KEY.NEXTVAL                          | 自动增长列                                                   | 在MYSQL中是自动增长列. 如下方法获取最新ID:  START TRANSACTION;     INSERT INTO user(username,password)    VALUES (username,MD5(password));   SELECT LAST_INSERT_ID() INTO id;  COMMIT; |                                                              |
| 30   | SUM(enable_flag)                                        | SUM(enable_flag)                                             | 一样： ORCALE： SELECT SUM(enable_flag) FROM SD_USR; MYSQL： SELECT SUM(enable_flag) FROM SD_USR; |                                                              |
| 31   | DBMS_OUTPUT.PUT_LINE(SQLCODE)                           | 在MYSQL中无相应的方法，其作用是在控制台中打印，用于测试，对迁移无影响。 | dbms_output.put_line每行只能显示255个字符，超过了就会报错    |                                                              |



参考：https://blog.csdn.net/huangyinzhao/article/details/80739291

​			https://www.cnblogs.com/yanyunpiaomaio/p/10821437.html





## MySQL-DECIMAL

MySQL `DECIMAL`数据类型用于在数据库中存储精确的数值。我们经常将`DECIMAL`数据类型用于保留准确精确度的列，例如会计系统中的货币数据。

要定义数据类型为`DECIMAL`的列，请使用以下语法：

```
column_name ``DECIMAL``(P,D);
```

在上面的语法中：

- `P`是表示有效数字数的精度。 `P`范围为`1〜65`。
- `D`是表示小数点后的位数。 `D`的范围是`0`~`30`。MySQL要求`D`小于或等于(`<=`)`P`。

`DECIMAL(P，D)`表示列可以存储`D`位小数的`P`位数。十进制列的实际范围取决于精度和刻度。

与INT数据类型一样，`DECIMAL`类型也具有`UNSIGNED`和`ZEROFILL`属性。 如果使用`UNSIGNED`属性，则`DECIMAL UNSIGNED`的列将不接受负值。

如果使用`ZEROFILL`，MySQL将把显示值填充到`0`以显示由列定义指定的宽度。 另外，如果我们对`DECIMAL`列使用`ZERO FILL`，MySQL将自动将`UNSIGNED`属性添加到列。

以下示例使用`DECIMAL`数据类型定义的一个叫作`amount`的列。



存储的空间大小，整数部分的占用空间如下：

| Leftover Digits | Number of Bytes |
| --------------- | --------------- |
| 0               | 0               |
| 1–2             | 1               |
| 3–4             | 2               |
| 5–6             | 3               |
| 7–9             | 4               |

占用字节数计算方法 —— 小数和整数分别计算，每9位数占4字节

例如：`DECIMAL(20,6)`，整数14位，需要4字节存9位，还需3字节存5位；小数6位，需3字节。共10bytes

感谢：

https://www.cnblogs.com/erisen/p/5970315.html

https://www.cnblogs.com/owenma/p/7097602.html







## MySQL-小技巧

1.比较运算符能用"!="就不用"<>"：
"!="增加了索引的使用几率。
2.明知只有一条查询结果，那就使用"LIMIT 1"：
"LIMIT 1"可以避免全表扫面，找到对应结果就不会再继续扫描了。
3.为列选择合适的数据类型
能用TINYINT就不用SMALLINT，能用SMALLINE就不用INT，道理你懂得，磁盘和内存消耗越小越好嘛。
4.将大的DELETE,UPDATE or INSERT查询变成多个小查询
能写一个几十行、几百行的SQL语句是不是显得逼格很高？然而，为了达到更好的性能以及更好的数据控制，应将它们写成多个小查询。
5.使用UNION ALL 代替 UNION,如果结果集允许重复的话。因为UNINON ALL不去重，效率高于UNION.
6.为获得相同结果集的多次执行，请保持SQL语句前后一致。这样做的目的是为了充分利用查询缓冲。
比如根据地域和产品ID查询产品价格，第一次使用了：
SELECT price FROM order WHERE id='123456' and region='BEIJING'
那么第二次同样的查询，请保持以上语句的一致性，比如不要将where语句里面的id和region位置调换顺序。
7.尽量避免使用"SELECT \*"
如果不查询表中的所有的列，尽量避免使用SELECT \* ，因为它会进行全表扫描，不能有效利用索引，增大了数据库服务器的负担，以及它与应用程序客户端之间的网络IO开销。
8.WHERE子句里面的列尽量被索引
只是”尽量“，并不是所有的列。因地制宜，根据实际情况进行调整，因为有时索引太多也会降低性能。
9.JOIN子句里面的列尽量被索引。同样只是”尽量“，并不是说所有的列。
10.ORDER BY 的列尽量被索引。OEDER BY的列如果被索引，性能也会更好。
11.使用LIMIT实现分页逻辑。不仅提高了性能，同时减少了不必要的数据库和应用间的网络传输。
12.使用EXPLAIN关键字去查看执行计划。EXPLAIN可以检查索引使用情况以及扫描的行。
总结：SQL调优方法很多，同样的查询结果可以有很多种不同的查询方式。其实最好的方法就是在开发环境中用最贴近真实的数据集和硬件环境进行测试，然后再发布到生产环境中。

https://www.yisu.com/zixun/26921.html







# Oracle 语句学习



## DECODE COMMAND

The syntax for the DECODE function in Oracle/PLSQL is:

```plsql
DECODE( expression , search , result [, search , result]... [, default] )
```

参考：https://www.techonthenet.com/oracle/functions/decode.php



##导入数据

```plsql
imp test/test@fealm97 file=F:\Oracle.dmp tables=lsetlist ignore=y


imp system/manager buffer=102400000  \
ignore=y tables=SP_WH_GWG1_17S_1S \
fromuser=CPXOA_GDWHT touser=CPXOA_GDWHT \
file=g:\1.dmp \
log=G:\20150630.sp_wh_gwg1_17_1s.log
```

impdp只能对应expdp导出的命令文件

1、directory需要 `create` 并 dmp文件放入其中；

2、权限必须要存在；

3、对应的导出的表空间也必须存在。 



## 导出数据

```plsql
exp ${user}/${password}@${url} file=${filename}.dmp log=${filelogname}.log tables=${table1},${table2}
```







### Oracle

# [NLSSORT](https://www.cnblogs.com/zhaochunyi/p/10898644.html)

排序语句学习：

```sql
SELECT *
  FROM test
  ORDER BY NLSSORT(name, 'NLS_SORT = XDanish');
```

支持中文排序，9i后增加

Oracle 9i开始，新增了按照拼音、部首、笔画排序功能。

  通过设置NSL_SORT值来实现:

  **SCHINESE_RADICAL_M** 按照部首（第一顺序）、笔划（第二顺序）排序

  **SCHINESE_STROKE_M** 按照笔划（第一顺序）、部首（第二顺序）排序

  **SCHINESE_PINYIN_M** 按照拼音排序



实现中文排序有两种常见方式:

1. **session级**

   **ALTER SESSION SET NLS_SORT='XXX';**

   此结果影响整个session。

2. **sql级**

   **SELECT \* FROM TABLE_XXX ORDER BY NLSSORT(字段名, 'NLS_SORT=XXX');**

https://blog.51cto.com/baser/2139625





# 建立用户

关闭密码校验：alter profile default limit PASSWORD_VERIFY_FUNCTION null;

```sql
--建立用户：

create user `username` identified by ` password`;

alter user `username` identified by  `password` ; 

--建立用户并制定表空间
create user pow identified by pow
default tablespace DATA_SPACE – 指定数据表空间用户
temporary tablespace TEMP_SPACE; – 指定临时表空间用户


--授予DBA权限： 
grant connect,resource,dba to `username`;

--撤销DBA权限：
revoke connect, resource from `username`;

--断开所有用户链接
select username,sid,serial# from v$session where username = 'JEECG_TEST';
alter system kill session '#{sid},#{serial}';  

--及联删除 cascade；
drop user jeecg_test cascade;
```





重启oracle：

```sql
shutdown immediate ;

startup;
```

参考：

https://blog.csdn.net/songyundong1993/article/details/81093335



几个容易混乱的数据库参数概念：

数据库名：db_name      

数据库实例名：instance_name 

操作系统环境变量：oracle_sid 

数据库服务名：service_names 

数据库域名：db_domain     

全局数据库名：global_db_name 

https://www.cnblogs.com/jichunhu/p/4063175.html



疑问：PL/SQL是否会保存连接的登录缓存呢？



###登录方式：

sqlplus 用户名/密码@主机:端口号/SID 可选as sysdba 

```
racdb =
   (DESCRIPTION =
     (ADDRESS = (PROTOCOL = TCP)(HOST = 10.16.35.62)(PORT = 1521))
     (ADDRESS = (PROTOCOL = TCP)(HOST = 10.16.35.63)(PORT = 1521))
     (LOAD_BALANCE = yes)      //开启负载
     (CONNECT_DATA =
       (SERVER = DEDICATED)		//专用服务器，一个客户端连接对应一个服务器进程
       (SERVICE_NAME = racdb)   //对应数据库的服务名
       (FAILOVER_MODE =      //连接失败后处理的方式 
            (TYPE = session) //TYPE =SESSION表示当一个连接好的会话的实例发生故障，系统会自动将会话切换到其他可用的实例，前台应用无须再度发起连接，但会话正在执行的SQL 需要重新执行
            (METHOD = basic) //表示初始连接就连接一个接点 
            (RETRIES = 180)  //连接失败后重试连接的次数 
            (DELAY = 5)    //连接失败后重试的延迟时间(以秒为单位)
       )
     )
 )
```



###数据迁移的基本步骤 - 实战

oracle中接受参数&1

问题：&1.的接收参数和&1一样的原因

解答：https://stackoverflow.com/questions/17051928/what-does-1-mean-in-oracle-database





##Oracle 语句create table as 与create table like 的区别

##### 相同点：

​	都是创建一个新表

##### 不同点

- create table as 只是复制原数据，其实就是把查询的结果建一个表
- create table like 产生与源表相同的表结构，包括索引和主键，数据需要用insert into 语句复制进去。例如：

```sql
create table newtest like test；
insert into newtest select * from test；
```

