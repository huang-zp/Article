> **使用zsh取代默认bash，配置oh-my-zsh。安装pyenv管理python版本。安装nvm管理node版本**

## zsh and oh-my-zsh

### zsh
* sudo apt-get install zsh
* touch ~/.zshrc
* 注销，重新登录
![](http://oumkbl9du.bkt.clouddn.com/2018-01-28-J3UdQ-2018-01-28_17-12-00.png)

### oh-my-zsh
* sudo apt-get install git
* sudo apt-get install curl
* sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
更换主题

* vim ~/.zshrc
![](http://oumkbl9du.bkt.clouddn.com/2018-01-28-vUIrK-2018-01-28_17-24-03.png)
 更换bira主题
 更多主题：https://github.com/robbyrussell/oh-my-zsh/wiki/Themes
 
 效果：
 ![](http://oumkbl9du.bkt.clouddn.com/2018-01-28-6Y1Dh-2018-01-28_17-28-01.png)
 
## Pyenv

使用pyenv-installer

	curl -L https://raw.githubusercontent.com/pyenv/pyenv-installer/master/bin/pyenv-installer | bash

～/.zshrc增加三行（or ~/.bashrc）

	export PATH="~/.pyenv/bin:$PATH"
	eval "$(pyenv init -)"
	eval "$(pyenv virtualenv-init -)"

重新激活

	source ~/.zshrc

安装pyenv依赖：

	sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev xz-utils tk-dev

## Nvm

	curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash
	
～/.zshrc增加（or ~/.bashrc）

	export NVM_DIR="$HOME/.nvm"
	[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" 

重新激活

	source ~/.zshrc