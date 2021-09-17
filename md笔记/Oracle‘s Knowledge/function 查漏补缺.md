# DENSE_RANK

> `DENSE_RANK` computes the rank of a row in an ordered group of rows and returns the rank as a `NUMBER`. The ranks are consecutive integers beginning with 1. The largest rank value is the number of unique values returned by the query. Rank values are not skipped in the event of ties. Rows with equal values for the ranking criteria receive the same rank. This function is useful for top-N and bottom-N reporting.



Dense_rank函数：用于排序操作，重复排序不进行累计，用于判断top-N的排序

注意⚠️：dense_rank和rank的区别，参考下面stackoverflow的说明

https://stackoverflow.com/questions/11183572/whats-the-difference-between-rank-and-dense-rank-functions-in-oracle/11183610



https://docs.oracle.com/cd/B19306_01/server.102/b14200/functions043.htm