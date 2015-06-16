#一些常用的git命令记录

## 常用命令

### git init

### git add

### git commit

### git status

### git diff

### git log

### git reset

### git checkout

### git rm

### git remote add origin

### git push

### git pull

### git fetch

### git clone 

### git checkout && git branch

### git merge

### git tag

### git config

### .gitignore

### 


## 用户配置

	git config --global user.name ""

	git config --global user.email ""
	
	git config --list

## 初始化

	git init
	
### 从现有仓库克隆

	git clone git@github.com:crystoneme/Markdown.git
	
### 查看当前文件状态

	git status
	
### 跟踪新文件

	git add 

### 查看尚未暂存的问题的更新部分

	git diff
	
### 查看已经暂存和上次提交的差异

	git diff --cached
	
### 提交更新

	git commit
	
### 跳过暂存步骤，直接提交更新的文件

	git commit -a

### 移除文件

	git rm
	
#### 删除已经暂存的文件，要用强制选项

	git rm -f

#### 将文件从跟踪清单中移除

	git rm --cached

### 移动文件（文件改名）

	git mv
	
### 查看提交历史

	git log
	git log -p //每次提交内容的差异
	git log --stat //增改行数统计

### 撤销操作

	git commit --amend //撤销刚才的提交，重新提交
	git reset HEAD file //取消某个文件的暂存操作
	git checkout -- file //取消文件的修改

### 远程仓库

#### 查看
	git remote

#### 添加远程仓库
	git remote add [name] url

#### 从远程仓库抓取数据
	git fetch [name] //只是抓取，需要手动合并
	git pull [name] // 抓取后自动合并
	
#### 推送到远程仓库
	git push [remote name] [branch name]
	git push origin master

#### 查看远程仓库信息
	git remote show [remote name]
	git remote show origin
	
#### 远程仓库删除，重命名
	git remote rename [old name] [new name]
	gir remote rm [name]
	
### 标签

#### 列出标签
	git tag
	
#### 新建标签(轻量级标签和含附注标签)
	git tag -a v1.1 -m "version 1.1"
	
#### 查看标签版本信息
	git show v1.1
	
#### 签署标签
	git tag -s v1.1 -m "version 1.1 with signed"

#### 轻量级标签 
	git tag v1.1
	
#### 验证标签
	git tag -v v1.1

#### 将标签推送到远程
	git push origin v1.1
	git push origin --tags //一次推送所有标签

### 有用tips
#### 别名
	git config --global alias.co checkout
	git config --global alias.br branch
	git config --global alias.ci commit
	git config --global alias.st status
	git config --global alias.unstage 'reset HEAD --'
	git config --global alias.last 'log -1 HEAD'

### 分支
#### 切换分支
	git branch -b [branch]//等于下面两条命令
	git branch [branch]
	git checkout [branch]
#### 合并分支
	git checkout master
	git merge //快进合并，如果可能
#### 删除分支
	git branch -d [branch]
	
#### 分支管理
	git branch --merged //哪些分支已并入当前分支
	git branch --no-merged //尚未合并的工作
	
### 远程分支
	用[远程仓库]/[分支名]表示远程分支
	
#### 推送远程分支
	git push [远程仓库] [分支名] // git push origin hotfix
	git push origin hotfix1:hotfix2 // 将本地分支hotfix1推送到远程分支hotfix2

#### 获取远程分支
	git fetch origin
	
#### 在远程分支基础上分化自己的分支(跟踪分支)
	git checkout -b [分支名] [远程仓库]/[分支名]  //git checkout -b fix origin/fix
	git checkout --track origin/fix //简化版
	
#### 删除远程分支
	git push [远程仓库] :[分支名] //git push origin :fix
	
### merge和rebase

	git checkout fix //在这个分支里参生的提交
	git rebase master //在这个分支里重放一遍
	git checkout master //回到主分支
	git merge fix //快进合并
	
	以上命名可以写作：
	git rebase master fix
	git merge fix
	
	git rebase --onto //这个不理解
	
	要把rebase当作在推送前清理提交历史的手段，且仅仅rebase那些永远不会公开的commit
	

	
 



