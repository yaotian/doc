# Git基础命令
## 创建版本库
以下命令可以创建版本库    

	mkdir dir
	cd dir
	git init

	git config --global user.name "My Name"
	git config --global user.email "my@email.com"

在目录下会创建.git. 可以修改.git下的config,例如name和email

## 初始化提交
	touch README
	git add . # 将当前目录添加到 git 仓库中， 使用 git add -A 则是添加所有改动的文档
	git commit -m "Initial commit"
	git remote add origin git@github.com:yaotian/doc.git # 设置仓库