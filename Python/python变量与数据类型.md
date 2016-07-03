#Python变量

在Python中，变量是指存储在某个内存地址上的数据。

变量可以数字、字符串、列表、元组、字典。

####（1）变量的定义
首先，在系统终端命令行里面键入python，进入python交互模式
<!-- script:shell -->
	$python
	Python 2.7.10 (default, Oct 23 2015, 19:19:21) 
	>>>

然后，输入下列命令（其中#为注释，>>>为提示符，二者不用输入）
<!-- script:python -->
	#1. 定义两个数字型变量，用于存储原始加数num1,num2
	>>> num1 = 1
	>>> num2 = 2
	#2. 定义另一个数字型变量，用于存储num1,num2的和
	>>> result = num1 + num2
	#3. 输出result
	>>> print(result)
	3

OK，定义变量完成1次加法运算的功能已经完成了。

但是，输出结果太丑有木有？我们来完善下：
<!-- script:python -->
	#4. 定义一个字符串类型的输出信息模板
	>>> msg = "[Maths-Plus]: %s + %s = %s" % (num1, num2, result)
	#5. 输出msg
	>>> print(msg)
	[Maths-Plus]: 1 + 2 = 3
现在输入结果是否好看多了呢？ 这里我们用到了字符串

####（2）变量类型的查看
通过上述的例子不难看出，python是不用显示声明变量类型的。

那我们如何知道一个变量到底是什么类型的呢？我们用type()
<!-- script:python -->
	#6. 查看num1的变量类型
	>>> type(num1)
	<type 'int'>
	#7. 查看msg的变量类型
	>>> type(num2)
	<type 'str'>
	