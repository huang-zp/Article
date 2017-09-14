>**因为最近需要在Flask项目中集成微信支付，在微信支付的开发过程中需要开发者根据微信返回的url生成二维码，于是发现了qrcode这个简单快捷的二维码生成库


### 安装
	pip install qrcode

根据平台不同有些依赖也要安装，例如colorama，总之提示缺什么 pip装什么
### 命令行生成

qr "Some text" > test.png


### 代码生成

	import qrcode
	img = qrcode.make('Some data here')

### 高阶用法

	import qrcode
	qr = qrcode.QRCode(
		version=1,
		error_correction=qrcode.constants.ERROR_CORRECT_L,
		box_size=10,
		border=4,
	)
	qr.add_data('Some data')
	qr.make(fit=True)

	img = qr.make_image()

解释：
**version**是1到40的整数，用于控制QR码的大小（最小的版本1是21x21矩阵），设置为None或者在qr执行make操作的时候使用fit参数去自动确定大小

**error_correction**指的是纠错容量（我的理解：扫码没有扫完也能成功）
ERROR_CORRECT_L
大约7％以下的错误可以纠正。
ERROR_CORRECT_M（默认）
大约15％以下的错误可以纠正。
ERROR_CORRECT_Q
约25％以下的错误可以纠正。
ERROR_CORRECT_H。
约30％以下的错误可以纠正。

**box_size** 控制像素

**border** 控制边框宽度

### 其他

* 链接
当用一个链接生成二维码的时候，用微信，QQ之类的扫码扫完之后直接进入了这个链接
http://www.huangzp.com 生成的二维码：

![](http://oumkbl9du.bkt.clouddn.com/2017-09-07-MAUoB-qrcode.png)

* 信息量
二维码用来传递信息，但是如果信息太多也是会gg的，我简单测试了一下，2k汉字是最大程度（可能不准确），生成的二维码是很大很紧凑...
![](http://oumkbl9du.bkt.clouddn.com/2017-09-07-oqKzf-bigqrcode.png)

> 二维码知识请参考
https://coolshell.cn/articles/10590.html
