# Oozie使用

## 下载及编译
	wget http://mirrors.gigenet.com/apache/incubator/oozie/stable/oozie-3.2.0-incubating.tar.gz

悲催的是java写的东东居然还需要下载下来编译， 所以需要有maven3的环境

配置maven3很简单，下载zip包后解压到某目录下，然后配置两个环境变量

	export M3_HOME={you maven folder}
	export Path=$Path:$M3_HOME/bin

到{Oozie home}/bin目录下运行

	mkdistro.sh

如果失败的话，从失败的地方重新运行可以用命令：      

	mkdistro.sh -ff



## 安装

## 配置
### Hadoop需要做一些修改







