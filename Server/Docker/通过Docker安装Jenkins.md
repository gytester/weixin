## 通过Docker安装Jenkins
@author jy.zenist.song create:2016-6-7, lasted Edit:2016-7-1

### 前言
Docker是一个伟大的发明，通过它可以大大的提升我们的工作效率，同时使我们变得更加专注。

下面将从两种Jenkins环境搭建途径，传统的服务部署与基于Docker容器的安装，来进行说明：

<hr/>

### 1. 通过传统方式的Jenkins安装
为了降低Jenkins传统安装部分的篇幅，该部分的安装全部采用非源码包的方式进行。

###1.1. Jenkins安装准备
如果要安装Jenkins，则至少需要准备如下依赖环境：

* 安装并配置JAVA运行环境
* Jenkins应用发布包


####1.2 JAVA运行环境安装
#####(1) 第一步，通过yum命令参看以下容器系统中是否安装有java
<!-- script:shell -->
	$ yum list installed | grep java
	Error: No matching Packages to list	#这段信息显示我这个容器里没有安装任何java

#####（2）第二步，通过yum命令查看yum源的资源库中是否存在我们所需的jdk版本
<!-- script:shell -->
	$ yum -y list java*
	...
	Available Packages
	...
	java-1.6.0-openjdk.x86_64
	...
	java-1.7.0-openjdk-demo.x86_64
	...
	java-1.8.0-openjdk-src-debug.x86_64
	...

#####（3）第三步，通过yum命令安装指定的、可用的java版本环境
这次根据jenkins运行的需求，我们需要安装JDK1.7版本的java运行环境。使用命令如下：
<!-- script:shell -->
	$ yum -y install java-1.7.0-openjdk*

安装完成后，可以通过运行java版本命令进行查看安装结果
<!-- script:shell -->
	$ java -version 	#java运行工具版本查看
	$ javac -version	#java编译工具版本查看

也可以通过yum list命令进行查看
<!-- script:shell -->
	$ yum -y list installed | grep java


#####（4）第四步，配置JAVA_HOME环境变量
通常，第三放的依赖JAVA运行环境的服务都需要读取本地的JAVA_HOME环境变量，设置JAVA_HOME就定义一个系统变量并将其指向一个含有java可执行程序的目录。那要配置环境变量，就首先要获取java安装的位置。

本次安装为使用的yum，yum默认会将JAVA的OpenSDK安装到/usr/lib/jvm目录下, 可以通过ls目录查看一下该目录下的具体文件情况
<!-- script:shell -->
	$ ls -l /usr/lib/jvm
	lrwxrwxrwx.  1 root root  26 Jun  6 14:29 java -> /etc/alternatives/java_sdk
	lrwxrwxrwx.  1 root root  32 Jun  6 14:29 java-1.7.0 -> /etc/alternatives/java_sdk_1.7.0
	lrwxrwxrwx.  1 root root  40 Jun  6 14:29 java-1.7.0-openjdk -> /etc/alternatives/java_sdk_1.7.0_openjdk
	drwxr-xr-x. 10 root root 116 Jun  6 14:29 java-1.7.0-openjdk-1.7.0.101-2.6.6.1.el7_2.x86_64
	lrwxrwxrwx.  1 root root  34 Jun  6 14:29 java-openjdk -> /etc/alternatives/java_sdk_openjdk
	lrwxrwxrwx.  1 root root  21 Jun  6 14:22 jre -> /etc/alternatives/jre
	lrwxrwxrwx.  1 root root  27 Jun  6 14:22 jre-1.7.0 -> /etc/alternatives/jre_1.7.0
	lrwxrwxrwx.  1 root root  35 Jun  6 14:22 jre-1.7.0-openjdk -> /etc/alternatives/jre_1.7.0_openjdk
	lrwxrwxrwx.  1 root root  53 Jun  6 14:22 jre-1.7.0-openjdk-1.7.0.101-2.6.6.1.el7_2.x86_64 -> java-1.7.0-openjdk-1.7.0.101-2.6.6.1.el7_2.x86_64/jre
	lrwxrwxrwx.  1 root root  29 Jun  6 14:22 jre-openjdk -> /etc/alternatives/jre_openjdk

通过命令输出结果可以看到，这个有一个文件夹，进去后会发现bin目录，并在bin目录下发现一系列的可运行的java命令。

再回过来看命令输出结果，里面若有软连接连接至这个目录，这么设计的目的就是为了后续升级维护起来方便。我们可以将这个软连作为JAVA_HOME的值进行配置。

这里使用全局的永久的修改方法，即修改系统的profile，位于/etc/目录下
<!-- script:shell -->
	$ vi /etc/profile
	#在开启的文件最末添加一行内容
	export JAVA_HOME="/usr/lib/jvm/jre-1.7.0-openjdk-1.7.0.101-2.6.6.1.el7_2.x86_64"

保存修改后的文件后，为确保其生效，需要进行重新加载
<!-- script:shell -->
	source /etc/profile

我们可以通过echo命令进行验证
<!-- script:shell -->
	$ echo $JAVA_HOME
	/usr/lib/jvm/jre-1.7.0-openjdk-1.7.0.101-2.6.6.1.el7_2.x86_64

可以看到变量信息输出正确，表明环境变量配置成功了。

####1.3 获取并安装Jenkins安装包
通过访问Jenkins官网，获取最新的发布版本

官网地址：[https://jenkins.io/](http://https://jenkins.io/)

本次安装的是当前的最新版：Jenkins-1.651.2-1.1

Jenkins程序安装文件可以通过命令行中的wget命令进行下载
<!-- script:shell -->
	$ wget http://pkg.jenkins-ci.org/redhat-stable/jenkins-1.651.2-1.1.noarch.rpm 

####1.4 下载成功后，通过rpm命令进行安装
<!-- script:shell -->
	$rpm -ivh jenkins-1.651.2-1.1.noarch.rpm

####1.9 接下来等待我们的是什么？
也许在进行上述步骤的时候已经出现问题。

也许根本就执行不下去，因为有些命令都运行不了。

也许你上述步骤都执行的很成功，但：

* 为什么我的Jenkins启动不了
* 我的环境变量配置是不是错了
* 我下载的jenkins版本是不是和我系统中的某些依赖不兼容
* ... ...

<hr/>

### 2. 通过Docker的方式安装Jenkins
如果你问谁最了解Jenkins？那一定是Jenkins的研发团队，如果他们都部署不成功，那还有谁？

#### 通过docker安装Jenkins
为什么没有子标题，因为根本不需要

通过运行docker命令进行Jenkins安装与启动
<!-- script:shell -->
	# 通过docker pull命令获取jenkins官方镜像
	$ docker pull jenkinsci/jenkins
	# 通过docker run命令运行jenkins
	$ docker run -p 8080:8080 -p 50000:50000 docker.io/jenkinsci/jenkins

现在，jenkins已经安装部署完成了。






