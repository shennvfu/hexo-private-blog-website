---
title: Linux_Basics_06
date: 2020-08-06 15:45:23
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

	寥落古行宫，宫花寂寞红。
	白头宫女在，闲坐说玄宗。

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

# Linux发展史与安装
## 一、Linux发展史
	1、Linux前身-Unix
	1968年  Multics项目
	MIT、Bell实验室、美国通用电气有限公司走到了一起，致力于开发Multics项目。到后期由于开发进度不是很好，MIT和Bell实验室相继离开这个项目的开发，最终导致项目搁浅。

	1970年（Unix元年，时间戳）  Unix诞生
	当时在开发Multics项目的时候，实验室中有一个开发成员开发了一款游戏（travel space：遨游太空），因为两个实验室相继离开项目开发，导致这名开发人员没法玩游戏，后来他提议组织人员重新在Multics项目之上重新的开发，也就出现了1970年的Unix。当时Unix操作系统是使用的汇编语言（机器语言）开发的。

	1973年  用C语言重写Unix
	因为汇编语言有一个最大的局限性：对于计算机硬件过于依赖。导致移植性不好，所以后期在1973年使用了C语言对其进行重新开发。

	1975年  Bell实验室允许大学使用Unix。

	1975年，bell实验室允许大学使用Unix操作系统用于教学作用，而不允许用于商业用途。
	2、Linux诞生
	人物  Linus
	Linux的开发作者，Linux之父，李纳斯·托瓦兹。Linux诞生时是荷兰在校大学生。
	1991年  0.0.1版本
	李纳斯当时学校使用的就是Unix操作系统，然后其对系统的底层代码进行了修改，放到了学校为学生开放的网站上，原先他把文件命名写成了Linus’s Unix，后期网络管理发现之后觉得这个名字不好，自己手动的将名字改成Linux。随后其他同学下载之后发现这个版本还是挺好用的，随后都把自己代码贡献给李纳斯。
	1992年  0.0.2版本
	1994年  1.0版本
	2003年  2.6版本
	上述所提及的版本号并不是分支版本，而是指Linux的内核版本。
	3、开源文化
	Linux是开源的操作系统。所谓开源就是指开放源代码。
	人  物  Stallman	斯特曼，开源文化的倡导人。

	1983年  GNU计划
	1985年  FSF基金会
	1990年  Emacs、GCC（c语言的编译器）、程序库  
	1991年	 Stallman去找Linus，商谈让Linux加入其开源计划（GNU计划）
	1992年  GNU/Linux

	4、Linux系统特点
	开放性（开源）、多用户、多任务、良好的用户界面、优异的性能与稳定性

	多用户多任务：
	单用户：一个用户，在登录计算机（操作系统），只能允许同时登录一个用户；
	单任务：一个任务，允许用户同时进行的操作任务数量；
	多用户：多个用户，在登录计算机（操作系统），允许同时登录多个用户进行操作；
	多任务：多个任务，允许用户同时进行多个操作任务；

	Windows属于：单用户、多任务。
	Linux属于：多用户、多任务。

	5、Linux分支
	分支：Linux分支有很多，现在比较有名的ubuntu、debian、centos（Community Enterprise Operating System）、redhat、suse等等。

## 二、Linux系统的安装

	1、安装方式
	目前安装操作系统方式有2种：真机安装、虚拟机安装。
	真机安装：使用真实的电脑进行安装，像安装windows操作系统一样，真机安装的结果就是替换掉当前的windows操作系统；
	虚拟机安装：通过一些特定的手段，来进行模拟安装，并不会影响当前计算机的真实操作系统；

	如果是学习或者测试使用，强烈建议使用虚拟机安装方式。
	2、虚拟机软件（了解）
	什么是虚拟机？
	虚拟机，有些时候想模拟出一个真实的电脑环境，碍于使用真机安装代价太大，因此而诞生的一款可以模拟操作系统运行的软件。

	虚拟机目前有2个比较有名的产品：vmware出品的vmware workstation、oracle 出品的virtual Box。

	3、虚拟机的安装
	3.1、VMware Workstation的安装
	①打开安装程序
	
	②进行下一步开始安装

	③同意许可协议

	④根据需要决定是否需要更改软件的安装位置

	点击下一步：

	⑤用户体验设置

	⑥快捷方式创建的步骤

	⑦点击安装按钮

	⑧点击完成

	⑨最重要的地方，在安装完之后需要检查，检查虚拟机软件是否有安装2个虚拟网卡

