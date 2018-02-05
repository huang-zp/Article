> **从去年七月学习Flask开始到现在，也算是对Python web有了一定的了解，前前后后使用Flask做了十几个网站，期间对Django一直是没事学一点那种态度，也没有使用Django完整的做过一个健壮的系统，总觉得Flask灵活，自己不需要Django，时间越长越感觉Django其实也挺重要，不需要像Flask一样去选择集成什么插件，选择怎样做结构。Django也有自己的特色，所以近期准备完整的把Django系统的学习一下。在此记录笔记。**

## Django setting设置

使用Pycharm新建Django项目后会自动生成Django基本模板，需要修改的如下：

**INSTALLED_APPS** ： 增加app名称
**TEMPLATES中的DIRS** : 

	'DIRS': [os.path.join(BASE_DIR, 'templates')]
**数据库**
默认数据库是sqlite，这是修改为mysql的例子：

	DATABASES = {
		'default': {
			'ENGINE': 'django.db.backends.mysql',
			'NAME': 'mbload',
			'USER': 'root',
			'PASSWORD': '123456',
			'HOST': '127.0.0.1'
		}
	}


**增加属性**

	STATICFILES_DIRS = [
		os.path.join(BASE_DIR, 'static')
	]
	
## Django urls设置

Django的url采用正则匹配的方法，这点和Flask是有点不同的

	urlpatterns = [
		url(r'^admin/', admin.site.urls),
		url(r'^index/$', index),
	]
	
此处容易犯的一个错误是/$的问题，假如index后没有加/$，此时假如还有另外一个url**"indexsecond"**，就像这样：

	urlpatterns = [
		url(r'^admin/', admin.site.urls),
		url(r'^index', index),
		url(r'^indexsecond', indexsecond),
	]
	
在浏览器中个输入/indexsecond，则会访问/index那个页面，因为/index提前匹配成功了输入的url

**增加name参数**

url(r'^index/$', index, name='my_index'),

在html中可以这样写自动生成url，类似于flask中的url_for

	{% url "my_index" %}
	
## 简单的model设计

Django中已经内置了orm，不需要flask一样自己去集成。

简单的设计参考代码：

	class User(models.Model):
		name = models.CharField(max_length=20, verbose_name=u'姓名', default='',  null=True, blank=True)
		email = models.EmailField(verbose_name=u'邮箱')
		address = models.CharField(max_length=100, verbose_name=u'地址')


		class meta:
			verbose_name = u'用户信息'
			verbose_name_plural = verbose_name
			db_table = 'user_message'
			
			
## 简单的views设计
和Flask不同，Django的request需要作为参数传进函数，没有进行装饰器修饰

简单的views代码：

	def index(request):
		return render(request, 'huangzp_index.html')