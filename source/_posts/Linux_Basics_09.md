---
title: Linux_Basics_09
date: 2020-08-09 18:23:43
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

	同穴窅冥何所望，他生缘会更难期。
	惟将终夜长开眼，报答平生未展眉。

[Linux命令手册][Linux]

[Linux]: https://man.linuxde.net/

[阿里云][aliyun]

[aliyun]: http://www.aliyun.com

[菜鸟教程][Runoob]

[Runoob]: http://www.runoob.com/

[百度][Baidu]

[Baidu]: http://www.baidu.com

[Google][Google]

[Google]: http://google.com.hk

# Shell基础 
	一、关于shell
	1、什么是shell
	什么是shell？
	Shell（外壳） 是一个用 C 语言编写的程序，它是用户使用 Linux 的桥梁。Shell 既是一种命令语言，又是一种程序设计语言。
	Shell 是指一种应用程序，这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的服务。

	什么是脚本？
	脚本简单地说就是一条条的文字命令，这些文字命令是可以看到的（如可以用记事本打开查看、编辑）。
	常见的脚本： JavaScript（JS，前端），VBScript， ASP，JSP，PHP（后端），SQL（数据库操作语言），Perl，Shell，python，Ruby，JavaFX，Lua等。

	为什么要学习和使用shell？
	Shell属于内置的脚本
	程序开发的效率非常高，依赖于功能强大的命令可以迅速地完成开发任务（批处理）
	语法简单，代码写起来比较轻松，简单易学

	常见的shell种类？
	在linux中有很多类型的shell，不同的shell具备不同的功能，shell还决定了脚本中函数的语法，Linux中默认的shell是/bin/bash（重点），流行的shell有ash、bash、ksh、csh、zsh等，不同的shell都有自己的特点以及用途。

	csh
	C shell 使用的是“类C”语法,csh是具有C语言风格的一种shell，其内部命令有52个，较为庞大。目前使用的并不多，已经被/bin/tcsh所取代。

	ksh
	Korn shell 的语法与 Bourne shell 相同，同时具备了 C shell 的易用特点。许多安装脚本都使用 ksh ，ksh有42条内部命令，与bash相比有一定的限制性。

	tcsh
	tcsh是csh的增强版，与 C shell 完全兼容。

	sh 
	是一个快捷方式，已经被/bin/bash所取代。

	nologin
	指用户不能登录

![](/hexo-private-blog-website/images/shell.png)
![](/hexo-private-blog-website/images/shell1.png)
![](/hexo-private-blog-website/images/shell2.png)

	zsh
	目前Linux里最庞大的一种shell：zsh。它有84个内部命令，使用起来也比较复杂。一般情况下，不会使用该shell。

	bash
	大多数Linux系统默认使用的shell，bash shell 是 Bourne shell 的一个免费版本，它是最早的 Unix shell，bash还有一个特点，可以通过help命令来查看帮助。包含的功能几乎可以涵盖shell所具有的功能，所以一般的shell脚本都会指定它为执行路径。

	2、shell入门
	编写规范：
	代码规范：
		#!/bin/bash				[指定告知系统当前这个脚本要使用的shell解释器]
		Shell相关指令

	文件命名规范：
		文件名.sh				.sh是linux下bash shell 的默认后缀

	使用流程：
	①创建.sh文件			touch/vim
	②编写shell代码
	③执行shell脚本			脚本必须得有执行权限

	案例1：创建test.sh，实现第一个shell脚本程序，输出hello world.
	输出命令：#echo 123
	注意：输出的内容如果包含字母和符号（不包含变量），则需要用引号包括起来。如果是纯数字可以包也可以不包。

![](/hexo-private-blog-website/images/shell3.png)

