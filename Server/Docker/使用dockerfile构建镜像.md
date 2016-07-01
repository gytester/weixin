##使用Dockerfile构建镜像
@author:jy.zenist.song, crate:2016-6-28, LastEdited:2016-7-1

Dockerfile是一种Docker定义的自身解释性脚本，类似于C语言的Makefile，其内部是由一条一条指令组成，每条指令对应Linux下面的一条命令。

Docker程序读取Dockerfile，根据其内部指令进行自定义image的生成，其内部描述了一个image产生的详细步骤，每一个步骤将成为image的一层，最多不能超过128层，也就实际命令是不能超过128行的。

####1. Dockerfile指令
指令的一般格式为：INSTRUCTION（指令） arguments（参数），可用指令列表如下:
<table style="font-size:12">
	<tr align="center">
		<td>指令</td>
		<td>格式</td>
		<td>释义</td>
	</tr>
	<tr>
		<td>#</td>
		<td>#commnet</td>
		<td>注释，不计入指令数量</td>
	</tr>
	<tr>
		<td>FROM</td>
		<td>方式一：FORM &lt;image> <br/>方式二：FROM &lt;image>:&lt;tag></td>
		<td>必选且必须为一条指令，指定原始镜像</td>
	</tr>
	<tr>
		<td>MAINTAINER</td>
		<td>MAINTAINER &lt;name></td>
		<td>指定维护者信息</td>
	</tr>
	<tr>
		<td>RUN</td>
		<td>方式一：RUN &lt;command> <br/>方式二：RUN ["executable", "param1","param2"]</td>
		<td>在shell终端运行命令,例echo hello:<br/>方式一：RUN "echo hello"相当于/bin/sh -c "echo hello";<br/>方式二：RUN ["/bin/sh","-c","echo hello"]</td>
	</tr>
	<tr>
		<td>EXPOSE</td>
		<td>EXPOSE &lt;port> [&lt;port>...]></td>
		<td>指定Docker服务端容器暴露的端口后，容器启动后需要使用-p &lt;port>:&lt;port>进行指定，或使用-P进行随机分配</td>
	</tr>
	<tr>
		<td>ENV</td>
		<td>ENV &lt;key> &lt;value></td>
		<td>指定环境变量，可供后续RUN指令使用，并在容器运行中保持</td>
	</tr>
	<tr>
		<td>WORKDIR</td>
		<td>WORKDIR /path/to/workdir</td>
		<td>指定后入RUN指令的工作目录，相当于cd命令</td>
	</tr>
	<tr>
		<td>COPY</td>
		<td>COPY &lt;src> &lt;dest></td>
		<td>复制本地主机的 &lt;src> （为Dockerfile所在目录的相对路径）到容器中的 &lt;dest></td>
	</tr>
	<tr>
		<td>ENTERYPOINT</td>
		<td>方式一：ENTERYPOINT ["executable", "param1","param2"]<br/>方式二：ENTERYPOINT command param1 param2</td>
		<td>所配置的容器启动后所默认执行的命令，不会被docker run提供的参数覆盖，当含有多个时仅最后一个生效。</td>
	</tr>
</table>


####2. 通过Dockerfile创建镜像
在编写万Dockerfile之后，可以通过运行命令docker build来建立镜像。其命令的格式如下：
<!-- script:shell -->
	$docker build [选项] 路径

改命令实现了依据指定目录下的Dockerfile的指令建立镜像并发送个Docker服务器的功能。运行命令示例如下：
<!-- script:shell -->
	$cd /path/to/dockfile
	$docker build -t zenist/tomcat:0.1.0 .

####3. 通过Dockerfile创建tomcat镜像
(1) 建立一个空目录，并进入其中进行Dockerfile文件编写
<!-- script:shell -->
	#mkdir tomcat
	#cd tomcat
	#vi Dockerfile

(2) 编写Dockerfile内容如下：
<!-- script:shell -->

	# 1.Pull base image  
	FROM ubuntu
	MAINTAINER jy.zenist.song "407081518@qq.com"
	
	# 2.Install curl
	RUN apt-get update 
	RUN apt-get -y install curl
	
	# 3.Install JDK 7  
	WORKDIR /tmp
	RUN curl -L 'http://download.oracle.com/otn-pub/java/jdk/7u65-b17/jdk-7u65-linux-x64.tar.gz' -H 'Cookie: oraclelicense=accept-securebackup-cookie; gpw_e24=Dockerfile' | tar -xz
	RUN mkdir -p /usr/lib/jvm
	RUN mv /tmp/jdk1.7.0_65/ /usr/lib/jvm/java-7-oracle/
	
	# 4.Set Oracle JDK 7 as default Java  
	RUN update-alternatives --install /usr/bin/java java /usr/lib/jvm/java-7-oracle/bin/java 300
	RUN update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/java-7-oracle/bin/javac 300

	# 5.Install tomcat7  
	WORKDIR /tmp
	RUN curl -L 'http://archive.apache.org/dist/tomcat/tomcat-7/v7.0.8/bin/apache-tomcat-7.0.8.tar.gz' | tar -xz
	RUN mv /tmp/apache-tomcat-7.0.8/ /opt/tomcat7/
	
	# 6.Add tomcat7.sh from local and add it to system service 
	ADD tomcat7.sh /etc/init.d/tomcat7
	RUN chmod 755 /etc/init.d/tomcat7

	# 7.Set System env
	ENV JAVA_HOME /usr/lib/jvm/java-7-oracle/
	ENV CATALINA_HOME /opt/tomcat7
	ENV PATH $PATH:$CATALINA_HOME/bin
	
	# 8.Expose ports.  
	EXPOSE 8080

	# 9.Define default command.  
	ENTRYPOINT service tomcat7 start && tail -f /opt/tomcat7/logs/catalina.out

(3) 在Dockerfile通目录下编辑容器所需加载的tomcat7.sh内容如下：
<!-- script:shell -->
	export JAVA_HOME=/usr/lib/jvm/java-7-oracle/  
	export TOMCAT_HOME=/opt/tomcat7  
	  
	case $1 in  
	start)  
	  sh $TOMCAT_HOME/bin/startup.sh  
	;;  
	stop)  
	  sh $TOMCAT_HOME/bin/shutdown.sh  
	;;  
	restart)  
	  sh $TOMCAT_HOME/bin/shutdown.sh  
	  sh $TOMCAT_HOME/bin/startup.sh  
	;;  
	esac 
 
	exit 0  

(4) 执行Dockerfile脚本：
<!-- script:shell -->
	#docker build -t zenist/tomcat:last .

(5) 启用tomcat镜像，并查看服务输出日志
<!-- script:shell -->
	#docker run -d -p 8080:8080 --name tomcat zenist/tomcat
	#docker logs tomcat