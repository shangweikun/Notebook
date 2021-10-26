## <a href='https://oracle-base.com/articles/10g/oracle-data-pump-10g'>Help</a>

The `HELP=Y` option displays the available parameters. The following output comes from 18c, but is edited to include the database version when the parameter was introduced.



### expdp

**ABORT_STEP (12.1)**

> Stop the job after it is initialized or at the indicated object.
> Valid values are -1 or N where N is zero or greater.
> N corresponds to the object's process order number in the master table.





**ATTACH (10.1)**

* 该选项用于在客户会话与已存在导出作业之间建立关联；

* ⚠️注意：如果使用 ATTACH 选项,在命令行除了连接字符串和 ATTACH 选项外,不能指定任何其他选项。

>Attach to an existing job.
>For example, ATTACH=job_name.

ex:

```shell
expdp ${user}/${password}  ATTACH=${user}_${date}.export_job
```





**ACCESS_METHOD (12.1)**

> Instructs Export to use a particular method to unload data.
> Valid keyword values are: [AUTOMATIC], DIRECT_PATH and EXTERNAL_TABLE.





**CLUSTER (11.2)**

> Utilize cluster resources and distribute workers across the Oracle RAC [YES].





**COMPRESSION (10.2)**

> Reduce the size of a dump file.
> Valid keyword values are: ALL, DATA_ONLY, [METADATA_ONLY] and NONE.





**COMPRESSION_ALGORITHM (12.1)**

> Specify the compression algorithm that should be used.
> Valid keyword values are: [BASIC], LOW, MEDIUM and HIGH.





**CONTENT (10.1)**

* 该选项用于指定要导出的内容.默认值为 ALL ；
* 当设置 CONTENT 为 ALL 时,将导出对象定义及其所有数据.为 DATA_ONLY 时,只导出对象数据, 为 METADATA_ONLY 时。

```shell
CONTENT={ALL | DATA_ONLY | METADATA_ONLY} 
```

> Specifies data to unload.
> Valid keyword values are: [ALL], DATA_ONLY and METADATA_ONLY.

ex:

```shell
Expdp ${user]/${password} DIRECTORY=dump DUMPFILE=a.dump CONTENT=METADATA_ONLY 
```





**DATA_OPTIONS (11.1)**

> Data layer option flags.
> Valid keyword values are: GROUP_PARTITION_TABLE_DATA, VERIFY_STREAM_FORMAT and XML_CLOBS.



**DIRECTORY (12.2)**

* 指定转储文件和日志文件所在的目录；
* Directory_object 用于指定目录对象名称.需要注意,目录对象是使用 CREATE DIRECTORY 语句建立的对象,而不是 OS 目录。

> Directory object to be used for dump and log files.

ex:

```sql
CREATE DIRECTORY dump as '/dump/; 
```

```shell
Expdp scott/tiger DIRECTORY=dump DUMPFILE=a.dump 
```





**DUMPFILE (10.1)**

用于指定转储文件的名称,默认名称为 expdat.dmp

> Specify list of destination dump file names [expdat.dmp].
> For example, DUMPFILE=scott1.dmp, scott2.dmp, dmpdir:scott3.dmp.





**ENCRYPTION (11.1)**

> Encrypt part or all of a dump file.
> Valid keyword values are: ALL, DATA_ONLY, ENCRYPTED_COLUMNS_ONLY, METADATA_ONLY and NONE.





**ENCRYPTION_ALGORITHM (11.1)**

> Specify how encryption should be done.
> Valid keyword values are: [AES128], AES192 and AES256.





**ENCRYPTION_MODE (11.1)**

> Method of generating encryption key.
> Valid keyword values are: DUAL, PASSWORD and [TRANSPARENT].





**ENCRYPTION_PASSWORD (10.2)**

> Password key for creating encrypted data within a dump file.





**ENCRYPTION_PWD_PROMPT (12.1)**

> Specifies whether to prompt for the encryption password [NO].
> Terminal echo will be suppressed while standard input is read.





**ESTIMATE (10.1)**

* 指定估算被导出表所占用磁盘空间的方法.默认值是 BLOCKS；

* 设置为 BLOCKS 时,oracle 会按照目标对象所占用的数据块个数乘以数据块尺寸估算对象占用的空间,设置为 STATISTICS 时,根据最近统计值估算对象占用空间 

