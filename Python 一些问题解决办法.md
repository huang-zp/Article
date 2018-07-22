### 1.运行任务到后台
	command > /log/file 2>&1 &
	


### 2.ubuntu16.04 shadowsock2.8 不支持chacha20-ietf
解决办法

	sudo apt install libsodium-dev
	
	pip install https://github.com/shadowsocks/shadowsocks/archive/master.zip -U
	
如果还有问题换Python 3 试一下


### 3.根据正则批量删除redid的键值对

	redis-cli keys "celery*" |xargs redis-cli del
	

### 4.根据正则批量删除当前文件夹下的匹配到的文件

	find . -name "*.log*" | xargs rm -rf