> **使用Pg数据库，orm的方式对多字段搜索中，不同索引方式进行速度测试**

## 环境
### 软件环境：

* ubuntu 16.04
* python 3.6.4
* sqlalchemy 1.2.2

### 硬件环境：

* 普通笔记本机械硬盘
* Intel Core i7-4510U CPU @ 2.00GHz × 4
* 内存11.6 GiB

### orm model

	class Domain(Base, BaseColumns):
		__tablename__ = "domains"
		domain = Column(Text, server_default='')
		ip = Column(String(30), server_default='')
		source = Column(String(100), server_default='')
		description = Column(Text, server_default='')
		registrant = Column(String(100), server_default='')
		asn = Column(String(30), server_default='')
		reverselookup = Column(String(100), server_default='')
		updatetime = Column(TIMESTAMP, server_default=func.now())
		
使用orm 对2311条数据通过Text 类型的domain和String类型的source字段在155w条数据的pg库中依次搜索

## 搜索过程：
	
## 搜索语句

	db.session.query(Domain).filter(Domain.domain == domain, Domain.source == source).first()
	
### domain 字段和source字段都不加索引：

	domain = Column(Text, server_default='')
	source = Column(String(100), server_default='')
![](http://oumkbl9du.bkt.clouddn.com/2018-03-21-TF28u-2018-03-21_11-06-45.png)

五次测试数据分别是：18.054s、20.630s、18.724s、18.717s、17.712s
平均时间：18.7674s

###  domain 字段加索引，source字段不加索引

	domain = Column(Text, server_default='', index=True)
	source = Column(String(100), server_default='')
![](http://oumkbl9du.bkt.clouddn.com/2018-03-21-TVIRG-2018-03-21_11-19-06.png)

五次测试数据分别是：2.953s、2.940s、3.468s、3.019s、2.884s
平均时间：3.0528s

###  domain 字段不加索引，source字段加索引

	domain = Column(Text, server_default='')
	source = Column(String(100), server_default='', index=True)
数据没有截图
五次测试数据分别是：3.145s、3.361s、4.050s、3.389s、3.405s
平均时间：3.47s

### domain 字段和source字段都加索引：

	domain = Column(Text, server_default='', index=True)
	source = Column(String(100), server_default='', index=True)
![](http://oumkbl9du.bkt.clouddn.com/2018-03-21-5aT0j-2018-03-21_11-22-48.png)
五次测试数据分别是：2.527s、2.625s、2.746s、2.705s、2.615s
平均时间：2.6436s

### domain 字段和source字段添加复合索引：

	domain = Column(Text, server_default='')
	source = Column(String(100), server_default='')
    __table_args__ = (
        Index('ix_domains_domain_source', 'domain', 'source'),
    )
数据没有截图
五次测试数据分别是：2.545、2.543s、2.650s、2.683s、2.583s
平均时间：2.6008s

### domain 字段和source字段添加唯一复合索引：

	domain = Column(Text, server_default='')
	source = Column(String(100), server_default='')
    __table_args__ = (
	    UniqueConstraint('domain', 'source', name='ix_domains_domain_source'),
    )
数据没有截图
五次测试数据分别是：2.728、2.542s、2.788s、2.856s、2.666s
平均时间：2.716s