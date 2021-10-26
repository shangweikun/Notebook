# 交互式文件上传输脚本



**背景**

由于公司要求，由文件平台传输的文件，只能通过调度平台统一进行调度；

而我们的调度平台原始到只能执行shell脚本；然后就有了一个文件解析的需求；

听到这个消息，一开始我是绝望并想拒绝的，但是看了看负债，默默的又咽了下去。



*开始淦*

------

**思路**

上一篇文章中，制定了基本的方针路线：

`通过sqlLoader方式导入数据库，java程序进行后续的补充操作`

同时领导希望这个shell脚本，有更好的扩展以及易用性，提出了以下几点的要求

*   能与java程序，做到分步进行，相互不影响；
*   一定时间内，轮询扫描就绪文件；

*   通过数据库开关，以达到某些功能控制的权限；

*   希望通过参数，使脚本变得更加具有普适性，通过参数既可以配置不同类型的文件解读；



既然领导已经下了命令，路都给你指好了，淦

-----

**方案**

根据 ***基本方针*** 和 ***领导路线*** 我们指定了以下的功能方案：

1.   在任务执行表中，登记任务开始记录；
2.   设置定时轮询，设置轮询时间，循环确认就绪文件是否已经到位；
3.   通过`find`命令，查找待解析文件；
4.   更新任务执行表：文件获取成功；
5.   执行`sqlldr`命令，对文件进行入库操作；
6.   更新任务执行表，确认任务执行状态，维护任务操作记录；



根据以上方案，在数据库中维护了两张表的信息

*   定时任务信息表

用于记录任务的执行状态；通过数据库中的持久化数据，实现shell脚本及java程序之间的交互；能确保程序的执行顺序同时，还能时刻监控任务的执行情况，做到失败任务监控；

*   数据信息源表

针对不同的业务场景，以及文件内容及文件类型，将文件进行分类；然后建立相应的数据源表，同时提供`sqlldr`所需要的`control`文件；

(交互系统不同，文件的传输内容也一般不同，具体体现在：分隔符，字符类型等等)



淦起来  **Talk is cheap, show me your code**

----

**细节**

*数据库交互* 

一开始，登记 ‘开始解析文件’ ，同时提示容器内的jvm虚拟机，



*   创建一个TASK任务交互表 `DB_SHELL_TASK_INFO` ；
*   主流程的操作顺序如下:

```shell
startParseFileSql

# 等待ok文件[解压tar包]

getFileSuccessSql

#sqlldr 导入数据库信息

if [ $? -eq 0 ] ; then
    scriptParseFileSuccessSql
  else
    scriptParseFileFailSql
  fi
```



抽取其中的 `startParseFileSql` ，具体代码如下：

```shell
startParseFileSql (){
  echo "$(date +'%Y-%m-%d %H:%M:%S'): File start to parse [title: $TASK_TITLE, date: $DATE]" >> $PATH_LOG_FILE
  sqlplus -s "$DB_USERID" <<EOF
set echo off feedback off heading off underline off;
merge INTO EBDCNEW.DB_SHELL_TASK_INFO A
using (
    select nvl((select task_id
                from EBDCNEW.DB_SHELL_TASK_INFO B
                where B.TASK_TITLE = to_char('$TASK_TITLE')
                  and B.TASK_DATE = to_char('$DATE')
               ), 0) as TASK_ID
    from dual
) T
on (T.TASK_ID = A.TASK_ID)
when not matched then
    INSERT (TASK_ID,
            TASK_TITLE,
            TASK_DATE,
            TASK_BEGIN_TIME,
            STATUS,
            STATUS_TXT)
    values (SEQ_DSTI_TASK_ID.nextval, to_char('$TASK_TITLE'), to_char('$DATE'),
            sysdate, '00', 'task start;')
/
commit
/
EOF
}
```

其中

`task_id`  唯一主键id

`task_title` 任务的标题

`task_date` 任务日期

`STATUS` : 该字段信息，表述文件状态；通过不同的枚举，确定文件当前的所属状态；

*   用于与JAVA程序轮询标识
    *   （当文件处于`02`表明：shell脚本执行成功，等待java程序调度）；
*   可以提供数据库监控，以监控文件异常状态；





*文件检查及清理*

通过轮询方式，检查ok文件是否就绪；

