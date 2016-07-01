## 自定义并后台运行一个含有sshd服务的容器
@author:jy.zenist.song, crate:2016-6-2, LastEdited:2016-7-1

###需求
* 下载一个CentOS image
* 在CentOS系统中安装sshd服务并另存为一个新的image
* 通过Docker运行CentOS容器并启动sshd服务

###实现
####1. 下载一个CentOS镜像
#####(1)通过Docker search命令搜索一个CentOS镜像
<!-- script:shell -->
	# docker search centos
	INDEX       NAME                                    DESCRIPTION                                     STARS
	docker.io   docker.io/centos                        The official build of CentOS.                   2301 
	docker.io   docker.io/jdeathe/centos-ssh            CentOS-6 6.7 x86_64 / CentOS-7 7.2.1511 x8...   25     
	docker.io   docker.io/million12/centos-supervisor   Base CentOS-7 with supervisord launcher, h...   11      
	docker.io   docker.io/nimmis/java-centos            This is docker images of CentOS 7 with dif...   11  
	docker.io   docker.io/consol/centos-xfce-vnc        Centos container with "headless" VNC sessi...   9 
	docker.io   docker.io/torusware/speedus-centos      Always updated official CentOS docker imag...   8  

从上述搜索结果中，我们选择DESCRIPTION为“The official build”的镜像进行下载。
#####(2)通过Docker pull命令下载image至本地
<!-- script:shell -->
	# docker pull docker.io/centos
	1544084fad81: Pull complete
	ebe253abc97d: Downloading [===========>                   ] 15.68 MB/70.58 MB
	f0e1cf3be051: Download complete
	e934940aaa90: Download complete

我们可以从下载进度中看到镜像大小为70.58MB，接下来等待所有的下载状态都变成“Pull complete”。
<!-- script:shell -->
	1544084fad81: Pull complete
	ebe253abc97d: Pull complete
	f0e1cf3be051: Pull complete
	e934940aaa90: Pull complete
	Digest: sha256:f1f827eca5be4b91ebf0916fa69d1f41234b84752a04360f034cad25c9e1138d
	Status: Downloaded newer image for docker.io/centos:latest


我们可以看到image下载到本地之后被存储的名称为“docker.io/centos”
####2. 启动原始CentOS镜像并安装与配置sshd服务
#####（1）首先我们需要以交互的方式启动CentOS image
<!-- script:shell -->
	$ docker run -i -t docker.io/centos /bin/sh
	sh-4.2#

现在我们已经进入的docker启动的CentOS容器的交互状态了
#####（2）通过交互命令行下对CentOS容器进行sshd服务的安装与配置
整个open-ssh服务的安装与配置可以分为三步进行：
第一步，通过yum命令进行openssh安装
<!-- script:shell -->
	sh-4.2# yum install openssh-server
	...
	Resolving Dependencies
	Resolving Dependencies
	--> Running transaction check
	---> Package openssh-server.x86_64 0:6.6.1p1-25.el7_2 will be installed
	...
	Installed:
	  openssh-server.x86_64 0:6.6.1p1-25.el7_2
	.
	Dependency Installed:
	  tcp_wrappers-libs.x86_64 0:7.6-77.el7


第二步，修改/etc/ssh/sshd_config文件，对本地ssh服务进行配置
<!-- script:shell -->
	sh-4.2# vi /etc/ssh/sshd_config
	#对开启的sshd_config配置文件进行如下修改
	#1. 修改ssh连接端口号（可选）
	Port 22 -> Port 9527
	#2. 关闭PAM认证
	UsePAM yes -> UsePAM no

第三步，设置root密码用户ssh登录认证
<!-- script:shell -->
	sh-4.2# passwd

截止sshd服务已经安装配置完毕！
#####（3）对image变更进行保存
我们为了保证origin centos镜像，我们对这次所操作的进行进行一个“镜像另存为”操作

首先我们通过docker ps命令查询当前容器的运行ID
<!-- script:shell -->
	# docker ps
	CONTAINER ID        IMAGE               COMMAND               CREATED             STATUS              
	1233a64272ff        docker.io/centos    "/bin/sh"             29 minutes ago      Up 29 minutes 

在获取容器ID后，我们通过docker commit命令对当前运行的容器进行“镜像另存为”操作
<!-- script:shell -->
	#Usage:	docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
	# -a 添加作者信息
	# -m 添加说明信息
	$ docker commit -a "jy.zenist.song" -m "save container to sshd image" 1233a64272ff zenist/centos-sshd

现在，我们可以通过docker images命令对我们刚刚所创建的centos-sshd image进行查看
<!-- script:shell -->
	$ docker images 
	REPOSITORY           TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
	zenist/centos-sshd   latest              07cf2710ffc4        2 minutes ago       281.1 MB

####3. 启动自定义的centos-sshd image并与后台运行
通过 docker run -d 命令启动iamge
<!-- script:shell -->
	#Usage:	docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
	#    -d, --detach=false              Run container in background and print container ID
	#    -p, --publish=[]                Publish a container's port(s) to the host
	$ docker run -d -p 9527:9527 zenist/centos-sshd /usr/sbin/sshd -D