![](/hexo-private-blog-website/images/shell4.png)

	注意，这里在运行时一定要写成 ./test.sh，而不是 test.sh，运行其它二进制的程序也一样，直接写 test.sh，Linux 系统会去 PATH（环境变量） 里寻找有没有叫 test.sh 的，而只有 /bin, /sbin, /usr/bin，/usr/sbin 等在 PATH 里，你的当前目录通常不在 PATH 里，所以写成 test.sh 是会找不到命令的，要用 ./test.sh 告诉系统说，就在当前目录找。

	案例2：使用root用户帐号创建并执行test2.sh，实现创建一个shelltest用户，并在其家目录中新建文件try.html。

![](/hexo-private-blog-website/images/shell4.png)

	脚本执行的另外一个方式：/bin/bash 脚本的路径（了解）

![](/hexo-private-blog-website/images/shell4.png)

	Shell脚本分为简单的写法（简单命令的堆积）和复杂写法（程序的设计）

	二、shell进阶（重点）
	1、变量（重点）
	1.1、变量的含义
	a. 什么是量
	量就是数据.
	b. 什么是变量

![](/hexo-private-blog-website/images/shell5.png)

	数据可以发生改变就是变量.
	在一个脚本周期内,其值可以发生改变的量就是变量.
	c. 什么叫做一个脚本周期
	一个脚本周期我们可以简单的理解为当前的shell文件

	变量是shell中不可或缺的一部分，也是最基础、最重要的组成部分。

	1.2、变量的定义与使用（重点）
	变量，先定义后使用。

![](/hexo-private-blog-website/images/shell6.png)

	定义形如：class_name="yunwe "
	使用形如：echo $class_name

	变量就是由2部分组成,一个是变量名（左边），另外一部分是变量的值（右边）
	变量名和变量值是什么关系??
	变量名和变量值是使用和被使用关系; 我们的变量名来使用变量值;

	在使用变量的时候一定需要在变量名前面添加一个$符号，该要求在其他语言中也存在的（例如php）。

	变量名的规范
	注意，变量名后面的等号左右不能有空格，这可能和你熟悉的所有编程语言都不一样。同时，变量名的命名须遵循如下规则：
	命名只能使用英文字母，数字和下划线，首个字符不能以数字开头。
	中间不能有空格，可以使用下划线“_”。
	不能使用标点符号。
	不能使用bash里的关键字（可用help命令查看保留关键字）。

	问题：以下哪个shell变量名是合法的？
	A. var		B.?var       C. user*name       D.echo

	案例1：使用变量改写入门脚本中的第1个shell脚本。

![](/hexo-private-blog-website/images/shell7.png)

![](/hexo-private-blog-website/images/shell8.png)

	关于单双引号的问题：
	双引号能够识别变量，双引号能够实现转义（类似于“\*”）
	单引号是不能识别变量，只会原样输出，单引号是不能转义的

	案例2：定义一个变量，输出当前时间，要求格式为“年-月-日 时:分:秒”。

![](/hexo-private-blog-website/images/shell8.png)

	注意：反引号（esc键下方的那个键），当在脚本中需要执行一些指令并且将执行的结果赋给变量的时候需要使用“反引号”。

![](/hexo-private-blog-website/images/shell8.png)

	1.3、只读变量（了解）
	语法：readonly 变量名

	案例：定义变量a并且其值为10，随后设置其为只读变量，再去尝试重新赋值

![](/hexo-private-blog-website/images/shell9.png)

	1.4、接收用户输入（重点）
	语法：read  -p  提示信息  变量名

	案例：编写一个脚本test6.sh，要求执行之后提示用户输入文件的名称（路径），然后自动为用户创建该文件

![](/hexo-private-blog-website/images/shell10.png)

	1.5、删除变量（了解）
	语法：unset 变量名

	案例：定义变量b=20，再输出b的值，随后删除b，最后再输出下b

![](/hexo-private-blog-website/images/shell10.png)


![](/hexo-private-blog-website/images/shell11.png)

	2、条件判断语句
	老婆给当程序员的老公打电话：下班顺路买一斤包子带回来，如果看到卖西瓜的，买一个。当晚，程序员老公手捧一个包子进了家门…老婆怒道：你怎么就买了一个包子？！老公答曰：因为看到了卖西瓜的。


