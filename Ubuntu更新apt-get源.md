> **ubuntu更新apt源的过程，以及各种源的链接**

## ubuntu更新源过程
首先备份默认源列表

	sudo cp /etc/apt/sources.list /etc/apt/sources.list_bak

选择合适的源替换掉sources.list文件的内容，例如163源，就像这样，然后保存

![](http://oumkbl9du.bkt.clouddn.com/2018-01-27-TbgUf-2018-01-28_01-03-53_.png)

接下来运行命令：

	sudo apt-get update

**Everything is all right !**

## 源

### 搜狐源
	deb http://mirrors.sohu.com/ubuntu/ precise main restricted universe multiverse
	deb http://mirrors.sohu.com/ubuntu/ precise-security main restricted universe multiverse
	deb http://mirrors.sohu.com/ubuntu/ precise-updates main restricted universe multiverse
	deb http://mirrors.sohu.com/ubuntu/ precise-proposed main restricted universe multiverse
	deb http://mirrors.sohu.com/ubuntu/ precise-backports main restricted universe multiverse
	deb-src http://mirrors.sohu.com/ubuntu/ precise main restricted universe multiverse
	deb-src http://mirrors.sohu.com/ubuntu/ precise-security main restricted universe multiverse
	deb-src http://mirrors.sohu.com/ubuntu/ precise-updates main restricted universe multiverse
	deb-src http://mirrors.sohu.com/ubuntu/ precise-proposed main restricted universe multiverse
	deb-src http://mirrors.sohu.com/ubuntu/ precise-backports main restricted universe multiverse

### 网易源
	deb http://mirrors.163.com/ubuntu/ precise main restricted universe multiverse
	deb http://mirrors.163.com/ubuntu/ precise-security main restricted universe multiverse
	deb http://mirrors.163.com/ubuntu/ precise-updates main restricted universe multiverse
	deb http://mirrors.163.com/ubuntu/ precise-proposed main restricted universe multiverse
	deb http://mirrors.163.com/ubuntu/ precise-backports main restricted universe multiverse
	deb-src http://mirrors.163.com/ubuntu/ precise main restricted universe multiverse
	deb-src http://mirrors.163.com/ubuntu/ precise-security main restricted universe multiverse
	deb-src http://mirrors.163.com/ubuntu/ precise-updates main restricted universe multiverse
	deb-src http://mirrors.163.com/ubuntu/ precise-proposed main restricted universe multiverse
	deb-src http://mirrors.163.com/ubuntu/ precise-backports main restricted universe multiverse
	
### 搜狐源

	deb http://mirrors.sohu.com/ubuntu/ trusty main restricted universe multiverse
	deb http://mirrors.sohu.com/ubuntu/ trusty-security main restricted universe multiverse
	deb http://mirrors.sohu.com/ubuntu/ trusty-updates main restricted universe multiverse
	deb http://mirrors.sohu.com/ubuntu/ trusty-proposed main restricted universe multiverse
	deb http://mirrors.sohu.com/ubuntu/ trusty-backports main restricted universe multiverse
	deb-src http://mirrors.sohu.com/ubuntu/ trusty main restricted universe multiverse
	deb-src http://mirrors.sohu.com/ubuntu/ trusty-security main restricted universe multiverse
	deb-src http://mirrors.sohu.com/ubuntu/ trusty-updates main restricted universe multiverse
	deb-src http://mirrors.sohu.com/ubuntu/ trusty-proposed main restricted universe multiverse
	deb-src http://mirrors.sohu.com/ubuntu/ trusty-backports main restricted universe multiverse
	
### 清华源

	deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty main restricted universe multiverse
	deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-security main restricted universe multiverse
	deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-updates main restricted universe multiverse
	deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-backports main restricted universe multiverse
	deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-proposed main restricted universe multiverse
	deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty main restricted universe multiverse
	deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-security main restricted universe multiverse
	deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-updates main restricted universe multiverse
	deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-backports main restricted universe multiverse
	deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-proposed main restricted universe multiverse
	
### 台湾源

	deb http://tw.archive.ubuntu.com/ubuntu trusty main restricted universe multiverse
	deb http://tw.archive.ubuntu.com/ubuntu trusty-security main restricted universe multiverse
	deb http://tw.archive.ubuntu.com/ubuntu trusty-updates main restricted universe multiverse
	deb http://tw.archive.ubuntu.com/ubuntu trusty-backports main restricted universe multiverse
	deb http://tw.archive.ubuntu.com/ubuntu trusty-proposed main restricted universe multiverse
	deb-src http://tw.archive.ubuntu.com/ubuntu trusty main restricted universe multiverse
	deb-src http://tw.archive.ubuntu.com/ubuntu trusty-security main restricted universe multiverse
	deb-src http://tw.archive.ubuntu.com/ubuntu trusty-updates main restricted universe multiverse
	deb-src http://tw.archive.ubuntu.com/ubuntu trusty-backports main restricted universe multiverse
	deb-src http://tw.archive.ubuntu.com/ubuntu trusty-proposed main restricted universe multiverse