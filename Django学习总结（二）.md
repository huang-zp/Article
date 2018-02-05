> **覆盖Django默认用户表，数据库设计一些知识点记录，使用分层设计解决数据库设计循环引用问题，增加Django导入搜索文件夹**

## 覆盖Django默认用户表

Django使用migrate之后会生产一些默认的表，其中就有user表，但有时满足不了自己本身的需求，所以需要覆盖掉默认用户表，继承默认用户表然后根据需求增加所需的字段，代码示例如下：

	from django.contrib.auth.models import AbstractUser
	class UserProfile(AbstractUser):
    nick_name = models.CharField(max_length=50, verbose_name=u'昵称', default='')
    birday = models.DateField(verbose_name=u'生日',null=True, blank=True)
    gender = models.CharField(max_length=6, choices=(("male", u"男"), ("female", u"女")), default='')
    address = models.CharField(max_length=50, default='')
    mobile = models.CharField(max_length=11, null=True, blank=True)
    image = models.ImageField(upload_to="image/%Y/%m", default=u'image/default.png', max_length=100)

    class Meta:
        verbose_name = "用户信息"
        verbose_name_plural = verbose_name

    def __unicode__(self):
        return self.username
		

然后再Django setting文件中加入：

	AUTH_USER_MODEL = "users.UserProfile"

users是用户app


## 数据库设计一些知识点

* **外键的设计：**

比如作者 文章之间一对多则，文章model设计中作者字段：

	user = models.ForeignKey(User, verbose_name=u'作者')
	
* **图片字段的设计：**

	
	image = models.ImageField(upload_to="image/%Y/%m", verbose_name=u'封面照片')
	
* **文件字段的设计：**


	download = models.FileField(upload_to="File/resource/%Y/%m", verbose_name=u'资源文件', max_length=100)

* **时间字段的设计：**


	from datetime import datetime
	add_time = models.DateTimeField(default=datetime.now, verbose_name=u'添加时间')

datetime.now不要加（）,否则就是此model创建的时间，不是实例的时间了。

* **选择字段的设计：**


	gender = models.CharField(max_length=6, choices=(("male", u"男"), ("female", u"女")), default='')

## 循环引用问题

有时间数据库设计的时候，例如作者和文章设计的时候，作者可能在user app中，文章可能在article app中，如果在user app中设计用户收藏的model，则需要在article app中导入article类，在article app中设计文章评论则需要导入user app中user类，当然也有其他model设计不像上面说的那样，可能其他的设计也不会导致循环引用问题，我在刚开始使用Flask开发网站的时候就是在同一层次下设计，遇见循环引用问题想办法写入同一个app，之前自己开发的小网站这样也不会有什么问题，开始实习后在公司看已经成型的项目（比自己之前写的要重很多）学到了很多，也越发感觉数据库设计的重要性。  所以我感觉分层去解决循环引用很实用，且结构分明

在user app和article app之外再增加一个用户操作app，像用户收藏，文章评论这样有关于用户操作的model设计全部在这里进行，层次上高于user和article

	from users.models import UserProfile
	from articles.models import Article
	class ArticleComments(models.Model):
		user = models.ForeignKey(UserProfile, verbose_name=u"用户")
		course = models.ForeignKey(Article, verbose_name=u"文章")
		comments = models.CharField(max_length=200, verbose_name=u"评论")
		add_time = models.DateTimeField(default=datetime.now, verbose_name=u'评论时间')

		class Meta:
			verbose_name = u"文章评论"
			verbose_name_plural = verbose_name
			

## 增加Django导入搜索文件夹

有时候会因为各种原因在代码根目录建文件夹存放不同的代码，总之会遇到的，这就会引起导入搜索问题

在setting文件中加入：

	sys.path.insert(0, os.path.join(BASE_DIR, '新建文件夹名字'))

