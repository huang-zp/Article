## 初始操作：

**安装客户端**
	
	sudo apt-get install postgresql-client

**安装服务器**

	sudo apt-get install postgresql

安装完成后，PostgreSQL服务器会自动在本机的5432端口开启。

切换到postgres用户

	sudo su - postgres

进入控制台：

	psql

为postgres用户设置密码：
	
	\password postgres

显示所有数据库

	\l

或者

	SELECT * FROM pg_database;

## 创建数据库
**方法1：**

进入控制台：

	sudo su - postgres psql

进入控制台后：

	CREATE DATABASE huangzp_first

**方法2：**

	sudo -u postgres createdb huangzp_second
	
![](http://oumkbl9du.bkt.clouddn.com/2018-02-05-8ilrm-2018-02-05_16-34-49.png)

## 删除数据库
也是两个方法，CREATE改DROP

**方法1**

	sudo su - postgres psql
控制台下：

	DROP DATABASE huangzp_first
	
**方法2**
	
	sudo -u postgres dropdb huangzp_second

![](http://oumkbl9du.bkt.clouddn.com/2018-02-05-nVlqb-2018-02-05_16-37-03.png)

## 数据备份与恢复
### 备份数据库:

	sudo -u postgres pg_dump -U 用户名 -d 数据库名 -h 服务器IP > 备份sql名
	
### 恢复数据库:

	sudo -u postgres psql 将被恢复的数据库名 < 已备份的sql名
	
	
## 一些postgres数据库命令：

	\h：查看SQL命令的解释，比如\h select。
	\?：查看psql命令列表。
	\l：列出所有数据库。
	\c [database_name]：连接其他数据库。
	\d：列出当前数据库的所有表格。
	\d [table_name]：列出某一张表格的结构。
	\du：列出所有用户。
	\e：打开文本编辑器。
	\conninfo：列出当前数据库和连接的信息。
	
## 参考文档：

> http://blog.csdn.net/chuan_day/article/details/41647651
http://www.ruanyifeng.com/blog/2013/12/getting_started_with_postgresql
http://blog.csdn.net/dyx1024/article/details/6596036