```shell
ESTIMATE={BLOCKS | STATISTICS} 
```

> Calculate job estimates.
> Valid keyword values are: [BLOCKS] and STATISTICS.





**ESTIMATE_ONLY (10.1)**

* 指定是否只估算导出作业所占用的磁盘空间,默认值为 N 
* 设置为 Y 时,导出作用只估算对象所占用的磁盘空间,而不会执行导出作业,为 N 时,不仅估算对象所占用的磁盘空间,还会执行导出操作. 

```shell
ESTIMATE_ONLY={Y | N} 
```

> Calculate job estimates without performing the export [NO].

ex:

```shell
Expdp ${user}/${password} ESTIMATE_ONLY=y NOLOGFILE=y 
```





**EXCLUDE (10.1)**

* 该选项用于指定执行操作时释放要排除对象类型或相关对象 
* Object_type 用 于 指 定 要 排 除 的 对 象 类 型 ,name_clause 用 于 指 定 要 排 除 的 具 体 对 象 . 
* ⚠️注意：EXCLUDE 和 INCLUDE 不能同时使用 

> Exclude specific object types.
> For example, EXCLUDE=SCHEMA:"='HR'".





**FILESIZE (10.1)**

* 指定导出文件的最大尺寸,默认为 0,(表示文件尺寸没有限制) 

> Specify the size of each dump file in units of bytes.





**FLASHBACK_SCN (10.1)**

* 指定导出特定 SCN 时刻的表数据 

> SCN used to reset session snapshot.





**FLASHBACK_TIME**

* 指定导出特定时间点的表数据 

> Time used to find the closest corresponding SCN value.

ex:

```	shell
Expdp ${user}/${user} DIRECTORY=dump DUMPFILE=a.dmp FLASHBACK_TIME="TO_TIMESTAMP('25-08-2004 14:35:00','DD-MM-YYYY HH24:MI:SS')"
```





**FULL (10.1)**

* 指定数据库模式导出,默认为 N  
* FULL={Y | N} 
    为 Y 时,标识执行数据库导出. 

> Export entire database [NO].





**HELP (10.1)**

* 指定是否显示 EXPDP 命令行选项的帮助信息,默认为 N 
* 当设置为 Y 时,会显示导出选项的帮助信息. 

> Display Help messages [NO].

ex:

```shell
Expdp help=y 
```





**INCLUDE (10.1)**

* 指定导出时要包含的对象类型及相关对象 

```shell
INCLUDE = object_type[:name_clause] [,… ] 
```

> Include specific object types.
> For example, INCLUDE=TABLE_DATA.





**JOB_NAME (10.1)**

* 指定导出时要包含的对象类型及相关对象 

```shell
INCLUDE = object_type[:name_clause] [,… ] 
```

> Name of export job to create.





**KEEP_MASTER (12.1)**

> Retain the master table after an export job that completes successfully [NO].





**LOGFILE (10.1)**

* 指定导出日志文件文件的名称,默认名称为 export.log 

> Specify log file name [export.log].





**METRICS (12.1)**

> Report additional job information to the export log file [NO].





**NETWORK_LINK (10.1)**

* 指定数据库链名,如果要将远程数据库对象导出到本地例程的转储文件中,必须设置该选项. 

> Name of remote database link to the source system.





**NOLOGFILE (10.1)**

* 该选项用于指定禁止生成导出日志文件,默认值为 N. 

> Do not write log file [NO].





**PARALLEL (10.1)**

* 指定执行导出操作的并行进程个数,默认值为 1 

> Change the number of active workers for current job.





**PARFILE (10.1)**

* 指定导出参数文件的名称 
    PARFILE=[directory_path] file_name 

> Specify parameter file name.





**QUERY (10.1)**

* 用于指定过滤导出数据的 where 条件 
    QUERY=[schema.] [table_name:] query_clause 
* ⚠️注意：Schema 用于指定方案名,table_name 用于指定表名,query_clause 用于指定条件限制子句.QUERY 选项不能与 CONNECT=METADATA_ONLY,ESTIMATE_ONLY,TRANSPORT_TABLESPACES等选项同时使用. 

> Predicate clause used to export a subset of a table.
> For example, QUERY=employees:"WHERE department_id > 10".





**REMAP_DATA (11.1)**

> Specify a data conversion function.
> For example, REMAP_DATA=EMP.EMPNO:REMAPPKG.EMPNO.





