0112 A Great Tools to Download Youtube Videos

介绍一个在命令行下载yutube视频的工具

##1.工具
[youtube-dl](https://yt-dl.org/)下载youtube视频工具

[ffmpeg](https://www.ffmpeg.org/)视频合并编码等

[proxychains4](https://github.com/rofl0r/proxychains-ng)命令行代理

各平台都有这些工具，具体安装自己看官网说明。

推荐这几个工具的原因是在本地端和VPS远程端都可以使用

##2.远程端使用

登陆VPS后

	youtube-dl -F https://www.youtube.com/watch?v=_UxieGouKP4
	
命令可以查看服务器上多个码率的视频，可以根据需要选择自己想要的下载：

	youtube-dl -F https://www.youtube.com/watch\?v\=_UxieGouKP4
	[youtube] _UxieGouKP4: Downloading webpage
	[youtube] _UxieGouKP4: Extracting video information
	[youtube] _UxieGouKP4: Downloading DASH manifest
	[info] Available formats for _UxieGouKP4:
	format code extension resolution  note
	139         m4a       audio only  DASH audio   48k , audio@ 48k (22050Hz), 1.14MiB (worst)
	171         webm      audio only  DASH audio  116k , audio@128k (44100Hz), 2.56MiB
	140         m4a       audio only  DASH audio  128k , audio@128k (44100Hz), 3.05MiB
	172         webm      audio only  DASH audio  160k , audio@256k (44100Hz), 3.48MiB
	141         m4a       audio only  DASH audio  256k , audio@256k (44100Hz), 6.11MiB
	278         webm      256x144     DASH video   68k , webm container, VP9, 1fps, video only, 811.62KiB
	160         mp4       256x144     DASH video  118k , 15fps, video only, 2.26MiB
	242         webm      426x240     DASH video  100k , 1fps, video only, 1.04MiB
	133         mp4       426x240     DASH video  247k , 30fps, video only, 2.92MiB
	243         webm      640x360     DASH video  169k , 1fps, video only, 1.81MiB
	134         mp4       640x360     DASH video  239k , 30fps, video only, 3.71MiB
	244         webm      854x480     DASH video  301k , 1fps, video only, 2.71MiB
	135         mp4       854x480     DASH video  520k , 30fps, video only, 7.40MiB
	247         webm      1280x720    DASH video  525k , 1fps, video only, 4.56MiB
	136         mp4       1280x720    DASH video 1126k , 30fps, video only, 13.89MiB
	248         webm      1920x1080   DASH video  926k , 1fps, video only, 8.56MiB
	137         mp4       1920x1080   DASH video 2046k , 30fps, video only, 22.28MiB
	17          3gp       176x144
	36          3gp       320x240
	5           flv       400x240
	43          webm      640x360
	18          mp4       640x360
	22          mp4       1280x720    (best)

我要选择的是最清晰的版本，但发现视频和音频是分开的，选项-F使用说明：

	-F, --list-formats           list all available formats

选项-f使用说明：
	
	-f, --format FORMAT          video format code, specify the order of
	                             preference using slashes, as in -f 22/17/18
	                             .  Instead of format codes, you can select
	                             by extension for the extensions aac, m4a,
	                             mp3, mp4, ogg, wav, webm. You can also use
	                             the special names "best", "bestvideo",
	                             "bestaudio", "worst".  By default, youtube-
	                             dl will pick the best quality. Use commas
	                             to download multiple audio formats, such as
	                             -f
	                             136/137/mp4/bestvideo,140/m4a/bestaudio.
	                             You can merge the video and audio of two
	                             formats into a single file using -f <video-
	                             format>+<audio-format> (requires ffmpeg or
	                             avconv), for example -f
	                             bestvideo+bestaudio.
	                             
 可以视频和音频文件分别下载，然后用ffmpeg工具自动合成。
 
 比如下载上面的视频要下面的命令：
 
	youtube-dl -f 137+141 https://www.youtube.com/watch\?v\=_UxieGouKP4                    [23:09:35]
	[proxychains] config file found: /usr/local/Cellar/proxychains-ng/4.7/etc/proxychains.conf
	[proxychains] preloading /usr/local/Cellar/proxychains-ng/4.7/lib/libproxychains4.dylib
	[youtube] _UxieGouKP4: Downloading webpage
	[youtube] _UxieGouKP4: Extracting video information
	[youtube] _UxieGouKP4: Downloading DASH manifest
	[download] Destination: Assassins Creed - Brotherhood - Original Game Soundtrack  - 01. Master Assassin-_UxieGouKP4.f137.mp4
	[download] 100% of 22.28MiB in 00:42
	[download] Destination: Assassins Creed - Brotherhood - Original Game Soundtrack  - 01. Master Assassin-_UxieGouKP4.f141.m4a
	[download] 100% of 6.11MiB in 00:09
	[ffmpeg] Merging formats into "Assassins Creed - Brotherhood - Original Game Soundtrack  - 01. Master Assassin-_UxieGouKP4.mp4"
 
已经自动合并完成了。

##3.本地端使用

本地端也可以用上面的方式下载，但还是下载小的视频为好，建议大的文件最好ssh登陆vps上下载，然后放在web服务器上中转一下，然后本地用下载工具下载。我下载的Handmade Hero系列视频就是这样的步骤。

这里要提一下的是本地命令行代理工具，我用的是shadowsocks代理，在命令行里面有个很好的工具[proxychains-ng](https://github.com/rofl0r/proxychains-ng)

具体安装请参考官网，在mac上brew安装的配置文件位于

	/usr/local/Cellar/proxychains-ng/4.7/etc/proxychains.conf

本地下载上面的youtube视频：

	proxychains4 youtube-dl -f 137+141 https://www.youtube.com/watch\?v\=_UxieGouKP4                   
	[proxychains] config file found: /usr/local/Cellar/proxychains-ng/4.7/etc/proxychains.conf
	[proxychains] preloading /usr/local/Cellar/proxychains-ng/4.7/lib/libproxychains4.dylib
	[youtube] _UxieGouKP4: Downloading webpage
	[youtube] _UxieGouKP4: Extracting video information
	[youtube] _UxieGouKP4: Downloading DASH manifest
	[download] Destination: Assassins Creed - Brotherhood - Original Game Soundtrack  - 01. Master Assassin-_UxieGouKP4.f137.mp4
	[download] 100% of 22.28MiB in 00:42
	[download] Destination: Assassins Creed - Brotherhood - Original Game Soundtrack  - 01. Master Assassin-_UxieGouKP4.f141.m4a
	[download] 100% of 6.11MiB in 00:09
	[ffmpeg] Merging formats into "Assassins Creed - Brotherhood - Original Game Soundtrack  - 01. Master Assassin-_UxieGouKP4.mp4"

##4.其它

youtube-dl不仅支持下载youtube网站视频，还能下载一堆视频网站的视频，[支持列表在](http://rg3.github.io/youtube-dl/supportedsites.html)

youtube-dl还可以下载youtube播放列表，限速等高级用法，具体设置，查看帮助文件：

	youtube-dl --help

Have Fun!




                            