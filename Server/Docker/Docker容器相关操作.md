##Docker容器相关操作

Docker容器其生命周期供4个阶段，新建，运行，停止，删除。

####1. 新建并启动容器
使用命令docker run及对应不同参数实现不同的启动方式，Docker的标准操作为：

* 检查本地是否存在指定的镜像， 不存在就从共有仓库下载
* 利用镜像创建并启动一个容器
* 分配一个文件系统，并在只读的镜像层外面挂在一个可读写层
* 从宿主机配置的网桥中桥接一个虚拟的接口道容器中去
* 从地址池配置一个ip地址给容器
* 执行用户指定的应用程序
* 执行完毕后容器被中止

#####（1）新建并启动一次性的运行命令容器

下面的命令实现了调用基于ubuntu镜像启动的容器内的/bin/echo命令输出“hello world”，之后终止容器
<!-- script:shell -->
	#docker run --name test --rm ubuntu /bin/echo "Hello world"

这与本地运行echo命令几乎感觉不出任何区别，但实际上已经新建并启动了一个容器，并与容器中运行了echo "Hello world"命令，输出结果后，停止并删除容器。
其参数含义如下：<br/>
<table style="font-size:12">
	<tr>
		<td>参数</td>
		<td>释义</td>
	</tr>
	<tr>
		<td>--name</td>
		<td>指定容器对象的名称</td>
	</tr>
	<tr>
		<td>--rm</td>
		<td>容器终止后自动删除，如不指定则不删除，可通过docker ps -a查看</td>
	</tr>
</table>
#####（2）新建并启动可交互性容器

下面的命令实现了以交互的模式启动基于ubuntu镜像的容器。
<!-- script:shell -->
	#docker run -it ubuntu /bin/bash
	root@39252551e588:/# 					#Docker容器的交互终端
其参数含义如下：<br/>
<table style="font-size:12">
	<tr>
		<td>参数</td>
		<td>释义</td>
	</tr>
	<tr>
		<td>-i</td>
		<td>指定容器的标准输入保持打开状态</td>
	</tr>
	<tr>
		<td>-t</td>
		<td>指定Docker分配一个伪终端并绑定到容器的标准输入上</td>
	</tr>
	<tr>
		<td>交互终端类型</td>
		<td>如，/bin/sh, /bin/bash, /usr/bin/python等</td>
	</tr>
</table>

#####（3）新建并启动守护状态容器
通过结合使用-d参数，就可以是容器在后台已守护态（Daemonized）形式运行。命令如下:
<!-- script:shell -->
	# docker run -d ubuntu /bin/sh -c "while true;do echo hello world;sleep 1;done"
	e15aa7c645a41a1bf9c78d9c3c089f6657654f13b97d7b559f14062a6779272

参看上述后台运行的容器日志，可以通过如下命令
<!-- script:shell -->
	# docker logs e15aa7c645a4

进入后台守护状态的容器
<!-- script:shell -->
	# docker attach e15aa7c645a4

非停止状态退出当前进入的后台运行容器

使用快捷键Ctrl+p,Ctrl+q

####（4）映射镜像端口至host主机
可结合-p参数将容器中的服务端口映射至host主机，通过主机IP：PORT的方式提供访问服务。

如，映射resin容器的8080端口和5000两个端口至host
<!-- script:shell -->
	# docker run -p 8080:8080 -p 5000:5000 -d ubuntu /bin/bash 
	e15aa7c645a41a1bf9c78d9c3c089f6657654f13b97d7b559f14062a6779272
参数说明：-p [ip:][hostport]:containerPort[/udp]

####（5）挂载host目录至容器
可以结合-v参数挂在host本地目录至容器，供容器使用（读写|只读）目录中的内容

如，映射host的/data/build，/data/src目录至容器的/local/build,/local/src，其中build目录为只读
<!-- script:shell -->
	# docker run -v /data/build:/local/build:ro -v /data/scr:/local/scr -d ubuntu /bin/bash 
	e15aa7c645a41a1bf9c78d9c3c089f6657654f13b97d7b559f14062a6779272
参数说明：-p [ip:][hostport]:containerPort[/udp]

<i>注：用此方法可以实现容器与host文件共享，方便容器使用过程中进行与host间的数据拷贝</i>

####2. 终止容器
使用命令docker start命令实现

* 与交互状态下终止
通过输入exit，或使用Ctrl+d快捷键
<!-- script:shell -->
	root@39252551e588:/#exit
	exit

* 根据容器NAME
<!-- script:shell -->
	#docker stop big_bartik

* 根据容器ID
<!-- script:shell -->
	#docker stop f467ef93fe16

命令所需的容器NAME或ID值，可以通过docker ps命令进行查询
<!-- script:shell -->
	#docker ps
	CONTAINER ID        IMAGE                    COMMAND                  CREATED             STATUS              PORTS                              NAMES
	44e246874440        centos                   "/bin/bash"              5 minutes ago       Up 5 minutes                                           furious_bell
	

####3. 启动已中止的容器
使用命令docker start命令实现

* 根据容器NAME
<!-- script:shell -->
	#docker start big_bartik

* 根据容器ID
<!-- script:shell -->
	#docker start f467ef93fe16


####4. 删除容器
要删除容器，首先需要关闭容器，当容器关闭后使用docker rm命令进行删除
<!-- script:shell -->
	#docker rm f467ef93fe16

可以通过docker ps加-a参数的方式查询已经终值但未删除的容器
<!-- script:shell -->
	#docker ps -a
	CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
	92381d245c65        centos              "/bin/bash"         3 minutes ago       Up 3 minutes                            insane_carson
	399c613a0e15        ubuntu              "/bin/bash"         20 minutes ago      Up 20 minutes                           sharp_lamarr
