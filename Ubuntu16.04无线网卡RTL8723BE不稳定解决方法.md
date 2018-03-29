> **Ubuntu16.04无线连接的时候会掉线，而且掉了之后就再也连不上了，只能重启。并且无线信号差。原因来源于无线网卡RTL8723BE在ubuntu环境下的不稳定**

## 查看本机无线网卡型号：

	lspci

![](http://oumkbl9du.bkt.clouddn.com/2018-03-04-UGKAb-2018-03-04_09-38-18.png)

倒数第二行： **RTL8723BE PCIe Wireless Network Adapter**
如果网卡型号没错，并且也有这毛病就应该是同一个问题

## 查看RTL8723BE这块网卡驱动的参数信息：

	modinfo rtl8723be

![](http://oumkbl9du.bkt.clouddn.com/2018-03-04-MOMsn-2018-03-04_09-43-55.png)

ips和fwlps控制节能
ant_sel控制信号强度


## 编辑驱动文件：

	sudo nano /etc/modprobe.d/rtl8723be.conf
	
添加以下配置：

	options rtl8723be debug=1
	options rtl8723be disable_watchdog=N
	options rtl8723be fwlps=Y
	options rtl8723be ips=Y
	options rtl8723be msi=N
	options rtl8723be swenc=N
	options rtl8723be swlps=N
	options rtl8723be ant_sel=2


![](http://oumkbl9du.bkt.clouddn.com/2018-03-04-JTufD-2018-03-04_09-47-37.png)

## 应用配置信息

	sudo modprobe -r rtl8723be
	sudo modprobe rtl8723be