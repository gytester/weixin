#Mac系统的测试环境搭建

@author：jy.zenist.song, create:2016-07-05

本文测试环境基于python语言，所安装的测试环境含有如下功能：

* Android部分
	* Android SDK基础环境与官方的各种调试工具
	* Android随机压力测试工具monkey
 	* Android UI自动化测试环境appium
* iOS部分
	* 基于instruments的各种官方的调试工具
	* app包命令行安装工具ideviceinstaller 
	* iOS 随机压力测试工具smart_monkey
	* iOS UI自动化测试环境appium

注：此文较长，建议收藏并通过PC版微信查看

###1. 依赖安装
#####（1）python运行环境
python自动化脚本开发与运行所需依赖的环境

安装与配置过程参见历史文档《Python与pip入门》。

可通过发送消息python环境或160701获取。

#####（2）JAVA环境安装
Android与java程序开发与测试的基本依赖环境
  
访问java官方网址,下载最新版mac系统pkg安装包
[http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
![image](https://raw.githubusercontent.com/zenist/doc/master/resource/app/Mac%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA-1.png)

下载并进行下一步式安装后，配置JAVA_HOME环境变量
<!-- script:shell -->
	#1. 编辑系统环境变量配置文件/etc/profile
	$ sudo vi /etc/profile
	#2. 在文件的末尾添加如下内容
	export JAVA_HOME=`/usr/libexe/java_home`
	#3. 编辑完成后，使用ESC进入命令模式':'，输入wq！命令回车保存并退出
	#4. 在当前开启的终端内重新载入系统环境变量
	$ source /etc/profile
	#5. 验证java配置，获取版本
	$ java -version
	java version "1.8.0_91"
至此，JAVA环境安装完毕

#####（3）Xcode及Xcode命令行工具安装

* Xcode安装

iOS测试依赖，目前iOS 9.3版本需要Xcode 7.3.0以上版本

建议直接从官网下载：
[https://developer.apple.com/download/](https://developer.apple.com/download/)

下载前需登录appleID，可免费注册，后续测试也需要使用

xCode工具下载后双击即可安装。

* Xcode命令行工具安装与配置

Xcode安装完成后，通过终端命令安装
<!-- script:shell -->
	#1. 输入命令
	$ xcode-select --install
	#2. 在弹出的对话框上选择【Install】
安装完成后，还需要进行配置，不然后续Appium运行会报错
<!-- script:shell -->
	#3. 输出当前路径配置
	$xcode-select -p 
	/Applications/Xcode.app/Contents/Developer
	#4.1 前面的/Applications/Xcode.app必须是xcode的安装路径
	#4.2 如不是，运行如下命令更改，${XCode_path}代表你本机的xdoe路径
	$xcode-select -s ${Xcode_path}/Contents/Developer

#####（4）Homebrew安装
Mac OSX上的软件包管理工具，能在Mac中方便的安装软件或者卸载软件，只需要一个命令， 非常方便，类似ubuntu系统下的apt-get的功能。

官方网址：[http://brew.sh/](http://brew.sh/)

可以通过在终端直接运行如下命令安装brew工具
<!-- script:shell -->
	#1. 该运行命令可以在官网首页直接复制获取
	$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
	#2. 通过brew安装wget进行安装验证
	$ brew install wget

#####（5）RubyGems安装与配置
RubyGems简称gems，是一个用于对 Ruby组件进行打包的 Ruby 打包系统，功能上类似于python的pip。

* 安装方法如下：

<!-- script:shell -->
	#1. 通过brew命令安装
	$ brew install ruby
	#2. 验证gem安装
	$ ruby -v
	ruby 2.3.0p0 (2015-12-25 revision 53290) [x86_64-darwin15]
	$ gem -v
	2.5.1

* 国内阿里源配置方法如下

为什么要配置成国内的源，你慢慢就会懂得...
<!-- script:shell -->
	$ gem sources --add https://ruby.taobao.org/ --remove https://rubygems.org/
	$ gem sources -l
	*** CURRENT SOURCES ***

	https://ruby.taobao.org
	# 请确保只有 ruby.taobao.org
从此体验飞一般的感觉.

#####（6）NPM安装
NPM（node package manager），是一个node的包管理器，功能上类似于python的pip。

* 安装方法如下：

<!-- script:shell -->
	#1. 通过brew命令安装
	$ brew install node
	$2. 验证node安装
	$ node -v
	v6.0.0
	$ npm -v
	3.8.6

* cnmp安装

cnmp是啥，简单理解为用了国内源的npm，为啥用cnpm?你猜？
<!-- script:shell -->
	#通过npm命令指定源的方式安装
	$npm install -g cnpm --registry=https://registry.npm.taobao.org
	

###2. 测试工具安装
#####（1）iOS app发布包命令行安装工具
可以直接通过通过如下brew命令安装
<!-- script:shell -->
	#1. 开启终端命令行，输入如下命令
	$ brew install ideviceinstaller
	#2. 安装验证, 输入工具使用帮助信息
	$ ideviceinstaller

#####（2）iOS smart_monkey工具安装
工具github地址如下：

[https://github.com/vigossjjj/CrashMonkey4IOS](https://github.com/vigossjjj/CrashMonkey4IOS)

<!-- script:shell -->
	#1. 安装依赖，可跳过已安装过的依赖
	$ brew install -HEAD ideviceinstaller
	$ brew install libimobiledevice
	$ brew install imagemagick
	#2. 安装smart_monkey
	$ gem install smart_monkey
	#3. 安装验证,输出帮助信息
	$ smart_monkey
	
##### (3) Android SDK安装与配置
由于网络原因，不推荐去官网下载，要是翻墙技术高，你随便。

推荐国内地址：

[http://www.android-studio.org/](http://www.android-studio.org/)

* 安装
访问android-studio网站，选择Mac版SDK下载。如下图

![image](https://raw.githubusercontent.com/zenist/doc/master/resource/app/Mac%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA-2.png)

下载后解压至某一目录下，我的是~/ANDROID_SDK

* 配置环境变量
需要配置ANDROID_HOME变量，并将SDK下的tools与platform-tools加入PATH变量下

<!-- script:shell -->
	#1. 编辑系统环境变量配置文件/etc/profile
	$ sudo vi /etc/profile
	#2. 在文件的末尾添加如下内容,启用“/Users/jy/ANDROID_SDK”为我本地SDK存放路径
	export ANDROID_HOME="/Users/jy/ANDROID_SDK/"
	export PATH=.:$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools
	#3. 编辑完成后，使用ESC进入命令模式':'，输入wq！命令回车保存并退出
	#4. 在当前开启的终端内重新载入系统环境变量
	$ source /etc/profile
	#5. 验证Android配置，运行adb命令
	$ adb

* Android SDK的源配置与版本安装

同样因为墙的存在，我们需要配置国内的源。

 1）启动android SDK管理器
 
<!-- script:shell -->
	#1. 在终端命令行运行命令直接启动
 	$ android
 
 	
 2）进入配置页面

![image](https://raw.githubusercontent.com/zenist/doc/master/resource/app/Mac%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA-3.png)

 3）进入配置国内代理

![image](https://raw.githubusercontent.com/zenist/doc/master/resource/app/Mac%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA-4.png)

 4）重新加载

![image](https://raw.githubusercontent.com/zenist/doc/master/resource/app/Mac%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA-5.png)

 5）现在就可以进行勾选所需的Android SDK进行安装了

##### (4) Appium安装
本文介绍界面式appium安装。

官网被墙了，建议通过百度云盘下载：

[http://pan.baidu.com/s/1jGvAISu](http://pan.baidu.com/s/1jGvAISu)

下载当前最新版appium-1.5.3.dmg后，下一步安装即可

安装成功后，可以直接通过spotlight（comm+空格）中输入appium启动

![image](https://raw.githubusercontent.com/zenist/doc/master/resource/app/Mac%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA-6.png)

启动成功后，点击Docker按钮进行安装检测

![image](https://raw.githubusercontent.com/zenist/doc/master/resource/app/Mac%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA-7.png)

##### 至此，Mac平台APP自动化测试环境部署完成
