# 索引失效 UNSABLE状态原因

①　对分区表的某个含有数据的分区执行了TRUNCATE、DROP操作可以导致该分区表的全局索引失效，而分区索引依然有效，如果操作的分区没有数据，那么不会影响索引的状态。需要注意的是，对分区表的ADD操作对分区索引和全局索引没有影响。

②　执行EXCHANGE操作后，全局索引和分区索引都无条件地会被置为UNUSABLE（无论分区是否含有数据）。但是，若包含INCLUDING INDEXES子句（缺省情况下为EXCLUDING INDEXES），则全局索引会失效，而分区索引依然有效。

③　如果执行SPLIT的目标分区含有数据，那么在执行SPLIT操作后，全局索引和分区索引都会被被置为UNUSABLE。如果执行SPLIT的目标分区没有数据，那么不会影响索引的状态。

④　对分区表执行MOVE操作后，全局索引和分区索引都会被置于无效状态。

⑤　手动置其无效：ALTER INDEX IND_OBJECT_ID UNUSABLE;。

对于分区表而言，除了ADD操作之外，TRUNCATE、DROP、EXCHANGE和SPLIT操作均会导致全局索引失效，但是可以加上UPDATE GLOBAL INDEXES子句让全局索引不失效。重建分区索引的命令为：ALTER INDEX IDX_RANG_LHR REBUILD PARTITION P1;。



鸣谢：

[1]: https://www.modb.pro/db/14578	"什么是不可用索引（Unusable Indexes），哪些操作会导致索引变为不可用即失效状态？"
[2]: https://www.orafaq.com/wiki/Unusable_indexes	"Unusable indexes"
[3]: http://www.dba-oracle.com/t_indexes_invalid_unusable.htm	"Oracle invalid or unusable indexes"
[4]: https://asktom.oracle.com/pls/apex/f?p=100:11:0::::p11_question_id:1859798300346695894	"Ask Tom"

