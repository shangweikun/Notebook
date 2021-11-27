Linux find 命令用来在指定目录下查找文件。任何位于参数之前的字符串都将被视为欲查找的目录名。如果使用该命令时，不设置任何参数，则 find 命令将在当前目录下查找子目录与文件。并且将查找到的子目录和文件全部进行显示。



### 语法

```shel
find   path   -option   [   -print ]   [ -exec   -ok   command ]   {} \;
```



**参数说明** :

find 根据下列规则判断 path 和 expression，在命令列上第一个 - ( ) , ! 之前的部份为 path，之后的是 expression。如果 path 是空字串则使用目前路径，如果 expression 是空字串则使用 -print 为预设 expression。

expression 中可使用的选项有二三十个之多，在此只介绍最常用的部份。

```
-mount, -xdev : 只检查和指定目录在同一个文件系统下的文件，避免列出其它文件系统中的文件
-amin n : 在过去 n 分钟内被读取过

-anewer file : 比文件 file 更晚被读取过的文件

-atime n : 在过去n天内被读取过的文件

-cmin n : 在过去 n 分钟内被修改过

-cnewer file :比文件 file 更新的文件

-ctime n : 在过去n天内被修改过的文件

-empty : 空的文件-gid n or -group name : gid 是 n 或是 group 名称是 name

-ipath p, -path p : 路径名称符合 p 的文件，ipath 会忽略大小写

-name name, -iname name : 文件名称符合 name 的文件。iname 会忽略大小写

-size n : 文件大小 是 n 单位，b 代表 512 位元组的区块，c 表示字元数，k 表示 kilo bytes，w 是二个位元组。

-type c : 文件类型是 c 的文件。

* d: 目录

* c: 字型装置文件

* b: 区块装置文件

* p: 具名贮列

* f: 一般文件

* l: 符号连结

* s: socket

-pid n : process id 是 n 的文件
```





-----

你可以使用 ( ) 将运算式分隔，并使用下列运算。

````
exp1 -and exp2

! expr

-not expr

exp1 -or exp2

exp1, exp2
````





------------

20211118更新



### 查找时间

find 命令有几个用于根据您系统的时间戳搜索文件的选项。这些时间戳包括

````
• mtime — 文件内容上次修改时间

• atime — 文件被读取或访问的时间

• ctime — 文件状态变化时间
````



mtime 和 atime 的含义都是很容易理解的，而 ctime 则需要更多的解释。由于 inode 维护着每个文件上的元数据，因此，如果与文件有关的元数据发生变化，则 inode 数据也将变化。这可能是由一系列操作引起的，包括创建到文件的符号链接、更改文件权限或移动了文件等。由于在这些情况下，文件内容不会被读取或修改，因此 mtime 和 atime 不会改变，但 ctime 将发生变化。

这些时间选项都需要与一个值 *n* 结合使用，指定为 *-n、n* 或 *+n*。

```
• -n 返回项小于 n

• +n 返回项大于 n

• n 返回项正好与 n 相等
```



默认情况下，-mtime、-atime 和 -ctime 指的是最近 24 小时。但是，如果它们前面加上了开始时间选项，则 24 小时的周期将从当日的开始时间算起。您还可以使用 mmin、amin 和 cmin 查找在不到 1 小时的时间内变化了的时间戳。

应该注意的是，使用 find 命令查找文件本身将更改该文件的访问时间作为其元数据的一部分。

您还可以使用 -newer、-anewer 和 –cnewer 选项查找已修改或访问过的文件与特定的文件比较。这类似于 -mtime、-atime 和 –ctime。

````
应该注意的是，使用 find 命令查找文件本身将更改该文件的访问时间作为其元数据的一部分。

您还可以使用 -newer、-anewer 和 –cnewer 选项查找已修改或访问过的文件与特定的文件比较。这类似于 -mtime、-atime 和 –ctime。

• -newer 指内容最近被修改的文件

• -anewer 指最近被读取过的文件

• -cnewer 指状态最近发生变化的文件
````



一篇非常赞的blog

https://www.oracle.com/cn/technical-resources/articles/linux-calish-find.html