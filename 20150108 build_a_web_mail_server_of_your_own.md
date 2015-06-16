Build A Web Mail Server Of Your Own

#搭建一个自己的web mail服务器

说明：过程很简单，Linux命令熟练的话，10分钟不到就可以完全弄好了。我没有设置二级域名，只在主域名下设置了一个目录，因为这样不用改DNS设置。我的vps服务器是Debian 7 32bit + Nginx + PHP + MySQL。

##0.参考

[http://www.v2ex.com/t/160066](http://www.v2ex.com/t/160066)

[http://rainloop.net/docs](http://rainloop.net/docs)


##1.下载安装包

ssh登陆VPS服务器，在nginx的web目录下新建一个文件夹rain，这里我的web目录是～blog：

	cd ~blog && mkdir rain
	curl -O http://repository.rainloop.net/v2/webmail/rainloop-latest.zip
	unzip rainloop-latest.zip
安装完毕，就是如此简单。

以上是下载安装包安装的，[官网文档](http://rainloop.net/docs/installation/)还说明可以用在线安装：

	cd ~blog && mkdir rain
	curl -s http://repository.rainloop.net/installer.php | php
	
都是一样的	

##2.修复权限

为了安全问题，需要修复文件权限，[官网文档](http://rainloop.net/docs/permissions/)：

	find . -type d -exec chmod 755 {} \;
	find . -type f -exec chmod 644 {} \;
	chown -R www:www .

##3.配置

由于是代收邮件，所有的配置信息、邮件、附件等都是保存在data目录下，所以为了安全，需要设置相应的访问权限。我用的web服务器是nginx，要按照如下设置，如果是其它的如apache，文档里有详细说明，我就不多说了。

	vim /usr/local/nginx/conf/nginx.conf

添加

	location ^~ /rain/data {
  		deny all;
	}
	
测试修改是否错误

	nginx -t
	
没有错误，重新载入是修改生效

	service nginx reload

软件的配置页面在地址 http://www.crystone.me/rain?admin （其实就是URL后面加上?admin）

默认用户名和密码分别是admin, 12345

安全起见，安装完毕后，需要马上修改，

其它设置，在[官方文档](http://rainloop.net/docs/configuration)有详细说明，自己摸索着更改。

所有的配置信息都在如下文件里面：

	~blog/rain/data/_data_3046846f7d2177be5a00192d01f3a68d/_default_/configs/application.ini
	
中间的一长串字符，是自动生成的，每次安装都不同。

##4.使用

官方说明可以用IMAP和SMTP等收发邮件，我的vps没有配置ssl所以暂时不行。软件还提供邮件加密，两步认证等，自己需要的话可以加上。

另外，提供了几个[不错的插件](http://rainloop.net/docs/plugins/)，可以按需添加，或者自己写一个？

最后放出我配置好的登陆页面和邮件截图

![登陆界面](http://upload-images.jianshu.io/upload_images/28724-702d7fe54040be74.png)

![邮件界面](http://upload-images.jianshu.io/upload_images/28724-152ef209c00154fe.png)

大家可以自己搭建一个，多一个渠道使用Gmail服务。

Have Fun!
