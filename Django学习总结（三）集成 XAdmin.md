> **Django本身自带后台admin，但是xadmin是基于admin更高一层的开发，同时体验更棒，感觉非常贴近民心。。。可能Flask的后台设计略有些繁琐（但是更自由）而至于我对体验xadmin更迫切...。主要写替换admin,xadmin的注册，xadmin的后台样式修改**

## xadmin替换admin

1. 安装pip install xadmin
2. 在setting文件INSTALLED_APPS列表中加入xadmin， crispy_forms
3. 在urls文件中


	import xadmin
	urlpatterns = [
		url(r'^xadmin/', xadmin.site.urls),
		]

4. Task 中运行makemigrations、migrate

## xadmin注册model

首先在add下新建adminx.py文件。

	class ArticleAdmin(object):
		list_display = ['author', 'title', 'add_time']
		search_fields = ['lesson', 'name']
		list_filter = ['author__name', 'name', 'add_time']
		
		xadmin.site.register(Article,ArticleAdmin)

这里和flask-admin相像，注意list_filter中'author_name'，代表author model中name字段


## xadmin 增加主题功能

在user app下adminx.py文件中

	from xadmin import views

	class BaseSetting(object):
		enable_themes = True
		use_bootswatch = True
		
	xadmin.site.register(views.BaseAdminView, BaseSetting)
	
## 修改xadmin后台标题，脚注，左边导航栏收起每个app的model

在user app下adminx.py文件中

	from xadmin import views
	class GlobalSetting(object):
		site_title = "后台管理系统"
		site_footer = "HUANGZP"
		menu_style = "accordion"
		
	xadmin.site.register(views.CommAdminView,GlobalSetting)
