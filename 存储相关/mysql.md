#### **索引的本质**

MySQL官方对索引的定义为：索引（Index）是帮助MySQL高效获取数据的数据结构。提取句子主干，就可以得到索引的本质：索引是数据结构。

mylSAM 使用的是非聚集索引(所以只存引用地址)

InnoDB 使用聚集索引(叶子节点存索引加数据)

聚集索引这种实现方式使得按主键的搜索十分高效，但是辅助索引搜索需要检索两遍索引：首先检索辅助索引获得主键，然后用主键到主索引中检索获得记录。尽量使用单调的主键（自增），这样不会使得b+tree 频繁分裂调整。

####  **MVCC**

trx_id:事务ID

roll_pointer:回滚指针指向之前数据位置(方便回滚)

update底层流程:

添加一条新的数据，并将当前列的 roll_pointer 指向 之前旧的数据。

select底层流程:

每个sesion执行第一个select时候会生成一个一致性视图(read-view),它是由执行查询时所有未提交事务id数组(数组里面最小id为min_id) 和 已经创建的最大事务id{max_id} 组成，查询的时候结果需要根据read-view 做对比从而获得快照结果。

![img](../assets/mysql/clipboard.png)

版本链对比规则:

1.如果落在绿色部分(trx_id < min_id)，表示版本是已提交的事务生成的，这个数据是可见的，

2.如果落在红色部分(trx_id > max_id) 表示这个版本是由将来启动的事务生成的，是肯定不可见的。

3.如果落在黄色部分(min_id<=trx_id<=max_id),包括两种情况

   a.若row的trx_id在数组中,表示这个版本是由i还没提交的事务生成的，不可见，对当前自己的事务是可见的

   b.若row的trx_id不在数组中,表示这个版本是由已经提交的事务组成，可见

   

#### **MyISAM和InnoDB**

MyISAM 适合于一些需要大量查询的应用，但其对于有大量写操作并不是很好。甚至你只是需要update一个字段，整个表都会被锁起来，而别的进程，就算是读进程都无法操作直到读操作完成。另外，MyISAM 对于 SELECT COUNT(*) 这类的计算是超快无比的。

InnoDB 的趋势会是一个非常复杂的存储引擎，对于一些小的应用，它会比 MyISAM 还慢。他是它支持“行锁” ，于是在写操作比较多的时候，会更优秀。并且，他还支持更多的高级应用，比如：事务。

#### **MYSQL分层**

1.连接层(提供与客户端的连接服务)

2.服务层(1.提供各种用户使用的接口(CURD...)2.提供SQL优化器)

3.引擎层(提供各种存储数据的方式(MyISAM和InnoDB))

4.存储层(存储数据)

sql:

编写过程: select .. from .. join .. on .. where ..group by .. having .. order by .. limit ..

解析过程: from .. join .. where .. group by .. having .. select .. order by .. limit ..

id				编号 （id值相同，从上往下执行，小表驱动大表，笛卡尔积，数据小的表先查｜id值不同，id越大越先查，主要是自查询，从内往外执行）

select_type		索引类型（PRIMARY:主查询、SUBQUERY:子查询、SIMPLE:简单查询 、DERIVED:衍生查询(用到了临时表)）

table			当前操作表

partitions		

type			 类型（system>const(结果只能有一个,主键索引和唯一索引)>eq_ref(一对一的情况下)>ref(根据索引查询，匹配多个数据)>range(范围查询)>index(查询全部索引数据)>all(查询全部page数据)）

possible_keys	 用到索引

key				实际使用索引

key_len			实际使用索引长度

ref				表之间引用

rows 			预估扫描行数

filtered			

Extra			额外信息