![](/hexo-private-blog-website/images/shell11.png)


	把程序员老婆的话当作一段需求分析一下吧。买一斤包子是一个确定无疑的需求项，无论后面是什么情况什么条件，前面这一斤包子是肯定要买的。看到卖西瓜的是一个条件判断，后面“买一个”是一个模糊不清的需求项，买一个什么呢？需求里没说啊。客户把这个当作开发人员默认了解的内容了。可是作为一个成熟合格的程序员，该老婆的丈夫应该马上跟进确认需求“买一个什么？”，要不然程序可怎么写呢？所以笑话里该程序员是不合格的，起码是不积极不负责的。在没有明确需求的情况下，他只能按照自己的理解来完成工作了。那比较可能的结果就有如下几种：
	1 看到卖西瓜的，买一个西瓜
		如果 看到卖西瓜的
			那么
			买一个西瓜
		否则
			买一斤包子
	2 看到卖西瓜的，买一个包子
		如果 看到卖西瓜的
			那么
			买一个包子
	3 看到卖西瓜的，买一个卖西瓜的
	4 看到卖西瓜的，买一个老婆一直想买的东西
	5 看到卖西瓜的，随便买一个东西

	上述1和2下面的条件汉字描述称之为“伪代码”，也是属于条件表达式的语法。

![](/hexo-private-blog-website/images/shell12.png)

	语法1（一个条件）：
	if condition
	then
	    command1 
	    command2
	    ...
	fi

	单行写法（一般在命令行中执行的时候）：if [ condition ]; then command; fi


	语法2（两个条件）：
	if condition
	then
	    command1 
	    command2
	    ...
	else
	    command
	fi

![](/hexo-private-blog-website/images/shell13.png)

	语法3（多个条件）：
	if condition1
	then
	    command1
	elif condition2 
	then 
	    command2
	else
	    commandN
	fi
	3、运算符
	在shell中，运算符和其他编程脚本语言一样，常见的有算数运算符、关系运算符、
	逻辑运算符、字符串运算符、文件测试运算符等
	3.1、算数运算符
	下表列出了常用的算术运算符，假定变量 a 为 10，变量 b 为 20：

