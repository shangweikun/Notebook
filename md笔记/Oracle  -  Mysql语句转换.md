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
| 3    | Date                                                         | DATATIME                                                     | 日期字段的处理 MYSQL日期字段分DATE和TIME两种，ORACLE日期字段只有DATE，包含年月日时分秒信息，用当前数据库的系统时间为 SYSDATE, 精确到秒，或者用字符串转换成日期型函数TO_DATE(‘2001-08-01’,’YYYY-MM-DD’)年-月-日 24小时:分钟:秒的格式YYYY-MM-DD HH24:MI:SS TO_DATE()还有很多种日期格式, 可以参看ORACLE DOC.日期型字段转换成字符串函数TO_CHAR(‘2001-08-01’,’YYYY-MM-DD HH24:MI:SS’)  日期字段的数学运算公式有很大的不同。<br>MYSQL找到离当前时间7天用DATE_FIELD_NAME ＞ SUBDATE（NOW（），INTERVAL 7 DAY）ORACLE找到离当前时间7天用 DATE_FIELD_NAME ＞SYSDATE - 7; <br>MYSQL中插入当前时间的几个函数是：NOW()函数以`'YYYY-MM-DD HH:MM:SS'返回当前的日期时间，可以直接存到DATETIME字段中。<br>CURDATE()以’YYYY-MM-DD’的格式返回今天的日期，可以直接存到DATE字段中。<br>CURTIME()以’HH:MM:SS’的格式返回当前的时间，可以直接存到TIME字段中。<br>例：insert into tablename (fieldname) values (now())  而oracle中当前时间是sysdate |
| 4    | INTEGER                                                      | int / INTEGER                                                | Mysql中INTEGER等价于int                                      |
| 5    | EXCEPTION                                                    | SQLEXCEPTION                                                 | 详见<<2009001-eService-O2MG.doc>>中2.5 Mysql异常处理         |
| 6    | CONSTANT VARCHAR2(1)                                         | mysql中没有CONSTANT关键字                                    | 从ORACLE迁移到MYSQL,所有CONSTANT常量只能定义成变量           |
| 7    | TYPE `g_grp_cur` IS REF CURSOR;                              | 光标 : mysql中有替代方案                                     | 详见<<2009001-eService-O2MG.doc>>中2.2 光标处理              |
| 8    | TYPE `unpacklist_type` IS TABLE OF VARCHAR2(2000) INDEX BY BINARY_INTEGER; | 数组: mysql中借助临时表处理 或者直接写逻辑到相应的代码中， 直接对集合中每个值进行相应的处理 | 详见<<2009001-eService-O2MG.doc>>中2.4 数组处理              |
| 9    | 自动增长的序列                                               | 自动增长的数据类型                                           | MYSQL有自动增长的数据类型，插入记录时不用操作此字段，会自动获得数据值。ORACLE没有自动增长的数据类型，需要建立一个自动增长的序列号，插入记录时要把序列号的下一个值赋于此字段。 |
| 0    | NULL                                                         | NULL                                                         | 空字符的处理 MYSQL的非空字段也有空的内容，ORACLE里定义了非空字段就不容许有空的内容。<br>按MYSQL的NOT NULL来定义ORACLE表结构, 导数据的时候会产生错误。<br>因此导数据时要对空字符进行判断，如果为NULL或空字符，需要把它改成一个空格的字符串。 |



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



* **字段修改语句**

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