**REUSE_DUMPFILES (11.1)**

> Overwrite destination dump file if it exists [NO].





**SAMPLE (10.2)**

> Percentage of data to be exported.





**SCHEMAS (10.1)**

* 该方案用于指定执行方案模式导出,默认为当前用户方案. 

> List of schemas to export [login schema].





**SERVICE_NAME (11.2)**

> Name of an active Service and associated resource group to constrain Oracle RAC resources.





**SOURCE_EDITION (11.2)**

> Edition to be used for extracting metadata.





**STATUS (10.1)**

* 指定显示导出作业进程的详细状态,默认值为 0 

> Frequency (secs) job status is to be monitored where
> the default [0] will show new status when available.





**TABLES (10.1)**

* 指定表模式导出 
    TABLES=[schema_name.]table_name\[:partition_name][,…] 

> Identifies a list of tables to export.
> For example, TABLES=HR.EMPLOYEES,SH.SALES:SALES_1995.





**TABLESPACES (10.1)**

* 指定要导出表空间列表 

> Identifies a list of tablespaces to export.





**TRANSPORTABLE (12.1)**

> Specify whether transportable method can be used.
> Valid keyword values are: ALWAYS and [NEVER].





**TRANSPORT_FULL_CHECK (10.1)**

* 该选项用于指定被搬移表空间和未搬移表空间关联关系的检查方式,默认为 N. 
    * 当 设置为 Y 时,导出作业会检查表空间直接的完整关联关系,如果表所在表空间或其索引所在的表空间只有一个表空间被搬移,将显示错误信息. 
    * 当设置为N 时, 导出作业只检查单端依赖,如果搬移索引所在表空间,但未搬移表所在表空间,将显示出错信息,如果搬移表所在表空间,未搬移索引所在表空间,则不会显示错误信息. 

> Verify storage segments of all tables [NO].





**TRANSPORT_TABLESPACES (10.1)**

* 指定执行表空间模式导出 

> List of tablespaces from which metadata will be unloaded.





**VERSION (10.1)**

* 指定被导出对象的数据库版本,默认值为 COMPATIBLE. 
    VERSION={COMPATIBLE | LATEST | version_string} 
    * 为 COMPATIBLE 时,会根据初始化参数 COMPATIBLE 生成对象元数据;
    * 为 LATEST 时,会根据数据库的实际版本生成对象元数据.
    * version_string 用于指定数据库版本字符串. 

> Version of objects to export.
> Valid keyword values are: [COMPATIBLE], LATEST or any valid database version.





**VIEWS_AS_TABLES (12.1)**

> Identifies one or more views to be exported as tables.
> For example, VIEWS_AS_TABLES=HR.EMP_DETAILS_VIEW.



```shell
$ expdp help=y

Export: Release 18.0.0.0.0 - Production on Mon Jan 28 08:31:55 2019
Version 18.3.0.0.0

Copyright (c) 1982, 2018, Oracle and/or its affiliates.  All rights reserved.


The Data Pump export utility provides a mechanism for transferring data objects
between Oracle databases. The utility is invoked with the following command:

   Example: expdp scott/tiger DIRECTORY=dmpdir DUMPFILE=scott.dmp

You can control how Export runs by entering the 'expdp' command followed
by various parameters. To specify parameters, you use keywords:

   Format:  expdp KEYWORD=value or KEYWORD=(value1,value2,...,valueN)
   Example: expdp scott/tiger DUMPFILE=scott.dmp DIRECTORY=dmpdir SCHEMAS=scott
               or TABLES=(T1:P1,T1:P2), if T1 is partitioned table

USERID must be the first parameter on the command line.

------------------------------------------------------------------------------

The available keywords and their descriptions follow. Default values are listed within square brackets.
































------------------------------------------------------------------------------

The following commands are valid while in interactive mode.
Note: abbreviations are allowed.

ADD_FILE
Add dumpfile to dumpfile set.

CONTINUE_CLIENT
Return to logging mode. Job will be restarted if idle.

EXIT_CLIENT
Quit client session and leave job running.

FILESIZE
Default filesize (bytes) for subsequent ADD_FILE commands.

HELP
Summarize interactive commands.

KILL_JOB
Detach and delete job.

PARALLEL
Change the number of active workers for current job.

REUSE_DUMPFILES
Overwrite destination dump file if it exists [NO].

START_JOB
Start or resume current job.
Valid keyword values are: SKIP_CURRENT.

STATUS
Frequency (secs) job status is to be monitored where
the default [0] will show new status when available.

STOP_JOB
Orderly shutdown of job execution and exits the client.
Valid keyword values are: IMMEDIATE.

STOP_WORKER
Stops a hung or stuck worker.

TRACE
Set trace/debug flags for the current job.

$
```