![](/hexo-private-blog-website/images/shell14.png)

	注意：条件表达式要放在方括号之间，并且要有空格，例如: [$a==$b] 是错误的，必须写成 [ $a == $b ]。
	原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用。
	expr 是一款表达式计算工具，使用它能完成表达式的求值操作。
	例如，两个数相加(注意使用的是反引号 ` 而不是单引号 ')：
	#!/bin/bash
	val=`expr 2 + 2`
	echo "两数之和为 : $val"

	两点注意：
	表达式和运算符之间要有空格，例如 2+2 是不对的，必须写成 2 + 2，这与我们熟悉的大多数编程语言不一样。
	完整的表达式要被 ` ` 包含，注意这个字符不是常用的单引号，在 Esc 键下边。

![](/hexo-private-blog-website/images/shell15.png)

![](/hexo-private-blog-website/images/shell16.png)

![](/hexo-private-blog-website/images/shell17.png)

![](/hexo-private-blog-website/images/shell18.png)

	3.2、关系运算符
	关系运算符只支持数字，不支持字符串，除非字符串的值是数字。
	下表列出了常用的关系运算符，假定变量 a 为 10，变量 b 为 20：

![](/hexo-private-blog-website/images/shell18.png)

![](/hexo-private-blog-website/images/shell19.png)

	-eq：equal
	-ne：not equal
	-gt：great than
	-lt：less than
	-ge：great than or equal
	-le：less than or equal

	案例：使用a=10，b=20来实现本案例

![](/hexo-private-blog-website/images/shell19.png)

![](/hexo-private-blog-website/images/shell20.png)

![](/hexo-private-blog-website/images/shell21.png)

	课堂作业：
	写一个脚本，判断当前输入的用户是否存在。如果存在则提示“用户存在”否则提示“用户不存在”。

![](/hexo-private-blog-website/images/shell22.png)


	3.3、逻辑运算符
	下表列出了常用的布尔运算符，假定变量 a 为 10，变量 b 为 20：


![](/hexo-private-blog-website/images/shell23.png)

	或运算：一个为真即为真，全部为假才是假
	与运算：一个为假即为假，全部为真才是真

![](/hexo-private-blog-website/images/shell23.png)

![](/hexo-private-blog-website/images/shell24.png)

	3.4、字符串运算符
	下表列出了常用的字符串运算符，假定变量 a 为 "abc"，变量 b 为 "efg"：


![](/hexo-private-blog-website/images/shell25.png)

	案例：将上述的语法验证下

![](/hexo-private-blog-website/images/shell26.png)

![](/hexo-private-blog-website/images/shell27.png)

	3.5、文件测试运算符（重点）
	文件测试运算符用于检测 Unix/Linux 文件的各种属性。
	属性检测描述如下：


![](/hexo-private-blog-website/images/shell27.png)

![](/hexo-private-blog-website/images/shell28.png)

	案例：测试上述标绿色的效果

![](/hexo-private-blog-website/images/shell29.png)

	注意：权限几个判断，如果只有一个部分符合，则认为是有权限的。

![](/hexo-private-blog-website/images/shell29.png)

	4、shell脚本附带选项（重点）
	问题描述：在linux shell中如何处理tail -10 access.log这样的命令行选项？
	步骤：
		调用tail指令
		系统把后续选项传递给tail
		Tail先去打开指定的文件
		取出最后10行

	问题：自己写的shell是否也可以像内置命令一样传递一些选项呢？
	答：可以的，传递方式与上述的描述是一样的，关键是怎么接收。例如：
	传递：
	#./test.sh  a  b  c
	接收：
	在脚本中可以用“$1”来表示a，“$2”来表示b，以此类推。

	接收可以用“$”加上选项对应的序号即可。


	测试：编写test14.sh，传递a，b，c，输出其值

![](/hexo-private-blog-website/images/shell30.png)

	其实$1、$2是变量。

![](/hexo-private-blog-website/images/shell30.png)

	练习：创建自定义指令“user”，可以直接执行，要求该指令具备以下语法和功能：
		a. #user -add 用户名			【添加用户】
		b. #user -del 用户名			【删除用户及其家目录】

![](/hexo-private-blog-website/images/shell31.png)

	同时题目中要求是指令，所以可以再去添加个别名：

![](/hexo-private-blog-website/images/shell31.png)

![](/hexo-private-blog-website/images/shell32.png)


	三、作业
	1、尝试写一个shell的简易计算器功能，实现加减乘除。
	2、作业：使用-e文件测试运算符，改写“1.4接收用户输入”的案例，在创建文件的时候需要先判断是否存在，如果存在则提示用户并且不执行创建操作，如果不存在则创建。
	3、尝试创建一个shell脚本，该脚本要求可以类似于“touch”指令一样，能够使用“touch 文件路径”的形式进行创建文件操作，并且要求创建好的文件权限默认为755。 

![](/hexo-private-blog-website/images/shell32.png)


	MySQL基础 
	一、关于数据库
	1、什么是数据库

![](/hexo-private-blog-website/images/mysql.png)

	如果一个项目是动态（内容会变化的，网页后缀.jsp、.php、.shtml等）内容的话，则数据库是必不可少的一个环节。
	2、MySQL简介
	MySQL是一个关系型数据库管理系统，由瑞典MySQL AB 公司开发，2008年被Sun公司收购，目前属于 Oracle 旗下产品。MySQL 是最流行的数据库管理系统之一，在 WEB 应用方面，MySQL是最好的 RDBMS (Relational Database Management System，关系数据库管理系统) 应用软件。其和php是黄金搭档（LAMP/LNMP）。
	3、常见的其他数据库软件
	目前市场上还有：Oracle（重量级的数据库）、MS SQL Server（微软）、Access（微软）、PostgreSQL、DB2、Mariadb（MySQL分支，完全兼容MySQL）。

![](/hexo-private-blog-website/images/mysql1.png)

	二、MySQL的安装与初始化
	操作之前先确保计算机时间准确。
	1、Linux下的软件安装方式（初步）
	a. 源码包（建议）
		优点
	开源，如果有足够的能力，可以修改源代码
	编译安装，更加适合自己的系统，稳定高效
	缺点
	安装步骤较多，容易出错
	编译过程时间较长

![](/hexo-private-blog-website/images/mysql2.png)

	案例：使用源码编译安装方式安装ncurses（一种常用的终端库）
	扩展：解包
	常用语法：
			#tar  -zxvf  *.tar.gz		（大多数）
			#tar  -jxvf  *.tar.bz2
	选项含义：
	-z或--gzip或--ungzip：通过gzip指令处理文件；
	-x或--extract或--get：从文件中还原文件；
	-v：显示操作过程；
	-f或--file：指定一个文件；
	-j：支持bzip2解压文件；
	①先将软件包传递到服务器上“/usr/local/src”

![](/hexo-private-blog-website/images/mysql3.png)

	②解压需要安装的源码包
	# tar -zxvf ncurses-6.1.tar.gz

![](/hexo-private-blog-website/images/mysql3.png)

	③切换到源码文件夹，然后执行后续操作

![](/hexo-private-blog-website/images/mysql3.png)

	配置（config/configure/bootstrap） → 编译（make/bootstrapd） → 安装（make install/bootstrapd install）

	配置操作主要是指定软件的安装目录、需要的依赖在什么地方、指定不需要可选依赖、配置文件的路径、通用数据存储位置等等。
	指定安装的路径：--prefix=路径
	需要依赖的路径：--with-PACKAGE名=[包所在的路径]
	不需要依赖：--without-PACHAGE名

	# ./configure --prefix=/usr/local/ncurses

![](/hexo-private-blog-website/images/mysql4.png)

	# make

![](/hexo-private-blog-website/images/mysql4.png)

	# make install

![](/hexo-private-blog-website/images/mysql5.png)

	成功之后查看目录（可选）：

![](/hexo-private-blog-website/images/mysql5.png)

	b. 二进制包（rpm）
		优点: 包管理系统简单，只需要几个命令就可以实现包的安装，升级，查询和卸载
	缺点: 经过编译，不再可以看到源代码

	回顾rpm相关指令：
	#rpm -qa|grep 关键词
	#rpm -e 关键词 [--nodeps]
	#rpm -ivh 完整名称
	#rpm -Uvh 完整名称
	#rpm -qf 文件路径			【查询指定文件属于哪个包】

![](/hexo-private-blog-website/images/mysql6.png)

	案例：使用二进制包安装lynx（一款纯命令行的浏览器）
	在光盘中就有这个包

![](/hexo-private-blog-website/images/mysql6.png)

	例如查看百度：#lynx --dump www.baidu.com

![](/hexo-private-blog-website/images/mysql6.png)

	c. yum等傻瓜式安装
	优点: 安装简单，快捷
	缺点: 完全丧失了自定义性
	注意：如果不更改软件来源的情况下，是需要联网才能使用yum的。

	常用的yum指令：
	#yum  list   [installed]   		列出当前已经装的和可以装的软件（全部）
	#yum  search	名       		搜索指定的关键词的包
	#yum  [-y]  install   包名		安装指定的包（-y表示允许不再确认）
	#yum  [-y]  update  [包名]		更新指定的包，不指定包则更新全部软件
	#yum  [-y]  remove  包名		卸载指定的包

	案例：使用yum指令卸载火狐浏览器
	#yum remove firefox

![](/hexo-private-blog-website/images/mysql7.png)

	案例：使用yum指令安装火狐浏览器
	#yum install firefox

![](/hexo-private-blog-website/images/mysql8.png)

	2、安装MySQL（重点）
	注：此处安装以yum安装为例
	2.1、MySQL安装
	#yum install mysql-server

![](/hexo-private-blog-website/images/mysql8.png)

![](/hexo-private-blog-website/images/mysql9.png)

	完成的：

![](/hexo-private-blog-website/images/mysql9.png)

	2.2、MySQL初始化
	#service mysqld start

![](/hexo-private-blog-website/images/mysql10.png)

	查看端口号（默认端口号3306）：

![](/hexo-private-blog-website/images/mysql10.png)

	# mysql_secure_installation
	Enter current password for root (enter for none):请输入当前root用户的密码，如果没有按回车，注意此root并非linux的root用户。

	Set root password?是否设置root密码？
	需要设置的密码：qhabOfhlluB9

	Remove anonymous users?是否移除匿名用户，选择移除（Y）

	Disallow root login remotely?是否不允许root远程登录（默认不允许）

	Remove test database and access to it?是否移除测试数据库（建议先不移除）

	Reload privilege tables now?是否重新加载权限表（当我们更改了mysql用户相关的信息之后建议去重载权限）

![](/hexo-private-blog-website/images/mysql10.png)

![](/hexo-private-blog-website/images/mysql11.png)


	2.3、MySQL的启动控制
	语法：service mysqld start/stop/restart

	进入mysql的方式：
	#mysql  -u用户名  -p

	退出MySQL到linux命令行：
	mysql > exit

![](/hexo-private-blog-website/images/mysql11.png)

	2.4、默认目录/文件位置（了解）
	数据库存储目录：/var/lib/mysql
	配置文件：/etc/my.cnf

![](/hexo-private-blog-website/images/mysql12.png)

	三、MySQL的基本操作（难点）
	1、名词介绍
	以Excel文件举例：
	数据库：可以看作是整个excel文件。
	数据表：可以看作是一个excel文件中的工作表。
	行（记录）：可以看作是一个工作表中的一行
	列（字段）：可以看作是一个工作表总的一列
	2、库操作（重点）
	以下命令在MySQL终端命令行中执行（大小写均可）：
	SHOW DATABASES;				显示当前MySQL中全部的数据库
	CREATE DATABASE 库名;			创建数据库
	DROP DATABASE 库名;			删除数据库
	USE 库名;					切换数据库

	Show databases效果

![](/hexo-private-blog-website/images/mysql13.png)

	创建数据库：创建yunwei数据库

![](/hexo-private-blog-website/images/mysql14.png)

	删除数据库：删除yunwei数据库

![](/hexo-private-blog-website/images/mysql14.png)

	切换数据库：切换到test数据库

![](/hexo-private-blog-website/images/mysql14.png)

	3、表操作
	SHOW TABLES;				显示当前数据库中所有的表名（必须先use数据库）	
	CREATE TABLE 表名称				在当前数据库下创建数据表
	(
	列名称1 数据类型 [NOT NULL AUTO_INCREMENT],
	列名称2 数据类型,
	列名称3 数据类型,
	....,

![](/hexo-private-blog-website/images/mysql15.png)

	PRIMARY KEY(主键字段名)
	);
	常见的数据类型：int（整型）、char（定长字符）、varchar（不定长字符）。
	主键一般就是序号所在的那一列（主键不能重复）。
	DESC 表名;				描述一个数据表（查看表结构）
	DROP TABLE [IF EXISTS] 表名;			删除一个数据表

	案例：使用上述的语法
	查看所有的数据表
	创建数据表（去test库中创建）
	要求：表名xg，要求有字段如下：
		Id字段，11位整型，不为空，自增，主键
		Username字段，varchar类型，20长度
		Password字段，char类型，32长度
	SQL（standard query language）语句：
	Create table xg(
		Id int(11) not null auto_increment,
	Username varchar(20),
	Password char(32),
	Primary key(id)
	);

![](/hexo-private-blog-website/images/mysql16.png)

![](/hexo-private-blog-website/images/mysql17.png)

	创建数据表（去test库中创建）
	要求：表名xg，要求有字段如下：
		Id字段，11位整型，不为空，自增，主键
		Username字段，varchar类型，20长度
		Password字段，char类型，32长度
	SQL（standard query language）语句：
	Create table xg(
		Id int(11) not null auto_increment,
	Username varchar(20),
	Password char(32),
	Primary key(id)
	);

![](/hexo-private-blog-website/images/mysql17.png)

查看表结构：

![](/hexo-private-blog-website/images/mysql18.png)

删除数据表：

![](/hexo-private-blog-website/images/mysql18.png)

	4、记录/字段操作（重点）
	4.1、增加记录
	语法1：INSERT INTO 表名称 VALUES (值1, 值2,....);
	语法2：INSERT INTO 表名称 (列1, 列2,...) VALUES (值1, 值2,....);

	案例：往数据表xg表中新增一个记录username为zhangsan，password为123456（加密结果E10ADC3949BA59ABBE56E057F20F883E）
	Sql语句：
	insert into xg (username,password) values (‘zhangsan’,’E10ADC3949BA59ABBE56E057F20F883E’)

![](/hexo-private-blog-website/images/mysql19.png)

![](/hexo-private-blog-website/images/mysql20.png)

	要求前面的列名与值要一一对应。

	4.2、更新记录
	语法：UPDATE 表名称 SET 列名称1 = 新值1,列名称2 = 新值2… WHERE 列名称 = 某值;
	案例：使用更新语句更新id大于等于2的记录，将其密码改为：25F9E794323B453885F5181F1B624D0B
	SQL语句：
	Update xg set password = ‘25F9E794323B453885F5181F1B624D0B’ where id >= 2;

![](/hexo-private-blog-website/images/mysql20.png)

	以后在执行影响行数的sql操作的时候一定需要注意条件是否写错或者漏写。

![](/hexo-private-blog-website/images/mysql20.png)

![](/hexo-private-blog-website/images/mysql21.png)

	4.3、查询记录
	SELECT 列名称1,列名称2… FROM 表名称 WHERE 条件;
	SELECT * FROM 表名称 WHERE 条件;
	案例：查询刚才新增的记录
	只查询用户名和密码，并且是id=2的：
	Select username,password from xg where id = 2;

![](/hexo-private-blog-website/images/mysql21.png)

	查询全部：
	Select * from xg;

![](/hexo-private-blog-website/images/mysql21.png)

![](/hexo-private-blog-website/images/mysql22.png)

	4.4、删除记录
	DELETE FROM 表名称 WHERE 列名称 = 值;

	案例：删除id为2的记录
	Delete from xg where id = 2;

![](/hexo-private-blog-website/images/mysql22.png)

![](/hexo-private-blog-website/images/mysql23.png)

	5、备份与还原（重点）
	5.1、备份（导出）
	全量备份（数据+结构）：#mysqldump -uroot -p123456 -A > 备份文件路径
	指定库备份（数据+结构）：# mysqldump -uroot -p123456 库名 > 备份文件路径
	多个库备份（数据+结构）：# mysqldump -uroot -p123456 --databases db1 db2 >  备份文件路径

	案例：备份整个库
	# mysqldump -uroot -pqhabOfhlluB9 -A > /root/sql_201804061609.sql

![](/hexo-private-blog-website/images/mysql23.png)

![](/hexo-private-blog-website/images/mysql24.png)

	案例：每1分钟自动备份1次test数据库

![](/hexo-private-blog-website/images/mysql24.png)

	计划任务编写

![](/hexo-private-blog-website/images/mysql24.png)

	等待几分钟观察目录情况：

![](/hexo-private-blog-website/images/mysql24.png)

![](/hexo-private-blog-website/images/mysql25.png)

	5.2、还原（导入）
	还原部分分（1）mysql命令行source方法 和 （2）系统命令行方法
	1.还原全部数据库:
	(1) mysql命令行：mysql> source 备份文件路径
	(2) 系统命令行： #mysql -uroot -p123456 < 备份文件路径
	2.还原单个数据库(需指定数据库)
	(1) mysql> use 库名
	mysql> source 备份文件路径
	(2) #mysql -uroot -p123456 库名 < 备份文件路径
	3.还原单个数据库的多个表(需指定数据库)

![](/hexo-private-blog-website/images/mysql25.png)

![](/hexo-private-blog-website/images/mysql26.png)

	(1) mysql> use 库名
	mysql> source 备份文件路径
	(2) mysql -uroot -p123456 库名 < 备份文件路径
	4.还原多个数据库，（一个备份文件里有多个数据库的备份，此时不需要指定数据库）
	(1) mysql命令行：mysql> source 备份文件路径
	(2) 系统命令行： mysql -uroot -p123456 < 备份文件路径

	案例1：人为删除xg表（模拟数据表丢失），然后通过最后一次备份还原数据表。
	先删除数据表

![](/hexo-private-blog-website/images/mysql26.png)

![](/hexo-private-blog-website/images/mysql27.png)

	还原操作：

![](/hexo-private-blog-website/images/mysql27.png)

	案例2：需要还原sql文件到test库（mobile.sql 31万条数据）

![](/hexo-private-blog-website/images/mysql27.png)

	设置Mysql连接字符集：
		Mysql> set names utf8;			【三码一致，服务器端+传输过程中+客户端】

![](/hexo-private-blog-website/images/mysql27.png)

![](/hexo-private-blog-website/images/mysql28.png)

	四、扩展
	1、mysql的远程管理工具
	分为两大类：B/S架构、C/S架构。
	B/S：B是指浏览器，S是指服务器。例如：百度搜索应用就属于BS架构软件。
	C/S：C是指客户端，S是指服务器。例如：QQ、电脑端微信等应用程序都是CS架构。

	在BS中，mysql有个典型的管理工具：PMA（phpMyAdmin）

![](/hexo-private-blog-website/images/mysql28.png)

![](/hexo-private-blog-website/images/mysql29.png)

![](/hexo-private-blog-website/images/mysql30.png)

	CS中比较典型的软件：navicat、mysql workbrach

![](/hexo-private-blog-website/images/mysql30.png)

	要解决的问题：允许mysql远程登录

![](/hexo-private-blog-website/images/mysql30.png)

	a. 先进入数据库选择mysql数据库；
	b. 执行sql语句：select host,user from user;

![](/hexo-private-blog-website/images/mysql30.png)

![](/hexo-private-blog-website/images/mysql31.png)

	c. 将其中的一个记录的host值改为“%”，表示可以允许任何地方登录

![](/hexo-private-blog-website/images/mysql31.png)

	d. 刷新权限表或者重启mysql
	刷新权限：mysql> flush privileges;

![](/hexo-private-blog-website/images/mysql31.png)

![](/hexo-private-blog-website/images/mysql32.png)

e. navicat登录成功

![](/hexo-private-blog-website/images/mysql32.png)

	五、作业
	1、导入sql文件（mobile.sql）到test数据库，并使用该数据表进行数据的增删改查练习。
	2、请编写一个简单的shell脚本，文件名为autoBakup.sh，并写出计划任务指令，能够实现每天0点整自动备份整个MySQL数据库。

![](/hexo-private-blog-website/images/mysql32.png)

![](/hexo-private-blog-website/images/mysql32.png)

## 省钱利器：
![](/hexo-private-blog-website/images/淘宝客21.jpg)
![](/hexo-private-blog-website/images/淘宝客22.jpg)
![](/hexo-private-blog-website/images/淘宝客23.jpg)

### 内存分布：
![](/hexo-private-blog-website/images/内存分布.bmp)
![](/hexo-private-blog-website/images/内存分布.png)
### 指针数组:
![](/hexo-private-blog-website/images/整型指针数组.bmp)
![](/hexo-private-blog-website/images/字符指针数组.bmp)
### 二维数组:
![](/hexo-private-blog-website/images/二维数组.png)
![](/hexo-private-blog-website/images/二维数组1.png)
### 支付宝打赏:
![](/hexo-private-blog-website/images/alipay.jpg)
### 微信打赏:
![](/hexo-private-blog-website/images/wechat.jpg)
### 财付通打赏：
![qq.jpg](http://ww1.sinaimg.cn/large/006DnxC4gy1gfggwd0rvjj32ai2lxdrm.jpg)