![](/hexo-private-blog-website/images/VMware虚拟网卡.png)

	如果没有这2个网卡的话，则会影响后期windows系统与虚拟机其中操作系统之间的相互通信（比如共享网络、文件传输等）。

	3.2、Virtual Box的安装

	①运行安装程序

	②点击下一步

	③选择性更改安装的位置

	④安装的选项设置

	⑤断网警告，点击是即可

	⑤点击安装按钮

	⑥完成

	⑦检查是否有对应的虚拟网卡存在

	两个软件安装完成之后在桌面上都有对应的快捷方式

	4、Linux版本的选择
	版本选择：CentOS 6.5   【镜像一般都是xxx.iso文件】
	问题：为什么不选择最新版的7.x版本？
	6.x目前依然是主流
	6.x的各种系统操作模式是基础
	7.x实际上也支持大多数6.x的操作形式

	官网：https://www.centos.org/

	从官网下载得到的镜像文件

	5、新建虚拟机
	5.1、使用VMware Workstation（重点）
	①点击“文件”菜单，选择“新建虚拟机…”选项，选择“自定义”点击下一步

![](/hexo-private-blog-website/images/VMware虚拟机1.png)

	②选择兼容性，默认即可，直接下一步

![](/hexo-private-blog-website/images/VMware虚拟机2.png)

	③选择镜像文件的时候选择“稍后….”，点击下一步

![](/hexo-private-blog-website/images/VMware虚拟机3.png)

	④选择需要安装的操作系统

![](/hexo-private-blog-website/images/VMware虚拟机4.png)

	⑤选择虚拟机的名称（名称将会后期出现在左侧）和设置虚拟系统的安装位置

![](/hexo-private-blog-website/images/VMware虚拟机5.png)

	⑥cpu设置

![](/hexo-private-blog-website/images/VMware虚拟机6.png)

	⑦分配内存

![](/hexo-private-blog-website/images/VMware虚拟机7.png)

	⑧选择网络类型，选择nat即可
	NAT：配置好之后windows即可和虚拟机进行互相通信，但是教室内的其他同学是访问不了的，只能自己可以访问虚拟机中的操作系统；
	桥接：配置好之后其他同学也可以访问你的虚拟机操作系统；

![](/hexo-private-blog-website/images/VMware虚拟机8.png)

	⑨后续默认的步骤，直接下一步

	⑩点击完成

![](/hexo-private-blog-website/images/VMware虚拟机9.png)

	5.2、使用Virtual Box（了解）

	①点击左上角的新建按钮

	②选择操作系统，由于centos在列表中是没有的，所以选择centos对应的主线redhat 32位

	③选择内存大小

	④创建磁盘，默认即可

	⑤选择磁盘类型，默认即可

	⑥选择磁盘大小的分配方式，方式为动态分配即可

	⑦设置磁盘的位置和大小确认

	6、Linux操作系统安装
	6.1、使用VMware workstation进行安装
	a. 由于之前没有指定iso镜像文件，因此此处需要先指定系统镜像文件

![](/hexo-private-blog-website/images/VMware虚拟机使用1.png)

	b. 运行此虚拟机

![](/hexo-private-blog-website/images/VMware虚拟机使用2.png)

	注意：如果开机之后鼠标点进去虚拟机出不来，则可以按下组合快捷键“ctrl+alt”。

	如果启动之后出现类似提示框（不是错误框）则勾选不再提示，并且确定即可：

![](/hexo-private-blog-website/images/VMware虚拟机使用3.png)

	如果在启动时候出现下述错误，则说明电脑没有开启cpu的 虚拟化，如果需要开启，则需要重启计算机，并且在开启的时候进入主板的BIOS设置开启虚拟化，然后保存设置重启电脑：

	c. 选择升级/安装已经存在的系统（通过↑/↓方向键）按下回车

![](/hexo-private-blog-website/images/CentOS系统安装1.png)

	d. 在检测到光盘（disc）之后选择跳过完整性检测直接进行安装

![](/hexo-private-blog-website/images/CentOS系统安装2.png)

	随后提示不支持的硬件，忽略直接下一步

![](/hexo-private-blog-website/images/CentOS系统安装3.png)

	e. 点击下一步

![](/hexo-private-blog-website/images/CentOS系统安装4.png)

	f. 选择在安装过程中使用的语言

![](/hexo-private-blog-website/images/CentOS系统安装5.png)

	g. 选择键盘类型，美国式英语

![](/hexo-private-blog-website/images/CentOS系统安装6.png)

	h. 选择存储设备类型

![](/hexo-private-blog-website/images/CentOS系统安装7.png)

	i. 对磁盘进行空白盘的初始化操作，选择“是，忽略所有的数据”

