# Linux工具

Linux性能监控工具主要包含以下4中：top命令，**vmstat命令**，iostat命令，pidstat工具。

故障查询：

**1.查询整机** 首先通过top命令查看整体情况，比较重要的指标有Load AVg，CPU usage，每个进程的CPU和MEM。

**2.CPU** 通过vmstat工具查询CPU的情况，vmstat包含两个参数，第一个参数是采样隔间时间，但是是秒，第二个参数是采样次数。

**3.内存**

```shell
free
free -g
free -m
```

一般应用程序可用内存/系统物理内存<20%代表内存不足，需要增加内存

**4.硬盘**

```shell
df -h
```

**5.磁盘IO**

```shell
iostat -xdk 2 3
```

**6.网络IO**

```shell
ifstat l
```