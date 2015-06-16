#Install Docker Under Mac OS



##1.安装virtualbox
	
	brew cask install virtualbox

##2.安装boot2docker
	
	brew install boot2docker

##3.初始化boot2docker,下载镜像

	boot2docker init
	
##4.启动boot2docker

	boot2docker start

##5.测试
	
	docker run hello-world
	

##6.下载镜像ubuntu14.04

	docker pull dl.dockerpool.com:5000/ubuntu:14.04

	docker tag dl.dockerpool.com:5000/ubuntu:14.04 ubuntu:14.04

证书问题：

	DOCKER_OPTS="--insecure-registry dl.dockerpool.com:5000"



##卸载

	boot2docker delete
	rm -rf ~/.boot2docker
	brew uninstall boot2docker

##参考

https://github.com/boot2docker/osx-installer/releases/latest
http://yansu.org/2014/04/10/install-docker-in-mac.html
http://docs.docker.com/installation/mac/
http://www.oschina.net/translate/installing-docker-on-mac-os-x
https://github.com/boot2docker/boot2docker/releases/download/v1.4.1/boot2docker.iso

http://viget.com/extend/how-to-use-docker-on-os-x-the-missing-guide

https://github.com/discourse/discourse_docker
http://steveltn.me/blog/2014/03/15/deploy-rails-applications-using-docker/
http://timo.piqiu.me/2014/05/26/%E4%BD%BF%E7%94%A8docker%E5%8F%91%E5%B8%83rails%E7%A8%8B%E5%BA%8F/



---

Levi@Levi-MBP: ~/Downloads
 $ brew info boot2docker                                             [15:09:13]
boot2docker: stable 1.4.1 (bottled), HEAD
https://github.com/boot2docker/boot2docker-cli
Not installed
From: https://github.com/Homebrew/homebrew/blob/master/Library/Formula/boot2docker.rb
==> Dependencies
Build: go ✘
Recommended: docker ✘
==> Options
--without-docker
	Build without docker support
--HEAD
	Install HEAD version
==> Caveats
To have launchd start boot2docker at login:
    ln -sfv /usr/local/opt/boot2docker/*.plist ~/Library/LaunchAgents
Then to load boot2docker now:
    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.boot2docker.plist

Levi@Levi-MBP: ~/Downloads
 $ brew install boot2docker                                          [15:10:14]
==> Installing boot2docker dependency: docker
==> Downloading https://downloads.sf.net/project/machomebrew/Bottles/docker-1.4.
######################################################################## 100.0%
==> Pouring docker-1.4.1.mavericks.bottle.tar.gz
==> Caveats
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d

zsh completion has been installed to:
  /usr/local/share/zsh/site-functions
==> Summary
🍺  /usr/local/Cellar/docker/1.4.1: 9 files, 7.0M
==> Installing boot2docker
==> Downloading https://downloads.sf.net/project/machomebrew/Bottles/boot2docker-1.4.1.m
######################################################################## 100.0%
==> Pouring boot2docker-1.4.1.mavericks.bottle.tar.gz
==> Caveats
To have launchd start boot2docker at login:
    ln -sfv /usr/local/opt/boot2docker/*.plist ~/Library/LaunchAgents
Then to load boot2docker now:
    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.boot2docker.plist
==> Summary
🍺  /usr/local/Cellar/boot2docker/1.4.1: 3 files, 7.3M

Levi@Levi-MBP: ~/Downloads
 $ boot2docker init                                                          [15:10:44]
Latest release for boot2docker/boot2docker is v1.4.1
Downloading boot2docker ISO image...
Success: downloaded https://github.com/boot2docker/boot2docker/releases/download/v1.4.1/boot2docker.iso
	to /Users/Levi/.boot2docker/boot2docker.iso
Generating public/private rsa key pair.
Your identification has been saved in /Users/Levi/.ssh/id_boot2docker.
Your public key has been saved in /Users/Levi/.ssh/id_boot2docker.pub.
The key fingerprint is:
83:53:a6:f6:68:ff:23:c0:85:d2:9a:f4:2a:93:5f:be Levi@Levi-MBP
The key's randomart image is:
+--[ RSA 2048]----+
|                 |
|                 |
|     . .o        |
|    o o=.        |
|   . *=.S        |
|    o.++ .       |
|   . .+..        |
|  + .+ .. .      |
|   +. E..o..     |
+-----------------+

Levi@Levi-MBP: ~/Downloads
 $ boot2docker start                                                         [15:13:02]
Waiting for VM and Docker daemon to start...
........................oooooooooooooooooooooo
Started.
Writing /Users/Levi/.boot2docker/certs/boot2docker-vm/ca.pem
Writing /Users/Levi/.boot2docker/certs/boot2docker-vm/cert.pem
Writing /Users/Levi/.boot2docker/certs/boot2docker-vm/key.pem

To connect the Docker client to the Docker daemon, please set:
    export DOCKER_HOST=tcp://192.168.59.103:2376
    export DOCKER_CERT_PATH=/Users/Levi/.boot2docker/certs/boot2docker-vm
    export DOCKER_TLS_VERIFY=1

Levi@Levi-MBP: ~/Downloads
 $ export DOCKER_HOST=tcp://192.168.59.103:2376                                           [15:14:17]

Levi@Levi-MBP: ~/Downloads
 $     export DOCKER_CERT_PATH=/Users/Levi/.boot2docker/certs/boot2docker-vm              [15:15:00]

Levi@Levi-MBP: ~/Downloads
 $     export DOCKER_TLS_VERIFY=1                                                         [15:15:06]

Levi@Levi-MBP: ~/Downloads
 $ $(boot2docker shellinit)                                                               [15:15:10]
Writing /Users/Levi/.boot2docker/certs/boot2docker-vm/ca.pem
Writing /Users/Levi/.boot2docker/certs/boot2docker-vm/cert.pem
Writing /Users/Levi/.boot2docker/certs/boot2docker-vm/key.pem

Levi@Levi-MBP: ~/Downloads
 $ docker run hello-world                                                                 [15:15:23]
Unable to find image 'hello-world:latest' locally
hello-world:latest: The image you are pulling has been verified
511136ea3c5a: Pull complete
31cbccb51277: Pull complete
e45a5af57b00: Pull complete
Status: Downloaded newer image for hello-world:latest
Hello from Docker.
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (Assuming it was not already locally available.)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

For more examples and ideas, visit:
 http://docs.docker.com/userguide/

Levi@Levi-MBP: ~/Downloads
 $ docker version                                                                         [15:16:26]
Client version: 1.4.1
Client API version: 1.16
Go version (client): go1.4
Git commit (client): 5bc2ff8
OS/Arch (client): darwin/amd64
Server version: 1.4.1
Server API version: 1.16
Go version (server): go1.3.3
Git commit (server): 5bc2ff8
                                                                                   [15:19:39]