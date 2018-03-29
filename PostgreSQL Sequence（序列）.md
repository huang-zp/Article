## Sequence

    id = Column(BigInteger(), primary_key=True, nullable=False,autoincrement=True, unique=True)
	
一般ORM设计数据库model的的时候，都会加一个id字段，作为唯一标实符，**autoincrement=True** 说明这个字段是自动增长的，当使用migrate的时候，会创建一个名为**"表名_seq_id"**的序列

例如：
![](http://oumkbl9du.bkt.clouddn.com/2018-03-23-UpWMi-2018-03-23_17-51-15.png)

也可以手动创建Sequence

	CREATE SEQUENCE test_id_seq;

	SELECT * FROM test_id_seq;

![](http://oumkbl9du.bkt.clouddn.com/2018-03-23-mdsjf-2018-03-23_17-56-15.png)

last_value ：当前自增到的值
start_value ： 从什么值开始自增
increment_by ： 自增数
max_value：最大值
min_value：最小值
is_cycled：到最大值后是否从新循环

## 相关函数

**nextval(‘sequence_name’)**: 将当前值设置成递增后的值

上面建立test_id_seq中序列的初始值是1，递增值是1， 执行一次nextval后应该会是2

![](http://oumkbl9du.bkt.clouddn.com/2018-03-29-tSNK2-2018-03-29_15-24-55.png)

**currval(‘sequence_name’)**: 返回当前值

![](http://oumkbl9du.bkt.clouddn.com/2018-03-29-vjgat-2018-03-29_15-29-21.png)

**setval(‘sequence_name’, n, b=true):** 设置当前值；

b参数的话默认是true。这个参数主要是控制设置好值后，下一次执行nextval函数是返回**设置的值**还是返回**设置的值加上递增的值**
是ture的时候返回**设置的值加上递增的值**
是fasle的时候返回**设置的值**

三句命令同时执行，b参数的作用显而易见
![](http://oumkbl9du.bkt.clouddn.com/2018-03-29-mZOcN-2018-03-29_15-41-08.png)

![](http://oumkbl9du.bkt.clouddn.com/2018-03-29-ONtW0-2018-03-29_15-42-49.png)

## 问题阐述

我需要同步一个表到一个新的表，而且id也要同步过来。
在设计新表的时候id字段autoincrement=True，所以为其建了一个索引
因为我同步的时候把旧表的id拿过来直接赋值给新表，所以新表一直没有运行nextval，索引的值还是1
当我同步完成后，在向新表中新写数据的时候这是id就要依靠自动生成，但是生成值是1，因为第一次运行。因为之前同步的数据有id为1的数据，况且id为主键。

so:

	SELECT setval('test_id_seq', (SELECT max(id) FROM test));

