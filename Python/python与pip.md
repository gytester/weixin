#python与pip包

对我而言，武术的非凡之处在于它的简单。简单的方法也是正确的方法，同时武术也没有什么特别之处。越接近武术的真谛，招式表现上的浪费越少。 
<p align="right">--李小龙，截拳道创始人<p>

##1. 什么是Python与pip

Python, 是一种面向对象、解释型计算机程序设计语言。Python语言具有简洁、易读以及可扩展性，且Python具有丰富和强大的库。

pip是一个安装和管理Python包的工具，通过pip可以便捷的安装、更新和卸载python包。

##2. 安装
###（1）安装python 2.7
下载地址：[https://www.python.org/downloads/](https://www.python.org/downloads/)

* windows，下载exe包，一路下一步就就可以了
* linux，下载源码tar包，解压后按顺序执行：./configure > make > make install
* mac, 下载pkg包，双击安装

安装完成后，在命令行里面运行python -V测试下

```
$python -V
``` 

最后，为啥要下载2.7，不下载3.X，因为第三方类库的兼容性。。。

如果你没感觉到，就当我没说，你随便用哪个都行。

 
###（2）安装pip：
（1）访问官网下载地址：

[https://pypi.python.org/pypi/pip/](https://pypi.python.org/pypi/pip/) 

（2）解压tar包，并通过cmd命令进入该目录

```
$ cd path/to/pip				#进入pip解压后的目录
$ python setup.py install		#安装pip
```
<i>所有的python tar形势的包都可以用上面的下载、解压、install方式安装</i>
	
（3）测试是否安装成功

```
$ pip -V						#这里是大写的字母V
pip 8.1.2 from /Library/Python/2.7/site-packages (python 2.7)
```
<i>如未运行成功，请将pip安装路径添加至系统环境变量PATH中</i>


##3. 使用
###（1）python基础用法

Python是一种解释性语言，即不需要预先编译成字节码文件就可以直接执行。我们使用print做一个例子，将print “Hello World”写入一个扩展名为.py的文件中，直接使用python执行。

```
$echo print(\"Hello World\") > hello.py
$python hello.py
Hello World
```
此外，Python还具有交互能力。可以直接调用python解释器直接与其进行交互，就像和它“对话”一样。

```
$python							#1. 终端命令行里面输入python命令
Python 2.7.10 (default, Oct 23 2015, 19:19:21) 
[GCC 4.2.1 Compatible Apple LLVM 7.0.0 (clang-700.0.59.5)] on darwin
>>>print("Hello World")			#2. python交互模式下直接输入python语句，按下回车键执行
Hello World
```

###（2）pip基础用法
如果你刚好需要开发一个读取excel的功能时，你该怎么办？

* 首先，不要自己造轮子，去网上看看是否有相应类库
 
自己去bing、百度搜索，阅读搜索结果的前两页，发现了xlrd模块

* 然后，使用pip安装它

```
$pip install xlrd
```
<i>这里可以通过==, >=, <=, >, <来指定一个版本号，如xlrd==0.9.4</i>

* 最后，导入该模块，对着网上的示例，或其模块API使用就可以了

```
import xlrd
```

最后，在补充三条pip常用命令，查询、升级与卸载本地包

```
$pip freeze				#查询本地已安装类库
$pip install -U xlrd	#升级本地xlrd类库至当前最新版
$pip uninstall xlrd		#下载xlrd类库
```