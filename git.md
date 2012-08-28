# Git基础常用命令
掌握本文中的常用命令，基本上可以覆盖80%的Git日常使用

## 创建版本库
以下命令可以创建版本库    

	mkdir dir
	cd dir
	git init

	git config --global user.name  "yaotian"
	git config --global user.email  "yaotian@gmail.com"


## 加remote repository
	git remote add origin git@github.com:yaotian/doc.git # 设置仓库

“origin”为远程repository的alias,在后面的命令中表示将表示这个远程的repository.

## 查看当前有哪些remote alias
	$ git remote -v
	origin	git@github.com:yaotian/doc.git (fetch)
	origin	git@github.com:yaotian/doc.git (push)

## 查看目前所处的branch分支
	$ git branch
	* master

*表示当前的分支

## 提交代码
	touch README
	git add . # 将当前目录添加到 git 仓库中， 使用 git add -A 则是添加所有改动的文档
	git commit -m "Initial commit"
	
## 把当前分支的代码提交到远程repository
	$ git push origin master
将本地的branch “master” 提交到远程repository "origin"


### Note:  
当git push的时候会出现这个问题

	! [rejected]        master -> master (non-fast-forward)
	error: failed to push some refs to 'git@github.com:yaotian/doc.git'
	To prevent you from losing history, non-fast-forward updates were rejected
	Merge the remote changes (e.g. 'git pull') before pushing again.  See the
	'Note about fast-forwards' section of 'git push --help' for details.

解决办法是

	git fetch origin；git merge origin/master

## 分支相关操作

### 如何创建分支
	$ git branch test
	$ git branch
	* master
  	test
### 进入某个分支
	$ git checkout test
	$ git branch
	  master
	* test
### merge分支
	$ git branch
	  master
	* test

	$ git checkout master
	$ git branch
	* master
	  test

	git merge test


## sublime中git插件的常用命令
	ctrl+shift+p
	git add Current file
	git quick commit
	git push current branch