![](/hexo-private-blog-website/images/CentOS系统安装8.png)

	j. 设置网卡自动连接，依次应用 – 关闭 – 下一步

![](/hexo-private-blog-website/images/CentOS系统安装9.png)

	k. 设置时区，默认亚洲/上海

![](/hexo-private-blog-website/images/CentOS系统安装10.png)

	l. 设置密码，设置好了之后下一步

![](/hexo-private-blog-website/images/CentOS系统安装11.png)

	m. 使用全部的磁盘空间来安装Linux系统，点击下一步

![](/hexo-private-blog-website/images/CentOS系统安装12.png)
![](/hexo-private-blog-website/images/CentOS系统安装13.png)

	n. 选择安装的Linux类型

![](/hexo-private-blog-website/images/CentOS系统安装14.png)
![](/hexo-private-blog-website/images/CentOS系统安装15.png)

	o. 选择开发 – 开发工具，前面复选框√，点击下一步

![](/hexo-private-blog-website/images/CentOS系统安装16.png)

	p. 等待软件包的安装

![](/hexo-private-blog-website/images/CentOS系统安装17.png)

	等待完成，点击重新引导

![](/hexo-private-blog-website/images/CentOS系统安装18.png)

	q. 重新引导之后点击“前进”

![](/hexo-private-blog-website/images/CentOS系统安装19.png)

	r. 在协议许可界面选择同意，然后点击前进

![](/hexo-private-blog-website/images/CentOS系统安装20.png)

	s. 创建普通用户帐号（可选），然后点击前进

![](/hexo-private-blog-website/images/CentOS系统安装21.png)

	t. 时间设置，设置好之后前进

![](/hexo-private-blog-website/images/CentOS系统安装22.png)

	u. 关于kdump，之后点击完成

![](/hexo-private-blog-website/images/CentOS系统安装23.png)

	v. 登录界面

![](/hexo-private-blog-website/images/CentOS系统安装24.png)

	如果需要使用非列出的用户进行登录则点击其他，否则双击列出的用户名即可，随后输入密码。

	w. 使用root帐号登录之后的提示

![](/hexo-private-blog-website/images/CentOS系统安装25.png)

	x. 看到的桌面

![](/hexo-private-blog-website/images/CentOS系统安装26.png)


	6.2、使用virtual Box安装Linux（了解）
	a. 选择需要安装的系统镜像

	b. 启动虚拟机

	注意：如果鼠标在虚拟机中想退出到windows，则需要按下ctrl+alt组合键（空格右侧的）

	c. 后续全部操作按照上面6.1章节中的步骤继续安装即可。

	7、终端
	问题：在目前的桌面系统中，如果需要关机可以通过“系统”“关机”进行关机，那么后期服务器都是命令行模式的，届时这种方式将不好用，那会要怎么关机呢？

	答：可以通过命令行方式进行关机。命令的输入需要在终端中进行输入。

	所谓终端，其实类似于windows下cmd命令行模式。在终端中可以输入需要执行的一些指令，同样可以通过终端进行关机（注意：以后在工作中很少会去使用关机命令，会使用重启比较多）。

	终端的形式：

![](/hexo-private-blog-website/images/终端的形式.png)

	终端组成部分：

