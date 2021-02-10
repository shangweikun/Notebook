# SQL



## 运维Sql



### 统计TPS

```sql
--TPS
select timeRecord, count(1)
from (select substr(BEGIN_TIME, 0, 19) as timeRecord
      from GATEWAY_LOG a
      where (BEGIN_TIME like '2021-01-14%'
          or END_TIME like '2021-01-14%'))
group by timeRecord
order by timeRecord desc;

--TPS
select avg(requestCount) as "TPS"
from (select count(1) as requestCount
      from GATEWAY_LOG a
      where (BEGIN_TIME like '2021-01-14%'
          or END_TIME like '2021-01-14%')

```



### 响应耗时

```sql
select avg(costTime) as "单日响应平均时间"
from (select to_number(regexp_substr((to_timestamp(a.END_TIME, 'YYYY-MM-DD HH24:MI:SS.FF') -
                                      to_timestamp(a.BEGIN_TIME, 'YYYY-MM-DD HH24:MI:SS.FF'))
                                         * 24 * 60 * 60 * 1000, '[^+][0-9]+')) as costTime
      from GATEWAY_LOG a
      where (BEGIN_TIME like '2021-01-14%'
          or END_TIME like '2021-01-14%')) T;
```



### 监听命令

```shell
lsnrctl start  
#会看到启动成功的界面;

lsnrctl stop  
#停止监听器命令.

lsnrctl status  
#查看监听器命令.

```