```shell
checkIsTheTargetFileIsOk (){
  
  echo "$(date +'%Y-%m-%d %H:%M:%S'): checkIsTheTargetFileIsOk begin fileName is[$1] " >> $PATH_LOG_FILE
  while true 
  do
  	
  	getSwitch
    switch=$?
    if [ $switch -eq "0" ] ; then {
      echo "$(date +'%Y-%m-%d %H:%M:%S'): switch is [$switch] , break loop" >> $PATH_LOG_FILE
      break
    }
    fi
  
    echo "$(date +'%Y-%m-%d %H:%M:%S'): check fileName is[$1] " >> $PATH_LOG_FILE
    find $PATH_DATA_DIR -name "$1" >> $PATH_LOG_FILE
    if [ 0 -lt $( find $PATH_DATA_DIR -name "$1" | wc -l ) ] ; then {
      echo "$(date +'%Y-%m-%d %H:%M:%S'): file [$1] is found , ift send success " >> $PATH_LOG_FILE
      break
    }
    fi

    if [ $(date +'%Y%m%d%H%M') -gt $SHELL_LOOP_END_TIME ] ; then {
      echo "$(date +'%Y-%m-%d %H:%M:%S'): loop out time, break loop " >> $PATH_LOG_FILE
      break
    }
    fi

    sleep 10
  done
}
```



配置系统参数，控制是否扫描就绪文件：`0 - 忽略就绪文件;1 - 等待就绪文件传输完毕 `

```shell
getSwitch (){
  switch=$(sqlplus -s "$DB_USERID" <<EOF
set head off
set linesize 20000
set echo off
set feedback off
set pagesize 0
set termout off
set trimout on
set trimspool on
select PARAMVALUE from FBAS_SYSPM where PARAMNAME='HRS_OK_FILE_CHECK_SWITCH'
/
exit
/
EOF
  )
  return $switch
}
```

这里说明下SQL-plus下面几个参数的含义

`set head off`	在每页的上面不显示列标题，而是以空白行代替;

`set linesize 20000`	设置sqlplus在一行中显示的字符总数;

`set echo off`	显示start启动脚本中的每个SQL命令，缺省为on;

`set feedback off`	当为off 时，一律不显示查询的行数;

`set pagesize 0`	设置分页页数;

`set termout off`	off禁止显示，以致从一个命令文件假脱机输出，在屏幕上看不到输出;

`set trimout on`	off时允许sqlplus显示尾部的空格

`set trimspool on`	去除重定向（spool）输出每行的拖尾空格，缺省为off

>SET TRIMOUT ON or SET TRIMSPOOL ON removes trailing blanks at the end of each displayed or spooled line.
>
>Setting these variables ON can reduce the amount of data written. However, if LINESIZE is optimal, it may be faster to set the variables OFF. The SQL*Plus output line is blank filled throughout the query processing routines so removing the spaces could take extra effort.



详细的所有参数参考下表

set  系统变量值

其中系统变量及其可选值如下:

