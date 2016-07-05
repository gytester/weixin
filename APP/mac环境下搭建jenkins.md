#Mac环境下搭建Jenkins
Mac环境下搭建与运行Jenkins的方式有很多，比如：

* tomcat+jenkins
* java -jar jenkins.war
* 安装jenkins.pkg

每种安装方式各有优势，这里那jenkins.pkd的方式进行说明

####安装
（1）下载jenkins

* 访问jenkins官网：[https://jenkins.io/](https://jenkins.io/)
* 点击【Download Jenkins】，并选择【2.XX】版本中的【Mac OS X】进行下载

（2）安装jenkins

双击所下载的jenkins-2.xx.pkg后，一路下一步即可
####配置
 由于jenkins默认配置会对后续的测试任务产生较多的限制，如运行不了smart_monkey工具。
 所以需要对默认的用户、用户组、jenkins_home进行修改
 
 (1) 关闭jenkins服务

	$sudo launchctl unload /Library/LaunchDaemons/org.jenkins-ci.plist 
 (2) 修改jenkins默认配置
 	
 	#1. 首先在用户目录下简历jenkins的home、log、tmp文件夹
 	$mkdir ~/jenkins
 	$mkdir ~/jenkins/home
 	$mkdir ~/jenkins/log
 	$mkdir ~/jenkins/tmp
 
 	#2. 修改下面这两个plist文件，修改内容看下面截屏
 	$sudo vi /Library/LaunchDaemons/org.jenkins-ci.plist
 	$sudo vi /Library/Preferences/org.jenkins-ci.plist
 
 （3）修改后的LaunchDaemons/org.jenkins-ci.plist如下：
 
 （4）修改后的Preferences/org.jenkins-ci.plist如下：

####启动jenkins
 （1）启动jenkins
	
	$sudo launchctl load /Library/LaunchDaemons/org.jenkins-ci.plist 
 （2）访问jenkins
 	
 	http://localhost:8080
  (3) Jenkins 2版本初始化配置需要输入Administrator密码
  	
  	jenkins的启动浏览器页面有提示
  	在$JENKINS_HOME/secrets/initialAdminPassword
  	我本地的获取方式：
  	$cat /Users/jy/Jenkins/Home/secrets/initialAdminPassword
  (4)填入密码后，Jenkins可以进行初始化
  	
	
####补充
运行日志查看：

	$tail -f ~/Jenkins/log/*
卸载命令：
	
	$/Library/Application\ Support/Jenkins/Uninstall.command
 
 		



