#Solve the problem of downloading HD youtube videos with youtube-dl
 

#解决用youtube-dl下载高清视频的问题

## 0.起

一直在vps上用youtube-dl下载youtube上的视频，用默认选项下载没有任何问题，但当要下高清格式视频时，出现了错误。试过几种方案，只有下面的成功了，记录一下。

debian系统rep上的ffmpeg版本很老，只好自己源码编译了。

关于如何使用youtube-dl下载视频用法，参考我之前的帖子：

## 1.安装编译必备工具

	sudo apt-get update
	sudo apt-get install git build-essential nasm pkg-config
	
一些基本的库

	sudo apt-get install libfaac-dev libmp3lame-dev libtheora-dev libvorbis-dev libopencore-amrnb-dev libopencore-amrwb-dev libgsm1-dev zlib1g-dev libgpac-dev

## 2.编译安装yasm（编译x264用）

	wget http://www.tortall.net/projects/yasm/releases/yasm-1.3.0.tar.gz 
	tar xvfz yasm-1.3.0.tar.gz 
	cd yasm-1.3.0 
	./configure --prefix=/usr/local 
	make 
	sudo make install 

可能会提示找不到 libfaac-dev libgpac-dev
	
	echo "deb http://www.deb-multimedia.org wheezy main non-free" >> /etc/apt/sources.list
	echo "deb-src http://www.deb-multimedia.org wheezy main non-free" >> /etc/apt/sources.list

添加后记得
	
	sudo apt-get update

## 3.编译x264

	git clone https://github.com/mirror/x264.git x264
	cd x264
	./configure --prefix=/usr/local --enable-shared
	make
	sudo make install

## 4.编译libxvid
	
	wget http://downloads.xvid.org/downloads/xvidcore-1.3.3.tar.gz 
	tar xvfz xvidcore-1.3.3.tar.gz 
	cd xvidcore/build/generic 
	./configure --prefix=/usr/local 
	make
	sudo make install

## 5.编译ffmpeg

	git clone https://github.com/FFmpeg/FFmpeg.git ffmpeg
	./configure --prefix=/usr/local --enable-gpl --enable-version3 --enable-nonfree --enable-shared --enable-libopencore-amrnb --enable-libopencore-amrwb --enable-libfaac --enable-libgsm --enable-libmp3lame --enable-libtheora --enable-libvorbis --enable-libx264 --enable-libxvid
	make
	sudo make install

最后编译ffmpeg的过程相当耗时，建议开始编译后找些别的事情做。

## 6.oops

安装完毕，运行ffmpeg，出现如下错误：

	ffmpeg: error while loading shared libraries: libavdevice.so.56: cannot open shared object file: No such file or directory

## 7.解决

	find /usr/local/lib |grep -E "libavdevice.so"
	/usr/local/lib/libavdevice.so.56
	/usr/local/lib/libavdevice.so
	/usr/local/lib/libavdevice.so.56.4.100
	vim /etc/ld.so.conf
有上面条目，可以不用修改

再运行一次

	ldconfig

于是，正常了

## 8.oops again

	youtube-dl -f 137+141 https://www.youtube.com/xxxxxx
	WARNING: Your copy of avconv is outdated, update avconv to version 10-0 or newer if you encounter any errors.
	ERROR: Could not write header for output file #0 (incorrect codec parameters ?)

## 9.解决：升级avconv

	git clone git://git.libav.org/libav.git avconv
	cd avconv
	./configure --enable-gpl --enable-libx264
	make
	sudo make install
	sudo cp avconv avplay avprobe /usr/bin

至此没有任何问题。

## 10.参考

[http://forum.ivorde.com/ffmpeg-error-while-loading-shared-libraries-libavdevice-so-52-cannot-open-shared-object-file-no-t129.html](http://forum.ivorde.com/ffmpeg-error-while-loading-shared-libraries-libavdevice-so-52-cannot-open-shared-object-file-no-t129.html)

[http://mikutc.sinaapp.com/archives/951](http://mikutc.sinaapp.com/archives/951
)

[http://ijays.com/2015/01/how-to-compile-avconv-with-libx264-on-ubuntu-12-04-lts/](http://ijays.com/2015/01/how-to-compile-avconv-with-libx264-on-ubuntu-12-04-lts/)

