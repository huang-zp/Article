>**之前自己写文章是在博客平台，也用过Hexo，但是总有一些不适的感觉，所以打算为自己写一个博客网站，不用再受其他牵制，于是MagicPress应运而生。后来平常学习中有一些小知识点总是记了好几次还是会忘，所以萌生了开发一个在线测试练习的平台，因为打算用子域名做为网址，发现Flask实现好简单**


## 关于蓝图

蓝图使用起来就像应用当中的子应用一样，可以有自己的模板，静态目录，有自己的视图函数和URL规则，蓝图之间互相不影响。但是它们又属于应用中，可以共享应用的配置。一个 Blueprint 对象与 Flask 应用对象的工作方式很像，但它确实不是一个应用，而是一个描述如何构建或扩展应用的组件，对于大型应用来说，我们可以通过添加蓝图来扩展应用功能，而不至于影响原来的程序。
想要了解更多的Flask蓝图，请自行谷歌学习。

## 蓝图绑定URL前缀

### 静态URL前缀

例如：
app.register_blueprint(test, url_prefix='/test')
url_prefix参数指定了这个test蓝图的URL前缀

### 动态URL前缀
直接看Demo吧

	# coding:utf-8
	from flask import Flask, render_template_string, Blueprint, g

	app = Flask(__name__)

	test = Blueprint('test', __name__)


	@test.url_value_preprocessor
	def get_name(endpoint, values):
		name = values.pop('name')
		g.name = 'the' + name


	@test.route('/')
	def index():
		return render_template_string('{{ g.name }}')

	app.register_blueprint(test, url_prefix='/<name>')
	if __name__ == '__main__':
		app.run()

**url_value_preprocessor:**
通过这个装饰器将蓝图URL前缀中传来的值传递到test蓝图中，通过g来储存信息、

效果：
![](http://oumkbl9du.bkt.clouddn.com/2017-09-21-j8MSr-blueprint1.png)
![](http://oumkbl9du.bkt.clouddn.com/2017-09-21-G7ntC-blueprint2.png)


## 蓝图绑定子域名

### 静态子域名

	app.config['SERVER_NAME'] = 'huangzp.com'
	app.register_blueprint(test, subdomain='test')
subdomain参数指定了这个test蓝图的URL前缀，
例如现在这个网站，代码中我这样写
![](http://oumkbl9du.bkt.clouddn.com/2017-09-21-WGcnM-blueprint3.png)
前提是我要先对blog.huangzp.com和point.huangzp.com进行DNS解析

### 动态子域名

在静态子域名的配置前提下，实现思路和动态后缀一样
获取值同样使用url_value_preprocessor装饰器
注册蓝图时使用subdomain参数替代url_prefix
配置变量SERVER_NAME