![](/hexo-private-blog-website/images/终端的组成部分.png)

	如何使用终端命令进行关机？
	在Linux中关机命令 有以下几个：shutdown -h now（正常关机）、halt（关闭内存）、init 0


	8、使用VMware备份操作系统
	在vm中备份方式有2种：快照、克隆。
	快照：又称还原点，就是保存在拍快照时候的系统的状态（包含了所有的内容），在后期的时候随时可以恢复。【侧重在于短期备份，需要频繁备份的时候可以使用快照，做快照的时候虚拟的操作系统一般处于开启状态】
	①在菜单“虚拟机”-“快照”-“拍摄快照”

	输入相关信息，点击拍摄快照

	②搞事情

	③使用快照恢复搞事情之前的状态
	路径：虚拟机 – 快照 – 快照管理器

	克隆：就是复制的意思。【侧重长期备份，做克隆的时候是必须得关闭】
	路径：先关机 – 右键需要克隆的虚拟机 – 管理 – 克隆

	克隆好的服务器相关密码帐号等信息与被克隆的系统一致。

	三、Linux系统的文件
	1、文件与文件夹（目录）
	什么是文件？
	一般都是一个独立的东西，可以通过一些特定的工具进行打开，并且其中不能在包含除了文字以外的东西。

	什么是文件夹？
	可以在其中包含其他文件的东西。

	为什么先讲文件？
	1:日常运维工作中，有近一半以上的工作内容 精力 其实都是对文件的操作。
	2: Linux 本身也是一个基于文件形式表示的操作系统。
	Linux一切皆文件。
	①在windows是文件的，在Linux下同样也是文件；
	②在windows不是文件的，在Linux下也是以文件的形式存储的；

	日常学习中和日常工作中，对于文件的操作的都有哪些种类？
	创建文件、编辑文件、保存文件、关闭文件、重命名文件、删除文件、恢复文件。

	2、Linux系统的文件目录结构

	目录结构：
	Bin：全称binary，含义是二进制。该目录中存储的都是一些二进制文件，文件都是可以被运行的。
	Dev：该目录中主要存放的是外接设备，例如U盘、其他的光盘等。在其中的外接设备是不能直接被使用的，需要挂载（类似windows下的分配盘符）。
	Etc：该目录主要存储一些配置文件。
	Home：表示“家”，表示除了root用户以外其他用户的家目录，类似于windows下的User/用户目录。
	Proc：process，表示进程，该目录中存储的是Linux运行时候的进程。
	Root：该目录是root用户自己的家目录。
	Sbin：全称super binary，该目录也是存储一些可以被执行的二进制文件，但是必须得有super权限的用户才能执行。
	Tmp：表示“临时”的，当系统运行时候产生的临时文件会在这个目录存着。
	Usr：存放的是用户自己安装的软件。类似于windows下的program files。
	Var：存放的程序/系统的日志文件的目录。
	Mnt：当外接设备需要挂载的时候，就需要挂载到mnt目录下。



# Linux的基本指令
## 一、指令与选项
	什么是Linux的指令？
	指在Linux终端（命令行）中输入的内容就称之为指令。

	一个完整的指令的标准格式：Linux通用的格式
	#指令主体（空格） [选项]（空格） [操作对象]
	一个指令可以包含多个选项
	操作对象也可以是多个

	例如：需要让张三同学帮忙去楼下小卖铺买一瓶农夫山泉水和清风餐巾纸，在这个指令中“买东西”是指令的主体，买的水和餐巾纸是操作的对象，农夫山泉、清风是操作的选项。

## 二、基础指令（重点）
	1、ls指令
	含义：ls （list）

	用法1：#ls
	含义：列出当前工作目录下的所有文件/文件夹的名称


	用法2：#ls 路径
	含义：列出指定路径下的所有文件/文件夹的名称
	关于路径（重要）：
	路径可以分为两种：相对路径、绝对路径。
	相对路径：相对首先得有一个参照物（一般就是当前的工作路径）；
		相对路径的写法：在相对路径中通常会用到2个符号“./”【表示当前目录下】、“../”【上一级目录下】。
	绝对路径：绝对路径不需要参照物，直接从根“/”开始寻找对应路径；

	用法3：#ls 选项 路径
	含义：在列出指定路径下的文件/文件夹的名称，并以指定的格式进行显示。
	常见的语法：
		#ls -l 路径
		#ls -la 路径
	选项解释：
		-l：表示list，表示以详细列表的形式进行展示
		-a：表示显示所有的文件/文件夹（包含了隐藏文件/文件夹）

