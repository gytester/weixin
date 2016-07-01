##Docker镜像相关操作

@author:jy.zenist.song, crate:2016-6-28, LastEdited:2016-7-1

Docker运行容器需要在本次存在对应的镜像，如果镜像不存在，Docker会从镜像仓库进行下载（默认是Docker Hub公共Registry中的仓库）

#####（1）获取镜像
在Docker Hub仓库下载一个ubuntu12.04操作系统的镜像
<!-- Script:shell -->
	#docker pull ubuntu:12.04

#####（2）列出本地镜像
<!-- Script:shell -->
	#docker images
	REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
	zenist/sshd                 latest              5761dc81f721        4 weeks ago         242.3 MB
	jenkins                     latest              bfe368622d35        5 weeks ago         710.1 MB
	nginx                       1.9                 c8c29d842c09        5 weeks ago         182.7 MB
	mysql                       5.6                 2c0964ec182a        5 weeks ago         329 MB
	centos                      latest              96eecaf1019a        6 weeks ago         196.7 MB
	ubuntu                      latest              c5f1cf30c96b        8 weeks ago         120.7 MB

输出信息中，个字段的意义如下：<br/>
<table style="font-size:12">
	<tr>
		<td>字段</td>
		<td>释义</td>
	</tr>
	<tr>
		<td>REPOSITORY</td>
		<td>标明镜像来自于那个仓库</td>
	</tr>
	<tr>
		<td>TAG</td>
		<td>镜像的标记（发行版本）</td>
	</tr>
	<tr>
		<td>ID</td>
		<td>镜像ID（唯一）</td>
	</tr>
	<tr>
		<td>SIZE</td>
		<td>镜像大小</td>
	</tr>
</table>
<i>具有不同tag的镜像，可能具有相同的ID</i>

#####（3）自定义镜像
自定义镜像有两种途径：<br/>
　　其一，利用本地文件系统创建一个新的<br/>
　　其二，从Docker Hub获取一样有镜像并跟新<br/>

这里已第二种方式进行说明，如何自定义一个具有python环境的Docker ubutu镜像
 
* Step1，先使用下载的镜像启动容器
<!-- Script:shell -->
	#docker run -it ubuntu /bin/bash		#启动命令会于容器部分详细说明
	root@57daea17d0d1:/# 					#容器交互终端，其中@与:符号之间的为容器ID，CID运行中唯一
* Step2，在容器的交互终端中安装python
<!-- Script:shell -->
	root@57daea17d0d1:/# apt-get update					
	root@57daea17d0d1:/# apt-get install python
* Step3, 提交变更
<!-- Script:shell -->
	root@57daea17d0d1:/# exit			#退出容器，如一次不成功，可多次运行
	exit								#容器输出退出状态，容器退出成功
	#docker commit -m "Init Python env" -a "jy.zenist.song" 57daea17d0d1 zenist/ubuntu:with-python
docker commit参数说明如下：<br/>
<table style="font-size:12">
	<tr>
		<td>参数</td>
		<td>释义</td>
	</tr>
	<tr>
		<td>-m</td>
		<td>指定提交的说明信息，类似于版本控制工具</td>
	</tr>
	<tr>
		<td>-a</td>
		<td>指定更新的用户信息</td>
	</tr>
	<tr>
		<td>CONTAINER_ID</td>
		<td>指定用来创建镜像的容器ID。如忘记，可以通过docker ps -l查看</td>
	</tr>
	<tr>
		<td>REPO_DESC</td>
		<td>仓库路径/镜像名称：TAG(发布名称)</td>
	</tr>
</table>

* Step4, 查看已保存的自定义镜像
<!-- Script:shell -->
	#docker images
	REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
	zenist/ubuntu               with-python         55fb061c1543        10 seconds ago      436 MB

#####（4）删除本地镜像
如果要移出本地的镜像，可以使用docker rmi命令。
<!-- Script:shell -->
	#docker rmi zenist/ubuntu