##Harbor安装

###1. Harbor简介

Harbor，由VMware中国研发的团队负责开发。Harbor项目是帮助用户迅速搭建一个企业级的registry 服务。它以Docker公司开源的registry为基础，提供了管理UI, 基于角色的访问控制(Role Based Access Control)，AD/LDAP集成、以及审计日志(Audit logging) 等企业用户需求的功能，同时还原生支持中文

Harbor项目GitHub地址：[https://github.com/vmware/harbor](https://github.com/vmware/harbor "https://github.com/vmware/harbor")

###2 Harbor架构简介

Harbor在架构上主要由五个组件构成：

![Harbor系统架构图](https://raw.githubusercontent.com/zenist/doc/master/resource/Docker/Harbor%E7%B3%BB%E7%BB%9F%E6%9E%B6%E6%9E%84%E5%9B%BE.png)

* Proxy: 前置的反向代理,统一接收浏览器、Docker客户端的请求，并将请求转发给后端不同的服务(registry, UI, token等)。

* Registry：储存Docker镜像，并处理docker push/pull 命令。

* Core services： 这是Harbor的核心功能，提供UI、webhook、token服务。

* Database：为core services提供数据库服务，负责储存用户权限、审计日志、Docker image分组信息等数据。

* Log collector：为了帮助监控Harbor运行，负责收集其他组件的log，供日后进行分析。

###3 Harbor安装
#####（1）安装依赖
* Python 2.7+
* Docker engine 1.10+
* Docker compose 1.6.0+

####（2）基于预编译包安装harbor
#####a. 下载预编译Harbor字节文件
通过访问vmware/harbor/release的git项目地址可以获取最新的harbor发布包，地址如下：

[https://github.com/vmware/harbor/releases](https://github.com/vmware/harbor/releases "Harbor预编译包发布地址")

Linux服务器可以通过wget进行下载：
<!-- lang:shell-->
	##1. 下载预编译包
	#wget https://github.com/vmware/harbor/releases/download/0.1.1/harbor_0.1.1.tgz
	##2. 加压缩至当前目录
	#tar zxf harbor_0.1.1.tgz
	##3. 进入到Harbor目录
	#cd harbor
#####b.  Harbor配置
Harbor部署之前需要进行简单的配置。配置过程如下：

* 1.编辑harbor.cfg配置文件
<!-- lang:shell-->
	##（承接2.3.2.1中的操作）
	#vi harbor.cfg

* 2. 配置harbor服务的访问地址、邮件服务器、认证服务器（可选）
<!-- lang:shell-->
	##1. 配置域名 = 服务器域名或服务器IP地址
	hostname = reg.yourdomain.com
	##2. 配置邮件服务器
	email_server = smtp.mydomain.com
	email_server_port = 25
	email_username = sample_admin@mydomain.com
	email_password = abc
	email_from = admin sample_admin@mydomain.com
	email_ssl = false
#####c.  Harbor启动与访问
* 1. 部署并启动harbor
<!-- lang:shell--> 
	##1. 部署harbor，位于Deploy目录下
	#./prepare	

* 2. 启动harbor
<!-- lang:shell--> 
 	##2. 启动harbor
	#docker-compose up -d

* 3. 访问harbor
打开浏览器，输入配置的域名或IP地址即可，入：http://reg.yourdomain.com