![](/hexo-private-blog-website/images/ls指令的用法.png)

		上述列表中的第一列字符表示文档的类型，其中“-”表示改行对应的文档类型为文件，“d”表示文档类型为文件夹。

		在Linux中隐藏文档一般都是以“.”开头。

	用法4：#ls -lh 路径
	含义：列出指定路径下的所有文件/文件夹的名称，以列表的形式并且在显示文档大小的时候以可读性较高的形式显示


	2、pwd指令
	用法：#pwd		（print working directory，打印当前工作目录）


	3、cd指令
	命令：#cd		（change directory，改变目录）
	作用：用于切换当前的工作目录的
	语法：#cd 路径

	案例：当前在“/”下，需要使用绝对路径切换到/usr/local。

	cd /usr/local

	案例：当前在/usr/local下，需要使用相对路径切换目录到home目录下的Linux123用户家目录中去。

	cd ../../home/linux123

	补充：
	在Linux中有一个特殊的符号“~”，表示当前用户的家目录。
	切换的方式：#cd ~

	4、mkdir指令
	指令：mkdir    （make directory，创建目录）
	语法1：#mkdir 路径 【路径，可以是文件夹名称也可以是包含名称的一个完整路径】

	案例：在当前路径下创建出目录“yunweihenniux”

	mkdir yunweihenniux

	注意：ls列出的结果颜色说明，其中蓝色的名称表示文件夹，黑色的表示文件，绿色的其权限为拥有所有权限。

	案例：在指定路径下创建出一个文件夹“yunweihenniux”

	mkdir /root/yunweihenniux

	语法2：#mkdir -p 路径
	含义：当一次性创建多层不存在的目录的时候，添加-p参数，否则会报错

	语法3：#mkdir 路径1 路径2 路径3 ….  【表示一次性创建多个目录】

	5、touch指令
	指令：touch   
	作用：创建文件
	语法：#touch 文件路径	【路径可以是直接的文件名也可以是路径】

	案例：使用touch来在当前路径下创建一个文件，命名为Linux.txt

	touch linux.txt

	案例：使用touch来同时创建多个文件

	touch linux1.txt linux2.txt

	案例：使用touch来在“Linux123”用户的家目录中创建文件，Linux.txt

	touch /home/linux123/linux.txt

	6、cp指令
	指令：cp		（copy，复制）
	作用：复制文件/文件夹到指定的位置
	语法：#cp 被复制的文档路径 文档被复制到的路径

	案例：使用cp命令来复制一个文件

	cp linux1.txt /home/linux123/linux1.txt
	cp linux1.txt /home/linux123/linux10.txt

	注意：Linux在复制过程中是可以重新对新位置的文件进行重命名的，但是如果不是必须的需要，则建议保持前后名称一致。

	案例：使用cp命令来复制一个文件夹

	注意：当使用cp命令进行文件夹复制操作的时候需要添加选项“-r”【-r表示递归复制】，否则目录将被忽略

	7、mv指令
	指令：mv   （move，移动，剪切）
	作用：移动文档到新的位置
	语法：#mv 需要移动的文档路径 需要保存的位置路径

	确认：移动之后原始的文件还在不在原来的位置？原始文件是不在原始位置的

	案例：使用mv命令移动一个文件

	mv linux1.txt /linux1.txt

	案例：使用mv命令移动一个文件夹

	mv /home/linux123/yunweihenniux/ /

	补充：在Linux中重命名的命令也是mv，语法和移动语法一样。

	8、rm指令
	指令：rm （remove，移除、删除）
	作用：移除/删除文档
	语法：#rm 选项 需要移除的文档路径
	选项：
		-f：force，强制删除，不提示是否删除
		-r：表示递归

	案例：删除一个文件

	在删除的时候如果不带选项，会提示是否删除，如果需要确认则输入“y/yes”，否则输入“n/no”按下回车。

	注意：如果在删除的时候不想频繁的确认，则可以在指令中添加选项“-f”，表示force（强制）。

	案例：删除一个文件夹

	注意：删除一个目录的时候需要做递归删除，并且一般也不需要进行删除确认询问，所以移除目录的时候一般需要使用-rf选项。

	案例：删除多个文档

	rm -rf a linux.txt

	案例：要删除一个目录下有公共特性的文档，例如都以Linux开头

	rm -f linux*

	其中*称之为通配符，意思表示任意的字符，Linux*，则表示只要文件以Linux开头，后续字符则不管。

	9、vim指令
	指令：vim   （vim是一款文本编辑器）
	语法：#vim 文件的路径
	作用：打开一个文件（可以不存在，也可以存在）

	案例：使用vim来打开文件

	退出打开的文件：在没有按下其他命令的时候，按下shift+英文冒号，输入q，按下回车即可

	10、输出重定向
	一般命令的输出都会显示在终端中，有些时候需要将一些命令的执行结果想要保存到文件中进行后续的分析/统计，则这时候需要使用到的输出重定向技术。

	>：覆盖输出，会覆盖掉原先的文件内容
	>>：追加输出，不会覆盖原始文件内容，会在原始内容末尾继续添加

	语法：#正常执行的指令 > / >> 文件的路径
	注意：文件可以不存在，不存在则新建

	案例：使用覆盖重定向，保存ls -la 的执行结果，保存到当前目录下的ls.txt

	ls -la > ls.txt

	案例：使用追加重定向，保存ls -la的执行结果到ls.txt中

	ls -la >> ls.txt

	11、cat指令
	作用1：cat有直接打开一个文件的功能。
	语法1：#cat 文件的路径

	作用2：cat还可以对文件进行合并
	语法2：#cat 待合并的文件路径1 待合并的文件路径2 …. 文件路径n > 合并之后的文件路径
	例如，合并3个文件，并存到一个文件中【配合输出重定向使用】


