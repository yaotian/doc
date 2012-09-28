# Python相关
关于python的书市面上一大堆，本文就不做系统介绍了。主要记录一些tips

## Virtualenv

### 安装在Ubuntu

	sudo apt-get install python-virtualenv

### 常用命令

使用之前先建立个虚拟环境，进入虚拟环境后安装软件

	virtualenv --no-site-packages {folder}  
	source {folder}/bin/activate
	pip install Django==1.3  




