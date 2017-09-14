>**Celery - 分布式任务队列。最近在MagicPress中集成了Celery ,并使用supervisor对celery进行管理（守护进程），celery入门很简单，通过一两个demo一般一个从未接触过celery的人应该都会对其有一个大致的认识与熟悉相应的应用场景。记录一下Flask工厂模式与celery的集成，难点：Flask应用上下文环境

##### 用MagicPress这个项目举例：

## 目录结构：
![](http://oumkbl9du.bkt.clouddn.com/2017-08-31-x9Nv6-mulu.png)

简要说明：
	logs:日志
	MagicPress:程序主目录
	migrations:数据库迁移
	config.py：配置文件
	manage.py:启动文件
	MagicPress.\_\_init__.py: app创建文件
	MagicPress.extensions.py: 扩展库实例

## \_\_init__.py 文件Celery相关代码

	...
	app = Flask(__name__)
	app.config.from_object(Config)
	celery = make_celery(app)
	from MagicPress.utils.tasks import send_async_email
	...

还有一个create_app的函数，很多人把app = Flask(\__name_\__)这代码放在这个函数中, 我本来也是，但是为了能集成celery我把这句代码拿了出来放在函数外面， 如果有更好的方法，希望能留言告诉我。

### 解释：

make_celery：一个函数，功能：任务执行封装到程序上下文
该函数创建一个新的Celery对象，按应用程序的配置配置celery的broker，从Flask配置中更新Celery配置的其余部分，然后创建一个任务的子类，将任务执行封装到应用程序上下文中。

**make_celery代码：**

	from celery import Celery

	def make_celery(app):
		celery = Celery(app.import_name, broker=app.config['CELERY_BROKER_URL'])
		celery.conf.update(app.config)
		TaskBase = celery.Task
		class ContextTask(TaskBase):
			abstract = True
			def __call__(self, *args, **kwargs):
				with app.app_context():
					return TaskBase.__call__(self, *args, **kwargs)
		celery.Task = ContextTask
		return celery
		
		
**导入是为了在celery中注册任务
tasks文件代码: **

	from .. import celery, mail
	from flask_mail import Message
	from flask import current_app


	@celery.task(name='send_async_email')
	def send_async_email(message_details):
		"""Background task to send an email with Flask-Mail."""
		try:
			msg = Message(message_details['subject'],
						  message_details['recipients'])
			msg.body = message_details['body']
			mail.send(msg)
		except Exception as e:
			current_app.logger.info(e)

## 运行
在\_\_init__.py同一目录下输入命令：
celery worker -A MagicPress.celery --loglevel=info

当然还有很多Flask工厂模式与Celery集成的方法， 如果这个方法不适合你的代码结构，可以去github看一些优秀的Flask与Celery集成的代码。