## 三、进阶指令（重点）
	1、df指令
	作用：查看磁盘的空间
	语法：#df -h		-h表示以可读性较高的形式展示大小


	2、free指令
	作用：查看内存使用情况
	语法：#free -m   -m表示以mb为单位查看

	剩余的真实可以用的内存为1665mb。
	Swap：用于临时内存，当系统真实内存不够用的时候可以临时使用磁盘空间来充当内存。

	3、head指令
	作用：查看一个文件的前n行，如果不指定n，则默认显示前10行。
	语法：#head -n 文件路径   【n表示数字】

	4、tail指令
	作用1：查看一个文件的未n行，如果n不指定默认显示后10行
	语法：#tail -n 文件的路径    n同样表示数字

	作用2：可以通过tail指令来查看一个文件的动态变化内容【变化的内容不能是用户手动增加的】
	语法：#tail -f 文件路径
	该命令一般用于查看系统的日志比较多。

	5、less指令
	作用：查看文件，以较少的内容进行输出，按下辅助功能键（数字+回车、空格键+上下方向键）查看更多
	语法：#less 需要查看的文件路径

	在退出的只需要按下q键即可。

	6、wc指令
	作用：统计文件内容信息（包含行数、单词数、字节数）
	语法：#wc -lwc 需要统计的文件路径
		-l：表示lines，行数
		-w：表示words，单词数   依照空格来判断单词数量
		-c：表示bytes，字节数

	7、date指令（重点）
	作用：表示操作时间日期（读取、设置）
	语法1：#date			输出的形式：2018年 3月 24日 星期六 15:54:28
	语法2：#date  +%F	（等价于#date  “+%Y-%m-%d” ）	输出形式：2018-03-24
	语法3：#date  “+%F %T”    引号表示让“年月日与时分秒”成为一个不可分割的整体
	等价操作#date  “+%Y-%m-%d %H:%M:%S”
	输出的形式：2018-03-24 16:01:00

	语法4：获取之前或者之后的某个时间（备份）
	#date  -d  “-1 day”  “+%Y-%m-%d %H:%M:%S”

	符号的可选值：+（之后） 或者 - （之前）
	单位的可选值：day（天）、month（月份）、year（年）
	%F：表示完整的年月日
	%T：表示完整的时分秒
	%Y：表示四位年份
	%m：表示两位月份（带前导0）
	%d：表示日期（带前导0）
	%H：表示小时（带前导0）
	%M：表示分钟（带前导0）
	%S：表示秒数（带前导0）

	8、cal指令
	作用：用来操作日历的
	语法1：#cal	  等价于 #cal  -1		直接输出当前月份的日历
	语法2：#cal  -3			表示输出上一个月+本月+下个月的日历
	语法3：#cal  -y 年份  		表示输出某一个年份的日历
	语法4：#cal  -s
	语法5：#cal  -m

	9、clear/ctrl + L指令
	作用：清除终端中已经存在的命令和结果（信息）。
	语法：clear		或者快捷键：ctrl + L

	需要注意的是，该命令并不是真的清除了之前的信息，而是把之前的信息的隐藏到了最上面，通过滚动条继续查看以前的信息。

	10、管道（重要）
	管道符：|
	作用：管道一般可以用于“过滤”，“特殊”，“扩展处理”。
	语法：管道不能单独使用，必须需要配合前面所讲的一些指令来一起使用，其作用主要是辅助作用。

	①过滤案例（100%使用）：需要通过管道查询出根目录下包含“y”字母的文档名称。
	#ls / | grep y
	针对上面这个命令说明：
	①以管道作为分界线，前面的命令有个输出，后面需要先输入，然后再过滤，最后再输出，通俗的讲就是管道前面的输出就是后面指令的输入；

	②grep指令：主要用于过滤

	②特殊用法案例：通过管道的操作方法来实现less的等价效果（了解）
	之前通过less查看一个文件，可以#less 路径
	现在通过管道还可以这么写：#cat 路径|less

	③扩展处理：请使用学过的命令，来统计某个目录下的文档的总个数？
	答：#ls / | wc -l


