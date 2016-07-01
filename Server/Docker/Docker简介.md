# Docker简介
@author:jy.zenist.song, crate:2016-5-25, LastEdited:2016-5-25

![image](https://raw.githubusercontent.com/zenist/doc/master/resource/Docker/docker_cover.jpg)

### 1. 什么是Docker
<p><b>Docker 是一个开源的应用容器引擎。</b><br/>
让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的 Linux 机器上（目前Docker已经退出基于windows与Mac的Beta版本）。即将服务于服务所需的依赖打包，便于维护、复制与移植。</p>

<p><b>Docker 是一个开源的应用容器引擎。</b><br/>
让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的 Linux 机器上（目前Docker已经退出基于windows与Mac的Beta版本）。即将服务于服务所需的依赖打包，便于维护、复制与移植。</p>

<p><font color="red"><b>最后，如果上面的实在不原理理解，你可以理解Docker为一个用了新颖方式实现的超轻量级虚拟机</b></font><br/>

### 2. 为啥需要容器？
要知道为啥需要容器，那么就应该先知道应用容器长什么样子。一个做好的应用容器长得就好像一个装好了一组特定应用的虚拟机一样。比如我现在想用MySQL那我就找个装好MySQL的容器，运行起来，那么我就可以使用 MySQL了。

为什么不直接装个 MySQL不就好了，何必还需要这个容器这么诡异的概念？话是这么说，可是你要真装MySQL的话可能要再装一堆依赖库，根据你的操作系统平台和版本进行设置，有时候还要从源代码编译报出一堆莫名其妙的错误，可不是这么好装。而且万一你机器挂了，所有的东西都要重新来，可能还要把配置在重新弄一遍。但是有了容器，你就相当于有了一个可以运行起来的虚拟机，只要你能运行容器，MySQL的配置就全省了。而且一旦你想换台机器，直接把这个容器端起来，再放到另一个机器就好了。硬件，操作系统，运行环境什么的都不需要考虑了。

### 3. Docker能带来什么好处
一个很大的好处就是可以保证线下的开发环境、测试环境和线上的生产环境一致。利用容器可以实现开发的时候直接在容器里开发，提测的时候把整个容器当前的镜像提交给测试，测试好了后开发在当前镜像的基础上进行Bug修复，直到测试通过后，在将经过测试的容器直接上线就好了。这样，通过容器，整个开发、测试和生产环境可以保持高度的一致。

### 4. 为啥不直接用VM
那么既然容器和 VM 这么类似为啥不直接用 VM 还要整出个容器这么个概念来呢？Docker 容器相对于 VM 有以下几个优点：

* 启动速度快，容器通常在一秒内可以启动，而VM通常要更久
* 资源利用率高，一台普通 PC 可以跑上千个容器，你跑上千个 VM 试试
* 性能开销小， VM 通常需要额外的 CPU 和内存来完成 OS 的功能，这一部分占据了额外的资源

为啥相似的功能在性能上会有如此巨大的差距呢，其实这和他们的设计的理念是相关的。 VM 的设计图如下：<br/>
![image](https://raw.githubusercontent.com/zenist/doc/master/resource/Docker/vm_struct.jpg)

VM 的 Hypervisor 需要实现对硬件的虚拟化，并且还要搭载自己的操作系统，自然在启动速度和资源利用率以及性能上有比较大的开销。而 Docker 的设计图是这样的： <br/>
![image](https://raw.githubusercontent.com/zenist/doc/master/resource/Docker/docker_struct.jpg)

Docker 几乎就没有什么虚拟化的东西，并且直接复用了 Host 主机的 OS，在 Docker Engine 层面实现了调度和隔离重量一下子就降低了好几个档次。

### 5. Docker相关网络资料整理
[1. Docker官方网站](http://www.docker.com/)

[2. Docker中文社区](http://www.docker.org.cn/)

[3. 史上最全的Docker资料集萃](http://special.csdncms.csdn.net/BeDocker/)

