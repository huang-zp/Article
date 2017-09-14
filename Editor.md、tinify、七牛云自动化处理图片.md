>**现在写文章都比较倾向于使用图床方便很多。传统做法是将图片上传到图床，复制外链，在文章中加入图片外链。但是做为一名有理想有抱负的程序员，忍受不住这么繁琐的流程，哈哈，那就来写个自动化的吧

## 环境:
- Python (2.7.13)
- Flask (0.12.2)
- qiniu (7.1.5)
- tinify (1.5.1)



## Editor.md
开源在线MakeDown编辑器，长这样：![](http://oumkbl9du.bkt.clouddn.com/2017-08-31-0aZCh-editor.png)
自带图片上传功能，开发者所要做的就是在editor.md配置中开启图片上传功能，然后写一个处理图片的函数，并返回一个图片的链接。

### 代码实现：

    def editor_pic(self):
        image_file = request.files['editormd-image-file']
        if image_file and allowed_photo(image_file.filename):
            try:
                filename = secure_filename(image_file.filename)
                filename = str(date.today()) + '-' + random_str() + '-' + filename
                file_path = os.path.join(bpdir, 'static/editor.md/photoupdate/', filename)
                qiniu_path = os.path.join(bpdir, 'static/blog/qiniu_pic/', filename)
                image_file.save(file_path)
                ting_pic(file_path, qiniu_path)
                qiniu_link = get_link(qiniu_path, filename)
                data = {
                    'success': 1,
                    'message': 'image of editor.md',
                    'url': qiniu_link
                }
                return json.dumps(data)
            except Exception as e:
                current_app.logger.error(e)
        else:
            return u"没有获得图片或图片类型不支持"

总之就是写一个接受图片的函数，然后保存这个图片，将图片的信息保存到json格式，返回过去就好了，其余代码就是和七牛、tinify有关了。

## tinify图片优化
图片的大小影响着页面的加载速度，以及七牛的存储空间，所以如果有个软件可以在保持图片显示效果几乎不变的情况大幅度压缩图片，何不尝试一下
TingPNG是一个网站，每月可以免费压缩500张图片，够使用的了，压缩能力超级棒，可以去体验一下。
使用这个网站提供的API，就可以方便的在代码中实现自动上传压缩下载
看官方网站怎么说的![](http://oumkbl9du.bkt.clouddn.com/2017-08-31-jQyp7-tingpng1.png)
压缩率达到了百分之七十..


写一个函数，功能是将图片上传到TingPNG,然后等压缩完将压缩后的图片保存下来
### 代码实现

	# -*- coding: utf-8 -*-
	import tinify
	from flask import current_app


	def ting_pic(filename, savename):
		try:
			tinify.key = "******"
			tinify.validate()
		except tinify.Error, e:
			# Validation of API key failed.
			current_app.logger.error(tinify.Error)
			current_app.logger.error(e)
		try:
			source = tinify.from_file(filename)
		except Exception as e:
			source.to_file(filename)
			current_app.logger.error(e)
		source.to_file(savename)

key去注册个账号就会有的...

## 七牛图床
七牛提供有各种代码的SDK，big python自然也是有

写一个函数，功能是将图片上传到七牛云图床，并返回此图片外链
### 代码实现：

	# -*- coding: utf-8 -*-

	from qiniu import Auth, put_file, etag
	from flask import current_app
	# 需要填写你的 Access Key 和 Secret Key
	access_key = '******'
	secret_key = '******'
	# 构建鉴权对象

	def get_link(localfile, filename):
		try:
			q = Auth(access_key, secret_key)
			# 要上传的空间
			bucket_name = '***'
			# 上传到七牛后保存的文件名
			key = filename
			# 生成上传 Token，可以指定过期时间等
			token = q.upload_token(bucket_name, key, 3600)
			# 要上传文件的本地路径

			ret, info = put_file(token, key, localfile)
		except Exception as e:
			current_app.logger.error(e)
		print(info)
		try:
			assert ret['key'] == key
			assert ret['hash'] == etag(localfile)
		except Exception as e:
			current_app.logger.info(e)
		# 需要替换成自己的七牛云链接
		return 'https://******'+filename
		
看效果：
![](http://oumkbl9du.bkt.clouddn.com/2017-08-31-V38OY-qiniu.png)
返回的是七牛云的外链，点击确定，图片就能增加到文章了。