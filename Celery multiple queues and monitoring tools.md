>** Celery 多队列总结以及实时监控工具Flower的使用**

因为一直对Celery队列，work，多服务器间跑Celery，以及监控之类的总是有些理不清。于是写了小demo去理一下

# Celery multiple queues

## Demo解释
**Demo结构：**

> flower\_demo
├── config.py
├── \_\_init__.py
├── task1.py
└── task2.py

> 0 directories, 4 files

**config.py**

	
	include=['flower_demo.task1', 'flower_demo.task2']

	task_routes = {
			'flower_demo.task1.add1': {'queue': 'add1task'},
			'flower_demo.task2.add2': {'queue': 'add2task'},
	}

**\_\_init__.py**

	from celery import Celery

	app = Celery('demo',
				 broker='redis://localhost',
				 backend='redis://localhost')

	app.config_from_object('flower_demo.config')

**task1.py**

	from flower_demo import app

	@app.task
	def add1(x, y):
		return x+y

	@app.task
	def minus1(x, y):
		return x - y

**task2.py**

	from flower_demo import app

	@app.task
	def add2(x, y):
		return x + y

	@app.task
	def minus2(x, y):
		return x - y

celery_app中task_routes用来指定task在哪个队列中执行，如果没有指定的task则都在default队列执行
**所以：**
task1.add1在add1task1队列执行
task2.add2在add2task2队列执行
task1.minus1，task2.minus2在default队列执行

## 启动work

![](http://oumkbl9du.bkt.clouddn.com/2018-03-30-y0wkX-2018-03-30_10-21-14.png)
开三个窗口，分别运行：
celery -A flower_demo worker --loglevel=info -Q add1task -n task1
celery -A flower_demo worker --loglevel=info -Q add2task -n task2
celery -A flower_demo worker --loglevel=info

三个命令启动了三个work。
-n参数是为当前work命名，如果没有-n，默认为当前用户名
-Q参数是为当前work指定队列。如果没有-Q，默认为’default队列‘

**执行task:**
![](http://oumkbl9du.bkt.clouddn.com/2018-03-30-3JQqC-2018-03-30_10-33-13.png)

task运行情况正如celery_app.py文件中指定的情况一样

### 指令task运行队列
**上面指定task队列是在celery实例的配置文件中写的。还有另一个方法，在task参数中写定指定队列**

**task1.py**

	from flower_demo import app

	@app.task
	def add1(x, y):
		return x+y

	@app.task(queue='minus1task')
	def minus1(x, y):
		return x - y

**task2.py**

	from flower_demo import app


	@app.task
	def add2(x, y):
		return x + y


	@app.task(queue='minus2task')
	def minus2(x, y):
		return x - y

启动work指定队列为minus1task，minus2task
![](http://oumkbl9du.bkt.clouddn.com/2018-03-30-5tOvM-2018-03-30_10-54-38.png)
启动task
![](http://oumkbl9du.bkt.clouddn.com/2018-03-30-Qmwzk-2018-03-30_10-56-25.png)

**队列更高级用法**
config.py

	from kombu import Exchange, Queue

	include=['flower_demo.task1', 'flower_demo.task2']

	task_routes = {
			'flower_demo.task1.add1': {'queue': 'add1task'},
			'flower_demo.task2.add2': {'queue': 'add2task'},
	},

	task_queues = (
			Queue('default', Exchange('default'), routing_key='default'),
			Queue('add1task', Exchange('add1task'), routing_key='add1task'),
			Queue('add2task', Exchange('add2task'), routing_key='add2task'),
		)

这种只要启动一个work就可以，不用启动一个work指定一个队列
看**[queues]**
![](http://oumkbl9du.bkt.clouddn.com/2018-03-30-0hKkz-2018-03-30_11-05-38.png)

task_queues这种声明队列主要作用在Exchange, 和 routing_key。主要用在多服务器间复杂队列。不太了解，pass。

# Monitoring tools
Flower：一个对 Celery 集群进行实时监控并提供 web 管理界面

**（猜测）**
flower监控主要依据消息队列，也就是RabbitMQ或者Redis。
flower看在这个队列中有多少work在运行
所以flwoer启动可用根据celery实例启动，会判断出来当前实例配置中的队列信息，然后在依据队列信息去查队列中的work，然后监控
![](http://oumkbl9du.bkt.clouddn.com/2018-03-30-Og9z9-2018-03-30_11-12-36.png)
也可用直接指定队列启动:
![](http://oumkbl9du.bkt.clouddn.com/2018-03-30-aD87h-2018-03-30_11-13-27.png)

So：
如果项目是需要多台服务器，celery需要在多台服务器中运行，只要指定消息队列是同一个服务器上的队列。flower可以一起监控。


# 关于阻塞：
一般启动一个work会默认是四个进程，也就是说同时可以执行四个任务，如果还有第五个任务，就要等四个进程中有空闲进程。
![](http://oumkbl9du.bkt.clouddn.com/2018-03-30-1H6RX-2018-03-30_11-27-05.png)
这个进程数量可以在启动work的时候加-c参数指定

	celery -A flower_demo worker --loglevel=info -c 8
![](http://oumkbl9du.bkt.clouddn.com/2018-03-30-lAat3-2018-03-30_11-28-01.png)

所以不同的类型任务最好多建一个队列。例如计算任务和发邮件任务。有可能计算任务时间太长会造成邮件任务在等。所以建一个发邮件的队列和一个计算用的队列