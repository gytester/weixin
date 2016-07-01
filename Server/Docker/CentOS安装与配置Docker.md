##Docker环境安装-基于CentOS

#### 安装依赖

* 64-bit CentOS 7+
* linux内核版本≥3.10
* sudo或root权限
#### Docker-enging 安装
（1）更新yum并添加docker源
<!-- script:shell -->
	##1. 更新yum
	#yum update
	##2. 增加yum源
	#sudo tee /etc/yum.repos.d/docker.repo <<-'EOF'
	[dockerrepo]
	name=Docker Repository
	baseurl=https://yum.dockerproject.org/repo/main/centos/7/
	enabled=1
	gpgcheck=1
	gpgkey=https://yum.dockerproject.org/gpg
	EOF
（2）安装Docker engine
<!-- script:shell -->
	#yum install docker-engine
（3）配置Docker为系统service
<!-- script:shell -->
	#cp -n /lib/systemd/system/docker.service /etc/systemd/system/docker.service
（4）配置Docker加速器
<!-- script:shell -->
	#sed -i "s|ExecStart=/usr/bin/docker daemon|ExecStart=/usr/bin/docker daemon --registry-mirror=https://r40ut9d0.mirror.aliyuncs.com |g" /etc/systemd/system/docker.service
（5）修改Docker默认工作目录（默认：/var/lib/docker目录）
<!-- script:shell -->
	#vim /etc/systemd/system/docker.service
	## 在'ExecStart=...'行的daemon后添加所需配置的docker工作目录，该行内容如下
	ExecStart=/usr/bin/docker daemon --graph=/data1/docker --registry-mirror=https://r40ut9d0.mirror.aliyuncs.com  -H fd://
	
（6）生效配置，重启docker服务
<!-- script:shell -->
	#systemctl daemon-reload
	#service docker restart