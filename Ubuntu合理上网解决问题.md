## SS安装配置
安装

	sudo pip install shadowsocks

编辑json配置文件

	sudo vim shadowsocks.json

配置文件内容

	 {
    "server": "服务器ip",
    "server_port": 服务器端口,
    "local_address": "127.0.0.1",
    "local_port": 1080,
    "password": "SS密码",
    "method": "aes-256-cfb",
    "fast_open": true,
    "timeout":300
	}

启动

	sudo sslocal -c shadowsocks.json

## Privoxy安装配置
Shadowsocks 使用的是 SOCKS5 类型的代理，需要自己将 HTTP 转换为 SOCKS5 ，使用 Privoxy 可以解决
安装privoxy

	sudo apt-get install privoxy

编辑privoxy配置文件

	vim /etc/privoxy/config
4.2节末尾增加

	listen-address localhost:8118

监听本地端口为8118的连接
5.2节末尾增加

	forward-socks5 / 127.0.0.1:1080 .

把 HTTP 流量转发到本机 127.0.0.1:1080 的 Shadowsocks
重启 privoxy

重新启动privoxy
	sudo /etc/init.d/privoxy restart
	
## Frixbox代理配置
配置firebox
![](http://oumkbl9du.bkt.clouddn.com/2018-01-31-xhFsB-2018-01-31_10-28-08.png)

result:
![](http://oumkbl9du.bkt.clouddn.com/2018-01-31-75uDx-2018-01-31_10-29-46.png)

## Chorme代理配置
配置chorme
安装插件SwitchyOmega,配置：
![](http://oumkbl9du.bkt.clouddn.com/2018-02-02-XpPHQ-2018-02-02_16-20-00.png)
在浏览器右上角启用：
![](http://oumkbl9du.bkt.clouddn.com/2018-02-02-ASxbF-2018-02-02_16-20-37.png)

result：
![](http://oumkbl9du.bkt.clouddn.com/2018-02-02-BFfft-2018-02-02_16-19-44.png)


## 终端配置
终端使用

	# 根据系统shell or ~/.bashrc
	vim ~/.zshrc  
增加

	export http_proxy="127.0.0.1:8118"
	export https_proxy="127.0.0.1:8118"
	export ftp_proxy="127.0.0.1:8118"
	
![](http://oumkbl9du.bkt.clouddn.com/2018-01-31-qQKBo-2018-01-31_10-41-14.png)
重新启动

	source ~/.zshrc
![](http://oumkbl9du.bkt.clouddn.com/2018-01-31-rYJ72-2018-01-31_10-42-53.png)

## 亦或全局设置

直接设置系统全局：
![](http://oumkbl9du.bkt.clouddn.com/2018-01-31-8wAeH-2018-01-31_10-43-53.png)

## 开机自启

	vim /etc/rc.local

增加内容：

	sudo sslocal -c /etc/shadowsocks.json
	sudo /etc/init.d/privoxy start

	
![](http://oumkbl9du.bkt.clouddn.com/2018-01-31-cbprp-2018-01-31_10-48-54.png)