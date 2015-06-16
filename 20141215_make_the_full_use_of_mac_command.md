##make the full use of Mac
---

###打开iterm
	点屏幕右上角或者快捷键command+space，输入“终端”或者"terminal"，进入系统自带终端。这个终端只是临时用，后面会安装更强大的终端iTerm2代替它的。

###安装[xcode command line tools](#)
```$xcode-select --install```

###安装[homebrew](http://brew.sh/)
```$ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"```

###安装[brew cask](http://caskroom.io/)
```$brew tap phinze/homebrew-cask```

```$brew install brew-cask```

###安装[iterm2](http://iterm2.com/)代替系统的iterm
```$brew cask install iterm2```

	之后的命令可以在iterm2中进行

###安装编辑器[text mate](https://github.com/textmate/textmate)
```$brew cask install textmate```

###安装软件[shadowsocks GUI](http://sourceforge.net/projects/shadowsocksgui/)

使用方法：[Shadowsocks for OSX 帮助](https://github.com/shadowsocks/shadowsocks-iOS/wiki/Shadowsocks-for-OSX-%E5%B8%AE%E5%8A%A9)

###安装[proxychain4](https://github.com/haad/proxychains)
```$brew install proxychains-ng```

	配置proxychain4
	$mate /usr/local/Cellar/proxychains-ng/4.7/etc/proxychains.conf
	加上下面一行
	socks5 	127.0.0.1 1080
	
###安装[youtube-dl](http://rg3.github.io/youtube-dl/)
```$brew cask install youtube-dl```

###安装zsh

####zsh配置文件.zshrc

###安装

###用键盘快捷键控制窗口
```$brew cask install spectacle```

	然后设置快捷键，我的快捷键是：
	
###spacers分割dock
	defaults write com.apple.dock persistent-apps -array-add '{tile-data={}; tile-type="spacer-tile";}'

	killall Dock

###Go2Shell在finder打开iterm
	要打开Go2Shell的Preference页面，在shell里面敲
	open -a Go2Shell --args config

###cheat：iterm下命令行好帮手
需要有安装python和pip

```$git clone https://github.com/chrisallenlane/cheat.git```

$cd cheat# python setup.py install

$cd cheat

$python setup.py install

$wget https://github.com/chrisallenlane/cheat/raw/master/cheat/autocompletion/cheat.bash

$mv cheat.bash ~/


###tmux
命令行多窗口使用利器


###下面博客的文章都可以看一下
http://www.yangzhiping.com/tech/homebrew-cask.html

http://venmos-com.qiniudn.com/blog/2013/06/18/cli-proxy/

http://heylinux.com/archives/2933.html

http://www.cnblogs.com/chijianqiang/

http://macshuo.com/


