#Docker体系搭建
@author:jy.zenist.song, crate:2016-6-28, LastEdited:2016-7-1

Docker体系一个可以分为三个大部分，Docker-engine、Docker-compose、Harbor(Docker-registry)

其三个部分结关系结构图如下：
![](https://raw.githubusercontent.com/zenist/doc/master/resource/Docker/Docker%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%9C%E5%9B%BE.png)

## 一、Docker-engine
Docker 是 PaaS 提供商 dotCloud 开源的一个基于 LXC 的高级容器引擎，源代码托管在 Github 上, 基于go语言并遵从Apache2.0协议开源。

* [Docker简介](https://github.com/gytester/weixin/blob/master/Server/Docker/CentOS%E5%AE%89%E8%A3%85%E4%B8%8E%E9%85%8D%E7%BD%AEDocker.md)

###2. Docker四要素
######（1）镜像Image
镜像就是一个只读的模板，一个封装一套完成目标环境的只读的静态文件。

<i>如，Jenkins的Docker镜像，就是封装了Jenkins应用与其依赖环境的一个静态文件。</i>

######（2）容器Container
容器是从静态镜像文件创建的动态的运行实例。其本身可以看作一个简易版的运行状态的Linux系统，其系统是支持读写操作的。容器支持启动、停止、开始、删除。通过同一个镜像创建的不同容器间是相互隔离的。

<i>如，通过Jenkins镜像创建的容器就是一个运行了Jenkins及其依赖服务的最精简版linux系统。</i>

######（3）仓库Repository
仓库是镜像文件的集中存放场所，其与镜像是一对多的关系，同时还管理与维护了每个镜像的不同版本（TAG）。

<i>如，Jenkins镜像就是维护在最大的公开仓库DockerHub中的</i>

######（4）仓库注册服务器Registry
仓库注册服务器提供了仓库的创建与管理功能，如权限管理，其与仓库是一对多的关系。通常企业出于不同的目的，主要是保密，都会与内网环境搭建自己Registry，这种Registry通常是私有的，并具有很快的上传与下载速度。

<i>另：私有的Registry可以解决因网络限制无法获取存于公网仓库的第三方镜像问题。</i>

######Docker四要素关系图示
![](https://raw.githubusercontent.com/zenist/doc/master/resource/Docker/Docker%E5%9B%9B%E8%A6%81%E7%B4%A0%E5%9F%BA%E6%9C%AC%E5%85%B3%E7%B3%BB%E8%A7%86%E5%9B%BE.png)

###3.Docker环境安装
Docker目前在Linux、Windows、Mac等多个平台都提供了安装支持。

* [CentOS安装与配置Docker](https://github.com/gytester/weixin/blob/master/Server/Docker/CentOS%E5%AE%89%E8%A3%85%E4%B8%8E%E9%85%8D%E7%BD%AEDocker.md)


###3.  Docker使用
Docker支持命令行的操作方式，其命令风格与git极为相近。

* [Docker镜像（Image）相关基础操作](https://github.com/gytester/weixin/blob/master/Server/Docker/Docker%E9%95%9C%E5%83%8F%E7%9B%B8%E5%85%B3%E6%93%8D%E4%BD%9C.md)

* [Docker容器（Container）相关基础操作](https://github.com/gytester/weixin/blob/master/Server/Docker/Docker%E5%AE%B9%E5%99%A8%E7%9B%B8%E5%85%B3%E6%93%8D%E4%BD%9C.md)

* Docker镜像（Image）高级操作:
	*  [使用Dockerfile构建镜像](https://github.com/gytester/weixin/blob/master/Server/Docker/%E4%BD%BF%E7%94%A8dockerfile%E6%9E%84%E5%BB%BA%E9%95%9C%E5%83%8F.md)

* Docker容器（Container）高级操作：
	* 导入、导出

##二、Docker私服

###1. Docker私服
Docker需使用镜像服务器来维护各种自定义镜像，而对企业内部的自定义服务器通常还有大量的私密信息，因此将内部自定义镜像放置在Docker公开Registry上就不合适了，那么就需要在企业内部搭建一套Docker镜像registry服务器。这套服务器成为Docker镜像企业内部私有服务器，简称Docker私服。

###2. Docker私服的好处
* 隐私信息保护。通过在企业内网搭建Docker私服后，可以使Docker镜像的维护、更新、与下载都在企业内部网络中进行，通过企业内部网络自身的访问保护，实现了镜像私有的目的。

* 摆脱网络依赖。通过配置Docker内部私服，可以使Docker通过内网获取所有DockerRegistry相关功能的使用。通过给Docker私服开通外网或是直接导入image文件就可以实现Docker镜像的获取。

###3. Docker私服的选择
Docker团队自身提供了一套registry服务，提供了基础的镜像服务功能，如docker push/pull命令。但对于企业内部细化后的研发团队与层级结构来说并不够用。这个时候VMware公司开源的Docker企业级Registry项目Harbor提供了解决方案。

###4. Harbor

* [Harbor简介与安装](https://github.com/gytester/weixin/blob/master/Server/Docker/Harbor%E7%AE%80%E4%BB%8B%E4%B8%8E%E5%AE%89%E8%A3%85.md)
* [Harbor使用](http://)

##三、Docker-compose


##四、Docker使用实例
* [自定义并后台运行一个含有sshd服务的容器](http://)
* [通过Docker安装Jenkins](http://)
* 基于Docker的测试环境搭建