### impdp

```shell
$ impdp help=y

Import: Release 18.0.0.0.0 - Production on Mon Jan 28 08:44:08 2019
Version 18.3.0.0.0

Copyright (c) 1982, 2018, Oracle and/or its affiliates.  All rights reserved.


The Data Pump Import utility provides a mechanism for transferring data objects
between Oracle databases. The utility is invoked with the following command:

     Example: impdp scott/tiger DIRECTORY=dmpdir DUMPFILE=scott.dmp

You can control how Import runs by entering the 'impdp' command followed
by various parameters. To specify parameters, you use keywords:

     Format:  impdp KEYWORD=value or KEYWORD=(value1,value2,...,valueN)
     Example: impdp scott/tiger DIRECTORY=dmpdir DUMPFILE=scott.dmp

USERID must be the first parameter on the command line.

------------------------------------------------------------------------------

The available keywords and their descriptions follow. Default values are listed within square brackets.

ABORT_STEP (12.1)
Stop the job after it is initialized or at the indicated object.
Valid values are -1 or N where N is zero or greater.
N corresponds to the object's process order number in the master table.

ACCESS_METHOD (12.1)
Instructs Import to use a particular method to load data.
Valid keyword values are: [AUTOMATIC], CONVENTIONAL, DIRECT_PATH,
EXTERNAL_TABLE, and INSERT_AS_SELECT.

ATTACH (10.1)
Attach to an existing job.
For example, ATTACH=job_name.

CLUSTER (11.2)
Utilize cluster resources and distribute workers across the Oracle RAC [YES].

CONTENT (10.1)
Specifies data to load.
Valid keywords are: [ALL], DATA_ONLY and METADATA_ONLY.

DATA_OPTIONS (11.1)
Data layer option flags.
Valid keywords are: DISABLE_APPEND_HINT, ENABLE_NETWORK_COMPRESSION,
REJECT_ROWS_WITH_REPL_CHAR, SKIP_CONSTRAINT_ERRORS, CONTINUE_LOAD_ON_FORMAT_ERROR,
TRUST_EXISTING_TABLE_PARTITIONS and VALIDATE_TABLE_DATA.

DIRECTORY (10.1)
Directory object to be used for dump, log and SQL files.

DUMPFILE (10.1)
List of dump files to import from [expdat.dmp].
For example, DUMPFILE=scott1.dmp, scott2.dmp, dmpdir:scott3.dmp.

ENCRYPTION_PASSWORD (10.2)
Password key for accessing encrypted data within a dump file.
Not valid for network import jobs.

ENCRYPTION_PWD_PROMPT (12.1)
Specifies whether to prompt for the encryption password [NO].
Terminal echo is suppressed while standard input is read.

ESTIMATE (10.1)
Calculate network job estimates.
Valid keywords are: [BLOCKS] and STATISTICS.

EXCLUDE (10.1)
Exclude specific object types.
For example, EXCLUDE=SCHEMA:"='HR'".

FLASHBACK_SCN (10.1)
SCN used to reset session snapshot.

FLASHBACK_TIME (10.1)
Time used to find the closest corresponding SCN value.

FULL (10.1)
Import everything from source [YES].

HELP (10.1)
Display help messages [NO].

INCLUDE (10.1)
Include specific object types.
For example, INCLUDE=TABLE_DATA.

JOB_NAME (10.1)
Name of import job to create.

KEEP_MASTER (12.1)
Retain the master table after an import job that completes successfully [NO].

LOGFILE (10.1)
Log file name [import.log].

LOGTIME (12.1)
Specifies that messages displayed during import operations be timestamped.
Valid keyword values are: ALL, [NONE], LOGFILE and STATUS.

MASTER_ONLY (12.1)
Import just the master table and then stop the job [NO].

METRICS (12.1)
Report additional job information to the import log file [NO].

NETWORK_LINK (10.1)
Name of remote database link to the source system.

NOLOGFILE (10.1)
Do not write log file [NO].

PARALLEL (10.1)
Change the number of active workers for current job.

PARFILE (10.1)
Specify parameter file.

PARTITION_OPTIONS (11.1)
Specify how partitions should be transformed.
Valid keywords are: DEPARTITION, MERGE and [NONE].

QUERY (10.1)
Predicate clause used to import a subset of a table.
For example, QUERY=employees:"WHERE department_id > 10".

REMAP_DATA (11.1)
Specify a data conversion function.
For example, REMAP_DATA=EMP.EMPNO:REMAPPKG.EMPNO.

REMAP_DATAFILE (10.1)
Redefine data file references in all DDL statements.

REMAP_SCHEMA (10.1)
Objects from one schema are loaded into another schema.

REMAP_TABLE (11.1)
Table names are remapped to another table.
For example, REMAP_TABLE=HR.EMPLOYEES:EMPS.

REMAP_TABLESPACE (10.1)
Tablespace objects are remapped to another tablespace.

REUSE_DATAFILES (10.1)
Tablespace will be initialized if it already exists [NO].

SCHEMAS (10.1)
List of schemas to import.

SERVICE_NAME (11.2)
Name of an active service and associated resource group to constrain Oracle RAC resources.

SKIP_UNUSABLE_INDEXES (10.1)
Skip indexes that were set to the Index Unusable state.

SOURCE_EDITION (11.2)
Edition to be used for extracting metadata.

SQLFILE (10.1)
Write all the SQL DDL to a specified file.

STATUS (10.1)
Frequency (secs) job status is to be monitored where
the default [0] will show new status when available.

STREAMS_CONFIGURATION (10.1)
Enable the loading of Streams metadata [YES].

TABLE_EXISTS_ACTION (10.1)
Action to take if imported object already exists.
Valid keywords are: APPEND, REPLACE, [SKIP] and TRUNCATE.

TABLES (10.1)
Identifies a list of tables to import.
For example, TABLES=HR.EMPLOYEES,SH.SALES:SALES_1995.

TABLESPACES (10.1)
Identifies a list of tablespaces to import.

TARGET_EDITION (11.2)
Edition to be used for loading metadata.

TRANSFORM (10.1)
Metadata transform to apply to applicable objects.
Valid keywords are: DISABLE_ARCHIVE_LOGGING, INMEMORY, INMEMORY_CLAUSE,
LOB_STORAGE, OID, PCTSPACE, SEGMENT_ATTRIBUTES, SEGMENT_CREATION,
STORAGE, and TABLE_COMPRESSION_CLAUSE.

TRANSPORTABLE (12.1)
Options for choosing transportable data movement.
Valid keywords are: ALWAYS and [NEVER].
Only valid in NETWORK_LINK mode import operations.

TRANSPORT_DATAFILES (10.1)
List of data files to be imported by transportable mode.

TRANSPORT_FULL_CHECK (10.1)
Verify storage segments of all tables [NO].
Only valid in NETWORK_LINK mode import operations.

TRANSPORT_TABLESPACES (10.1)
List of tablespaces from which metadata is loaded.
Only valid in NETWORK_LINK mode import operations.

VERSION (10.1)
Version of objects to import.
Valid keywords are: [COMPATIBLE], LATEST, or any valid database version.
Only valid for NETWORK_LINK and SQLFILE.

VIEWS_AS_TABLES (12.1)
Identifies one or more views to be imported as tables.
For example, VIEWS_AS_TABLES=HR.EMP_DETAILS_VIEW.
Note that in network import mode, a table name is appended
to the view name.

------------------------------------------------------------------------------

The following commands are valid while in interactive mode.
Note: abbreviations are allowed.

CONTINUE_CLIENT
Return to logging mode. Job will be restarted if idle.

EXIT_CLIENT
Quit client session and leave job running.

HELP
Summarize interactive commands.

KILL_JOB
Detach and delete job.

PARALLEL
Change the number of active workers for current job.

START_JOB
Start or resume current job.
Valid keywords are: SKIP_CURRENT.

STATUS
Frequency (secs) job status is to be monitored where
the default [0] will show new status when available.

STOP_JOB
Orderly shutdown of job execution and exits the client.
Valid keywords are: IMMEDIATE.

STOP_WORKER
Stops a hung or stuck worker.

TRACE
Set trace/debug flags for the current job.

$
```

