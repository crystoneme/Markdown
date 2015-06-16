Create A Animated GIF For Batch JPG Files

将一批图片保存成GIF动画

##1、需要工具ImageMagick

	brew install ImageMagick

##2、改变分辨率

	find . -iname “*.jpg” -exec convert -strip +profile “*” -quality 85 {} {} \;　
	
##3、改变大小

	find . -iname “*.jpg” -exec convert -resize 50%x50% {} {} \;　

##4、制作gif

	convert -delay 20 -loop 0 *.jpg result.gif
	
未压缩

	ffmpeg -framerate 1/10 -pattern_type glob -i '*.jpg' output.gif 