# Linux的基本指令（2）
## 一、高级指令

	1、hostname指令
	作用：操作服务器的主机名（读取、设置）
	语法1：#hostname			含义：表示输出完整的主机名
	语法2：#hostname  -f			含义：表示输出当前主机名中的FQDN（全限定域名）

	FQDN(全限定域名): FQDN：(Fully Qualified Domain Name)全限定域名：同时带有主机名和域名的名称。（通过符号“.”）
	例如：主机名是bigserver,域名是mycompany.com,那么FQDN就是bigserver.mycompany.com。 [1] 
	全限定域名可以从逻辑上准确地表示出主机在什么地方，也可以说全域名是主机名的一种完全表示形式。
	从全限定域名中包含的信息可以看出主机在域名树中的位置。DNS解析流程：首先查找本机HOSTS表，有的直接使用表中定义，没有查找网络连接中设置的DNS 服务器由他来解析。

	例如，acmecompany公司的Web服务器的全域名可以是 acmecompany.，而若sales主机是在销售部子域，则它的全域名可以是sales.acmecompany。当给出的名字像acmecompany而不是acmecompany.时，他们通常是指主机名，而名字后边带有点号（“.”是指根域名服务器）的则认为是全域名。这种区别在理解和控制解析过程时是非常重要的。点号实际上指出了域名树的根。

	全域名在实际中是非常有用的。电子邮件就使用全域名作为收信人的电子邮件地址，如janicejones@ acmecompany. com，其中收信人为janicejones，跟在收信人名字后面是符号@，@后面是邮件服务器的全域名，或者说是邮件服务器所在企业的域名，最后是顶层域名.com。. com意味着acmecompany是一个商业机构。


	2、id指令
	作用：查看一个用户的一些基本信息（包含用户id，用户组id，附加组id…），该指令如果不指定用户则默认当前用户。
	语法1：#id		默认显示当前执行该命令的用户的基本信息
	语法2：#id  用户名		显示指定用户的基本信息 环境指的是环境变量

	验证上述信息是否正确？
	验证用户信息：通过文件/etc/passwd
	验证用户组信息：通过文件/etc/group    尽量不要去修改这两个文件 因为修改完之后你下次再用这个账户进来你就不一定进的来了


	3、whoami指令
	作用：“我是谁？”显示当前登录的用户名，一般用于shell脚本，用于获取当前操作的用户名方便记录日志。
	语法：#whoami

	4、ps -ef指令（重点）
	指令：ps	
	作用：主要是查看服务器的进程信息
	选项含义：
		-e：等价于“-A”，表示列出全部的进程
		-f：显示全部的列（显示全字段）

	列的含义：
	UID：该进程执行的用户id；执行该进程的用户id
	PID：进程id；
	PPID：该进程的父级进程id，如果一个程序的父级进程找不到，该程序的进程称之为僵尸进程（parent process ID）；
	C：Cpu的占用率，其形式是百分数；
	STIME：该进程的启动时间；
	TTY：终端设备，发起该进程的设备识别符号，如果显示“?”则表示该进程并不是由终端设备发起；
	TIME：进程的执行时间；
	CMD：该进程的名称或者对应的路径；

	案例：（100%使用的命令）在ps的结果中过滤出想要查看的进程状态
	#ps -ef|grep “进程名称”

	再例如查看火狐浏览器的进程：ps -ef | grep firefox

	5、top指令（重点）
	作用：查看服务器的进程占的资源（100%使用）
	语法：
		进入命令：#top			（动态显示）
		退出命令：按下q键

	表头含义：
	PID：进程id；
	USER：该进程对应的用户；就是哪个用户执行的这个命令
	PR：优先级；
	VIRT：虚拟内存；
	RES：常驻内存；
	SHR：共享内存；
		计算一个进程实际使用的内存 = 常驻内存（RES）- 共享内存（SHR）
	S：表示进程的状态status（sleeping，其中S表示睡眠，R表示运行）；
	%CPU：表示CPU的占用百分比；
	%MEM：表示内存的占用百分比；
	TIME+：执行的时间；
	COMMAND：进程的名称或者路径；

	在运行top的时候，可以按下方便的快捷键：
	M：表示将结果按照内存（MEM）从高到低进行降序排列；
	P：表示将结果按照CPU使用率从高到低进行降序排列；
	1：当服务器拥有多个cpu的时候可以使用“1”快捷键来切换是否展示显示各个cpu的详细信息；

	6、du -sh指令
	作用：查看目录的真实大小
	语法：#du -sh 目录路径
	选项含义：
		-s：summaries，只显示汇总的大小
		-h：表示以高可读性的形式进行显示

	案例：统计“/root/yunweihenniux”目录的实际大小

	案例：统计“/etc”目录实际大小

	7、find指令
	作用：用于查找文件（其参数有55个之多）
	语法：#find 路径范围 选项 选项的值
	选项：
		-name：按照文档名称进行搜索（支持模糊搜索）
		-type：按照文档的类型进行搜索
			文档类型：“-”表示文件（在使用find的时候需要用f来替换），“d”表示文件夹

	案例：使用find来搜索httpd.conf
	#find / -name httpd.conf

	案例：搜索etc目录下所有的conf后缀文件
	#find /etc -name *.conf

	案例：使用find来搜索/etc/sane.d/目录下所有的文件
	#find /etc/sane.d/ -type f   f表示file 文件的意思

	案例：使用find来搜索/etc/目录下所有的文件夹
	#find /etc -type d    d表示directory目录的意思

	8、service指令（重点）
	作用：用于控制一些软件的服务启动/停止/重启
	语法：#service 服务名 start/stop/restart

	例如：需要启动本机安装的Apache（网站服务器软件），其服务名httpd
	#service httpd start

	通过ps命令来检查httpd服务是否启动：

	9、kill指令（重点）
	作用：表示杀死进程		（当遇到僵尸进程或者出于某些原因需要关闭进程的时候）
	语法：#kill  进程PID		（语法需要配合ps一起使用）

	案例：需要kill掉Apache的进程

	与kill命令作用相似但是比kill更加好用的杀死进程的命令：killall
	语法：#killall 进程名称

	10、ifconfig指令（重点）
	作用：用于操作网卡相关的指令。
	简单语法：#ifconfig		（获取网卡信息）

	Eth0表示Linux中的一个网卡，eth0是其名称。Lo（loop，本地回还网卡，其ip地址一般都是127.0.0.1）也是一个网卡名称。

	注意：inet addr就是网卡的ip地址。

	11、reboot指令
	作用：重新启动计算机		
	语法1：#reboot		重启
	语法2：#reboot  -w   模拟重启，但是不重启（只写关机与开机的日志信息）

	12、shutdown指令
	作用：关机			（慎用）
	语法1：#shutdown -h now	“关机提示”  或者  #shutdown  -h 15:25  “关机提示”
	案例：设置Linux系统关机时间在12:00


	如果想要取消关机计划的话，则可以按照以下方式去尝试：
	①针对于centos7.x之前的版本：ctrl+c
	②针对于centos7.x（包含）之后的版本：#shutdown  -c

	除了shutdown关机以外，还有以下几个关机命令：
	#init 0
	#halt
	#poweroff

	13、uptime指令
	作用：输出计算机的持续在线时间（计算机从开机到现在运行的时间）
	语法：#uptime


	14、uname指令
	作用：获取计算机操作系统相关信息
	语法1：#uname			获取操作系统的类型
	语法2：#uname  -a		all，表示获取全部的系统信息（类型、全部主机名、内核版本、发布时间、开源计划）

	15、netstat -tnlp指令
	作用：查看网络连接状态
	语法：#netstat -tnlp

	选项说明：
	-t：表示只列出tcp协议的连接；
	-n：表示将地址从字母组合转化成ip地址，将协议转化成端口号来显示；
	-l：表示过滤出“state（状态）”列中其值为LISTEN（监听）的连接；
	-p：表示显示发起连接的进程pid和进程名称；


	16、man指令
	作用：manual，手册（包含了Linux中全部命令手册，英文）
	语法：#man 命令			（退出按下q键）

	案例：通过man命令查询cp指令的用法
	#man cp


	二、练习题
	1、如何通过命令行重启linux操作系统？	#reboot
	2、如何在命令行中快速删除光标前/后的内容？   前：ctrl + u   后：ctrl + k
	3、如何删除/tmp下所有A开头的文件？		#rm -f /tmp/A* 
	4、系统重要文件需要备份，如何把/etc/passwd备份到/tmp目录下？
		#cp /etc/passwd /tmp/
	5、如何查看系统最后创建的3个用户？
		#tail -3 /etc/passwd
	6、什么命令可以统计当前系统中一共有多少账户？
		#wc -l /etc/passwd        #cat /etc/passwd|wc -l
	7、如何创建/tmp/test.conf文件？
		#touch /tmp/test.conf
	8、如何通过vim编辑打开/tmp/test.conf?
		#vim /tmp/test.conf
	9、如何查看/etc/passwd的头3行和尾3行？
		#head -3 /etc/passwd
		#tail -3 /etc/passwd
	10、如何一次性创建目录/text/1/2/3/4？
		#mkdir -p /text/1/2/3/4
	11、如何最快的返回到当前账户的家目录？
		#cd ~				#cd
	12、如何查看/etc所占的磁盘空间？
		#du -sh /etc
	13、如何删除/tmp下所有的文件？
		#rm -rf /tmp/*
	14、尝试启动Apache的服务，并且检查是否启动成功。
		#service httpd start
		#ps -ef|grep httpd
	15、使用已学命令杀死Apache的进程。
		#killall httpd


## 省钱利器：
![](/hexo-private-blog-website/images/淘宝客14.jpg)
![](/hexo-private-blog-website/images/淘宝客15.jpg)
![](/hexo-private-blog-website/images/淘宝客16.jpg)

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



