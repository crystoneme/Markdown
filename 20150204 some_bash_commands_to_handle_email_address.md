#Some Bash Commands to Handle Email Address
#处理邮件地址的几个bash命令


### 去空格(行首，行尾，Tab，空行)

方法1

	cat mail.txt | sed 's/^[ \t]*//;s/[ \t]*$//' > tmp
	cat tmp > mail.txt

方法2

	perl -pi -e 's/^[\ \t]+|[\ \t]+$//g' mail.txt
	perl -pi -e "s/^\n//" mail.txt
	
方法3（一句搞定）
	
	perl -pi -e 's/^[\ \t]+|[\ \t]+$//g;s/^\n//' mail.txt
	
### 大写转小写

	cat mail.txt | tr A-Z a-z > tmp
	cat tmp > mail.txt
	
### 去重

	awk '!x[$0]++' mail.txt > tmp
	cat tmp > mail.txt
	rm tmp


