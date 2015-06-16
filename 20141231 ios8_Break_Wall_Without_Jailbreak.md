#IOS8 Break Wall Without Jailbreak

参考文章：

[http://emptyzone.github.io/tech/2014/10/13/cross-fire-wall-on-ios8/](http://emptyzone.github.io/tech/2014/10/13/cross-fire-wall-on-ios8/)
[https://medium.com/@cattyhouse/ios-ondemand-ipsec-vpn-setup-ebfb82b6f7a1](https://medium.com/@cattyhouse/ios-ondemand-ipsec-vpn-setup-ebfb82b6f7a1)

关键词：IPsec VPN IKEv1 IOS Debian

说明：本文中的 \_NAME\_, \_VPS_IP\_, \_EMAIL\_, \_PASSWORD\_ 为占位符，替换成你自己的配置

---

## 0 缘起

本来用shadowsocks上网已经非常安逸，几乎体会不到墙带来的阻隔了，但shadowsocks只能在桌面端使用，在ios手机平板上用必须越狱才行。

在折腾了两天时间，经历过一次把vps彻底搞坏的情况，并且尝试了几个方案，本方案最终成功了。测试了下上网，看视频，没有出现多大的问题。

所以总结了一下，方便自己以后再次折腾，也方便他人吧。

2014年的最后一天，当作新年礼物送给自己。

我的服务器vps托管在buyvm，价格不错，稳定。系统是Debian 7 32bit。

---

## 1 编译 strongswan（基于 OpenVZ 的 VPS）

### 1.1 下载最新软件包

	$wget http://download.strongswan.org/strongswan-5.2.1.tar.bz2
	$tar xvf strongswan* 
	$cd strongswan*

### 1.2 安装编译所需的包

	$apt-get install libgmp3-dev openssl  libssl-dev

我用的机器是OpenVZ 要加上 --enable-kernel-libipsec 参数：

	$./configure --sysconfdir=/etc --disable-sql --disable-mysql --disable-ldap --enable-dhcp --enable-eap-identity --enable-eap-mschapv2 --enable-md4 --enable-xauth-eap --enable-eap-peap --enable-eap-md5 --enable-openssl --enable-shared --enable-unity --enable-eap-tls   --enable-eap-ttls --enable-eap-tnc --enable-eap-dynamic --enable-addrblock --enable-radattr --enable-nat-transport --enable-kernel-netlink --enable-kernel-libipsec
	$make
	$make install

---

## 2 生成证书

### 2.1 建立证书文件夹

	$mkdir ~/ipsec_cert && cd ~/ipsec_cert

### 2.2 生成服务器证书

	$wget https://gist.githubusercontent.com/songchenwen/14c1c663ea65d5d4a28b/raw/cef8d8bafe6168388b105f780c442412e6f8ede7/server_key.sh
	$chmod +x server_key.sh
	$./server_key.sh _VPS_IP_

### 2.3 生成客户端证书

	$wget https://gist.githubusercontent.com/songchenwen/14c1c663ea65d5d4a28b/raw/54843ae2e5e6d1159134cd9a90a08c31ff5a253d/client_key.sh
	$chmod +x client_key.sh
	$./client_key.sh _NAME_ _EMAIL_

### 2.4 执行完成后

可以把以用户名开头的 .p12 文件 和 cacerts/strongswanCert.pem 拷贝到web服务器的downloads目录中，我用的是nginx，在nginx里打开autoindex就可以了，在nginx.conf中添加代码:
	
	/downloads {
		autoindex on;
	}

### 2.5 把私钥拷贝到程序目录

	$sudo cp cacerts/strongswanCert.pem /etc/ipsec.d/cacerts/strongswanCert.pem
	$sudo cp certs/vpnHostCert.pem /etc/ipsec.d/certs/vpnHostCert.pem
	$sudo cp private/vpnHostKey.pem /etc/ipsec.d/private/vpnHostKey.pem

---

## 3. 配置文件

### 3.1 编辑 /etc/ipsec.conf

	$vim /etc/ipsec.conf
	config setup
		# strictcrlpolicy=yes
		#  uniqueids = replace
		# charondebug="cfg 2, dmn 2, ike 2, net 0" #要看Log时，取消注释本行
	conn %default
		keyexchange=ikev1
		dpdaction=hold
		dpddelay=600s
		dpdtimeout=5s
		lifetime=24h
		ikelifetime=240h
		rekey=no
		left=_VPS_IP_ #这里换成你登录 VPN 用的域名或 IP，与生成证书时相同 
		leftsubnet=0.0.0.0/0
		leftcert=vpnHostCert.pem
		leftsendcert=always
		right=%any
		rightdns=8.8.8.8
		rightsourceip=10.0.0.0/8
	conn CiscoIPSec
		rightauth=pubkey
		rightauth2=xauth
		auto=add

### 3.2 编辑 /etc/ipsec.secrets

	$vim /etc/ipsec.secrets 
	: RSA vpnHostKey.pem
	_NAME_ : EAP "_PASSWORD_"

（插曲：开始忘记了加入第一行，导致后面一直不成功，后来分析/var/log/daemon.log找到了原因，见倒数第三行）

![tail /var/log/daemon.log](https://www.dropbox.com/s/4mlhschc4v8whcv/BJ11~%7BU%7DOG65K%40BR8%5DG8A%7BJ.jpg?dl=0)

### 3.3 配置防火墙 iptables

重要的是开启NAT转发, 开放4500, 500端口,esp协议, Web Server端口80, 443，Shadowsocks端口。

	$sudo vim /etc/iptables.firewall.rules
	*filter
	#  Allow all loopback (lo0) traffic and drop all traffic to 127/8 that doesn't use lo0
	-A INPUT -i lo -j ACCEPT
	-A INPUT -d 127.0.0.0/8 -j LOG --log-prefix "looback denied: " --log-level 7
	-A INPUT -d 127.0.0.0/8 -j REJECT
	#  Accept all established inbound connections
	-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
	#  Allow all outbound traffic - you can modify this to only allow certain traffic
	-A OUTPUT -j ACCEPT
	#  Allow HTTP and HTTPS connections from anywhere (the normal ports for websites and SSL).
	-A INPUT -p tcp --dport 80 -j ACCEPT
	-A INPUT -p tcp --dport 443 -j ACCEPT
	# Allow Shadowsocks
	-A INPUT -p tcp --dport 8979 -j ACCEPT
	# Allow ipsec
	-A INPUT -p udp --dport 4500 --j ACCEPT
	-A INPUT -p udp --dport 500 --j ACCEPT
	-A INPUT -p esp -j ACCEPT
	#  Allow SSH connections
	#  The -dport number should be the same port number you set in sshd_config
	-A INPUT -p tcp -m state --state NEW --dport 22 -j ACCEPT
	#  Allow ping
	-A INPUT -p icmp -j ACCEPT
	#  Log iptables denied calls
	-A INPUT -m limit --limit 5/min -j LOG --log-prefix "iptables denied: " --log-level 7
	-A INPUT -j DROP
	COMMIT

### 3.4 在 /etc/sysctl.conf 中开启 net.ipv4.ip_forward=1

	$vim /etc/sysctl.conf 
	
### 3.5 编辑 /etc/network/if-pre-up.d/firewall
	
	$vim /etc/network/if-pre-up.d/firewall
	#!/bin/sh 
	/sbin/iptables-restore < /etc/iptables.firewall.rules
	sudo chmod +x /etc/network/if-pre-up.d/firewall

### 3.6 编辑 /etc/rc.local 在 exit 0 前加上

	$vim /etc/rc.local
	iptables -t nat -A POSTROUTING -j MASQUERADE
	iptables -A FORWARD -p tcp --tcp-flags SYN,RST SYN -j TCPMSS --clamp-mss-to-pmtu
	iptables -A POSTROUTING -t nat -s 10.0.0.0/8 -j SNAT --to-source _VPS_IP_

### 3.7 重启服务器

	$sudo reboot
	
### 3.8 查看iptables设置

	$sudo iptables -L

### 3.9 将ipsec设置开机自启

	$sudo vim /etc/rc.local 
	在 exit 0 前加上 ipsec start

---

## 4 手机/平板端设置

### 4.1 用iphone浏览器访问vps网站

进入downlaods目录，里面有两个证书文件，分别安装，其中的密码是生成密钥时提供的密码。

### 4.2 添加VPN

如果以前没有添加过VPS，需要进入 设置 - 通用 - VPN - 添加VPN设置，选择最右边的IPSec：

	描述：任意填写
	服务器：_VPS_IP_
	账户：_NAME_
	密码：_PASSWORD_
	使用证书：打开
	证书：选择刚才添加的证书
	
### 4.3 保存，可以开始Enjoy了。