| 系统变量        | 参数信息                         | 说明                                                         |
| --------------- | -------------------------------- | ------------------------------------------------------------ |
| arraysize       | {20(默认值)\|n}                  | 设置一批的行数，是sqlplus一次从数据库获取的行数，有效值为1至5000。大的值可提高查询和子查询的有效性，可获取许多行，但也需要更多的内存。当超过1000时，其效果不大。 |
| autocommit      | {off(默认值)\|on\|immediate}     | 控制oracle对数据库的修改的提交。设置on时，在oracle执行每个sql命令或pl/sql块后对数据库提交修改；设置置off时，则制止自动提交，需要手工地提交修改。例如用sql的commit命令，immediate功能同on。 |
| blockterminator | {.(默认值)\|c}                   | 用于结束pl/sql块。要执行块时必须发出run命令或/命令.          |
| cmdsep          | { \|c\|off(默认值)\|on}          | 用于分隔在一行中输入的多个sql/plus命令。on或off控制在一行中是否能输入多个命令。on时将自动地将命令分隔符设为分号（其中c表示所置字符）。 |
| compatibility   | {v5\|v6\|v7\|native(默认值)}     | 指定当前所连接的oracle版本。如果当前oracle的版本为5，则置compatibility为v5，为版本6时置成v6，为版本7时置成v7。如果希望由数据库决定该设置，则置成native。 |
| concat          | {.(默认值)\|c\|off\|on(默认值)}  | 设置结束一替换变量引用的字符。在中止替换变量引用字符之后可跟所有字符作为体会组成部分，否则sqlplus将解释为替换变量名的一部分。当concat开关为on时，sqlplus可重置concat的值为点（.）。 |
| copycommit      | {0(默认值)\|n}                   | 控制copy命令提交对数据库修改的批数。每次拷贝n批后将提交到目标数据库。有效值为0到5000。可用变量arraysize设置一批的大小。如果置copycommit为0，则仅在copy操作结束时执行一次提交。 |
| crt             | crt                              | 改变sqlplus runform命令使用的缺省crt文件。如果设置crt不包含什么，则crt仅包含’’’’。如果在一个form的系统调用期间要使用new.crt（缺省crt是old.crt）。可按下列形式调用form:sql>runform –c new form名或者sql>set crt newsql>runform form名第二中方法存储crt选择以致在下次运行runform命令（是在同一次sqlplus交互中）时，不需要指定。 |
| define          | {&\|c\|off\|on(默认值)}          | 设置在替换变量时所使用的字符。on或off控制sqlplus是否扫描替换变量的命令及用他们的值代替。define的on或off的设置控制scan变量的设置。 |
| echo            | {off\|on}                        | 控制start命令是否列出命令文件中的每一命令。为on时，列出命令；为off时，制止列清单。 |
| embedded        | {off(默认值)\|on}                | 控制每一报表在一页中开始的地方。为off时，迫使每一报表是在新页的顶部开始；为on时，运行一报表在一页的任何位置开始。 |
| escape          | {\(默认值)\|c\|off(默认值)\|on}  | 定义作为escape字符的字符。为off时，使escape字符不起作用；为on时，使escape字符起作用。 |
| feedback        | {6(默认值)\|n\|off\|on}          | 显示由查询返回的记录数。on和off置显示为开或关。置feedback为on时，等价于置n为1；如果置feedback为0 等价于将它置成off。 |
| flush           | {off\|on(默认值)}                | 控制输出送至用户的显示设备。为off时，运行操作系统做缓冲区输出；为on时，不允许缓冲。仅当非交互方式运行命令文件时使用off 这样可减少程序i/o总数，从而改进性能。 |
| heading         | {off\|on(默认值)}                | 控制报表中列标题的打印。为on时，在报表中打印列标题；为off时，禁止打印列标题。 |
| headsep         | {\|(默认值)\|c\|off\|on(默认值)} | 定义标题分隔字符。可在column命令中使用标题分隔符，将列标题分成多行。on和off将标题分隔置成开或关。当标题分隔为关(off)时，sqlplus打印标题分隔符像任何字符一样。 |
| linesize        | {80(默认值)\|n}                  | 设置sqlplus在一行中显示的字符总数，它还控制在ttitle和btitle中对准中心的文本和右对齐文本。可定义linesize为1至最大值，其最大值依赖于操作系统。 |
| long            | {80(默认值)\|n}                  | 为显示和拷贝long类型值的最大宽度的设置。对于oracle7，n的最大值为2g字节；对于版本6，最大值为32767。 |
| longchunksize   | {80(默认值)\|n}                  | 为sqlplus检索long类型值的增量大小。由于内存的限制，可按增量检索，该变量仅应用于oracle7。 |
| maxdata         | n                                | 设置sqlplus可处理的最大行宽字符数，其缺省值和最大值在不同操作系统中是可变的。 |
| newpage         | {1(默认值)\|n}                   | 设置每一页的头和顶部标题之间要打印的空行数。如果为0，在页之间送一换号符，并在许多终端上清屏。 |
| null            | text                             | 设置表示空值(null)的文本。如果null没有文本，则显示空格(缺省时)。使用column命令中的null子句可控制null变量对该列的设置。 |
| numformat       | 格式                             | 设置显示数值的缺省格式，该格式是数值格式。                   |
| numwidth        | {10(默认值)\|n}                  | 对显示数值设置缺省宽度。                                     |
| pagesize        | {14(默认值)\|n}                  | 设置从顶部标题至页结束之间的行数。在11英寸长的纸上打印报表，其值为54，上下各留一英寸（newpage值为6）。 |
| pause           | {off(默认值)\|on\|text}          | 在显示报表时，控制终端滚动。在每一暂停时，必须按return键。on将引起sqlplus在每一报表输出页开始时暂停。所指定的文本是每一次sqlplus暂停时显示的文本。如果要键入多个词，必须用单引号将文本括起来。 |
| recsep          | {wrapped(默认值)\|each\|off}     | recsep告诉sqlplus在哪儿做记录分隔。例如将recsep置成wrapped，在每一缠绕行之后，打印记录分行符；如果将recsep置成each，sqlplus在每一行后打印一记录分行符；如果将recsep置成off，sqlplus不打印分行符。 |
| recsepchar      | {\|c}                            | 指定显示或打印记录分行符的条件。一个记录分行符，是由recsepchar指定的字符组成的单行。空格为recsepchar的默认字符。 |
| scan            | {off\|on(默认值)}                | 控制对存在的替换变量和值的扫描。off禁止替换变量和值的处理；on则允许正常处理。 |
| serveroutput    | {off\|on}size                    | 控制在sqlplus中的存储过程是否显示输出。off时为禁止；on时则显示输出。size设置缓冲输出的字节数，缺省值为2000。n不能小于2000或大于一百万。 |
| showmode        | {off(默认值)\|on}                | 控制sqlplus在执行set命令时是否列出其新老值old或new的设置。   |
| space           | {1(默认值)\|n}                   | 设置输出列之间空格的数目，其最大值为10。                     |
| sqlcase         | {mixed(默认值)\|lower\|upper}    | 先于执行之前，将sql命令和pl/sql块的大小写进行转换。sqlplus将转换命令中的全部文本，包括带引号的直接量和标示符。sqlcase不改变sql缓冲区本身。 |
| sqlcontinue     | {>(默认值)\|文本}                | 在一附加行上继续一sqlplus命令时，sqlplus以该设置的字符序列进行提示。 |
| sqlnumber       | {off\|on(默认值)}                | 为sql命令和pl/sql块的第二行和后继行设置提示。为on时，提示行号；为off时，提示设置为sqlprompt的值。 |
| sqlperfix       | {#(默认值)\|c}                   | 设置sqlplus前缀字符。在键入一sql命令或pl/sql块时，可在单独行上键入一sqlplus命令，由sqlplus的前缀字符做前缀。sqlplus直接执行该命令，不影响sql命令或pl/sql块。前缀字符必须是非字母数字字符。 |
| sqlprompt       | {sql>(默认值)\|文本}             | 设置sqlplus的命令提示符。                                    |
| sqlterminator   | {(默认值)\|c\|off\|on(默认值)}   | 设置用于结束和执行sql命令的字符。off意味着sqlplus不识别命令终止符，用键入空行来结束sql命令；on重设置终止符为默认的分号。 |
| suffix          | {sql(默认值)\|文本}              | 设置缺省文件的后缀。sqlplus在命令中使用，来引用命令文件。suffix不控制输出(spool)文件的扩展名。 |
| tab             | {off\|on(默认值)}                | 决定sqlplus在终端输出中如何格式化空白空间。为off时，在输出中使用空格格式化空白空间；为on时，用tab字符。tab的缺省值依赖于系统，用show tab命令可查看该缺省值。 |
| termout         | {off\|on(默认值)}                | 控制由文件执行命令所产生的输出的显示。off禁止显示，以致从一个命令文件假脱机输出，在屏幕上看不到输出；on时显示输出。Termout off 不影响交互地进行命令的输出。 |
| time            | {off(默认值)\|on}                | 控制当前日期的显示。on时，在每条命令提示前显示当前时间；off时禁止时间的显示。 |
| timing          | {off(默认值)\|on}                | 控制时间统计的显示。on时，显示每一个运行的sql命令或pl/sql块的时间统计；off时，禁止每一个命令的时间统计。 |
| trimout         | {off\|on(默认值)}                | 决定sqlplus在每一显示行的末端是否允许带空格。on时将每行尾部的空格去了，特别当从慢速的通信设备存取sqlplus时可改进性能；off时允许sqlplus显示尾部的空格。trimout on不影响假脱机输出。设置tab on时，sqlplus忽略trimout on。 |
| underline       | {-(默认值)\|c\|off\|on(默认值)}  | 设置用在sqlplus报表中下划线列标题的字符。on或off将下划线置成开或关。 |
| verify          | {off\|on(默认值)}                | 控制sqlplus用值替换前、后是否列出命令的文本。on时显示文本；off时禁止列清单。 |
| wrap            | {off\|on(默认值)}                | 控制sqlplus是否截断数据项的显示。off时截断数据项；on时允许数据项缠绕到下一行。在column命令中使用wrapped和truncated子句可控制对指定列的wrap的设置。 |





*参数化*

使用 `getopt` 命令，获取对应的参数，并进行解析存储到变量中；

```shell
Usage:
 getopt <optstring> <parameters>
 getopt [options] [--] <optstring> <parameters>
 getopt [options] -o|--options <optstring> [options] [--] <parameters>

Parse command options.

Options:
 -a, --alternative             allow long options starting with single -
 -l, --longoptions <longopts>  the long options to be recognized
 -n, --name <progname>         the name under which errors are reported
 -o, --options <optstring>     the short options to be recognized
 -q, --quiet                   disable error reporting by getopt(3)
 -Q, --quiet-output            no normal output
 -s, --shell <shell>           set quoting conventions to those of <shell>
 -T, --test                    test for getopt(1) version
 -u, --unquoted                do not quote the output

 -h, --help                    display this help
 -V, --version                 display version

For more details see getopt(1).
```

`getopt` 命令可以接受一系列任意形式的命令行选项和参数，并自动将它们转换成适当的格式。

脚本中使用了三个参数

`-a`	:	允许使用 `-` 来声明一个长的选项;

`-o`	:	短的选项声明;

`-l`	:	长的选项声明;

`--`	:	举一个例子比较好理解:

```shell
#我们要创建一个名字为 "-f"的目录你会怎么办？
# mkdir -f #不成功，因为-f会被mkdir当作选项来解析，这时就可以使用
# mkdir -- -f 这样-f就不会被作为选项。
```



其实理解起来真的不太方便，在这里直接上下使用场景，其实能更好的了解这个命令：

```shell
ARGS=`getopt -a -o a:b:c:d:e:f:g:h: --long FunctionPath:,ShellPath:,DataPath:,ControlFile:,SqlLoaderPreffix:,FileNamePreffix:,FileNameSuffix:,TaskTitle: -- "$@"`

echo "$@"

eval set -- "$ARGS"
while true ; do
        case "$1" in
                -a|--FunctionPath) echo "Option FunctionPath $2";PATH_FUNCTION_DIR="$2";shift ;;
                -b|--ShellPath) echo "Option ShellPath $2";PATH_SHELL="$2";shift ;;
                -c|--DataPath) echo "Option DataPath $2";PATH_DATA_DIR="$2";shift ;;
                -d|--ControlFile) echo "Option ControlFile $2";CONTROL_FILE="$2";shift ;;
                -e|--SqlLoaderPreffix) echo "Option SqlLoaderPreffix $2";SQLLOADER_PREFFIX="$2";shift ;;
                -f|--FileNamePreffix) echo "Option FileNamePreffix $2";FILE_NAME_PREFIX="$2";shift ;;
                -g|--FileNameSuffix) echo "Option FileNameSuffix $2";FILE_NAME_SUFFIX="$2";shift ;;
                -h|--TaskTitle) echo "Option TaskTitle $2";TASK_TITLE="$2";shift ;;
                --) shift ; break ;;
        esac
shift
done
echo "Parase arguments"
```

`getopt` 我的简单使用：即以自己的意愿去声明变量，并获取使用；

看上面的例子中，`-a|--FunctionPath` 当指定了该参数时，对`PATH_FUNCTION_DIR`的变量进行了赋值操作；其他类同；



`getopt` 命令的内容非常之多，功能非常强大，有兴趣的朋友，可以去阅读下相关的介绍及博客；



源码：

[code](https://github.com/shangweikun/Ra/tree/main/shell/InteractiveFileFramework/)



鸣谢：

https://blog.csdn.net/Kingonion/article/details/19295491

https://docs.oracle.com/cd/B19306_01/server.102/b14357/ch8.htm

https://cloud.tencent.com/developer/article/1043821
