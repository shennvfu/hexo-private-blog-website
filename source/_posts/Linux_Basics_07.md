---
title: Linux_Basics_07
date: 2020-08-07 16:55:35
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

谢公最小偏怜女，自嫁黔娄百事乖。
顾我无衣搜荩箧，泥他沽酒拔金钗。

[Linux命令手册][Linux]

[Linux]:https://man.linuxde.net/

[菜鸟教程][Runoob]

[Runoob]: http://www.runoob.com/

[百度][Baidu]

[Baidu]: http://www.baidu.com

[Google][Google]

[Google]: http://google.com.hk

# 编辑器之神——vim编辑器

## 一、vi介绍
	Vi编辑器是所有Unix及Linux系统下标准的编辑器，类似于windows系统下的notepad（记事本）编辑器，由于在Unix及Linux系统的任何版本，Vi编辑器是完全相同的，因此可以在其他任何介绍vi的地方都能进一步了解它，Vi也是Linux中最基本的文本编辑器，学会它后，我们将在Linux的世界里畅行无阻，尤其是在终端中。

	关于vim：
	vi和vim都是Linux中的编辑器，不同的是，vim比较高级，可以视为vi的升级版本。vi使用于文本编辑，但是vim更适用于coding（写代码的）。

	Vim重点是光标的移动，模式切换，删除，查找，替换，复制，粘贴，撤销命令的使用。

## 二、vim三种模式（重点）
	Vim中存在三种模式（大众的认知）：命令模式、编辑模式（输入模式）、末行模式（尾行模式）。

	命令模式：在该模式下是不能对文件直接编辑，可以输入快捷键进行一些操作（删除行，复制行，移动光标，粘贴等等）【打开文件之后默认进入的模式】；
	编辑模式：在该模式下可以对文件的内容进行编辑；
	末行模式：可以在末行输入命令来对文件进行操作（搜索、替换、保存、退出、撤销、高亮等等）；

	Vim的打开文件的方式（4种，要求掌握的就前三种）：
	#vim 文件路径					作用：打开指定的文件
	#vim  +数字  文件的路径			作用：打开指定的文件，并且将光标移动到指定行
	#vim  +/关键词  文件的路径		作用：打开指定的文件，并且高亮显示关键词
	#vim 文件路径1 文件路径2 文件路径3   作用：同时打开多个文件

	重点：先复制出一个/etc/passwd文件，复制当前家目录下（千万不要在etc下直接修改！！！）

	后续一切vim命令都是基于/root/passwd文件进行操作。

	退出方式：输入:q按下回车即可

## 三、命令模式
	注意：该模式是打开文件的第一个看到的模式（打开文件即可进入）
	1、光标移动
	①光标移动到行首
	按键：shift + 6 或 ^（T字母上面的6，不要按小键盘的6）

	②光标移动到行尾
	按键：shift + 4 或 $（R字母的左上角的4，不是小键盘的4）

	③光标移动到首行
	按键：gg

	④光标移动到末行
	按键：G

	⑤翻屏
	向上翻屏：按键ctrl + b   （before）	或 		PgUp
	向下翻屏：按键ctrl + f	   （after）		或		PgDn
	2、复制操作
	①复制光标所在行
	按键：yy
	粘贴：在想要粘贴的地方按下p键

	②以光标所在行为准（包含当前行），向下复制指定的行数
	按键：数字yy

	③可视化复制
	按键：ctrl + v（可视块）或V（可视行）或v（可视），然后按下↑↓←→方向键来选中需要复制的区块，按下y键进行复制，最后按下p键粘贴
	3、剪切/删除
	①剪切/删除光标所在行
	按键：dd			（删除之后下一行上移）
	注意：dd严格意义上说是剪切命令，但是如果剪切了不粘贴就是删除的效果。

	②剪切/删除光标所在行为准（包含当前行），向下删除/剪切指定的行
	按键：数字dd		（删除之后下一行上移）

	③剪切/删除光标所在的当前行之后的内容，但是删除之后下一行不上移
	按键：D				（删除之后当前行会变成空白行）

	④可视化删除
	按键：ctrl + v（可视块）或V（可视行）或v（可视），上下左右移动，按下D表示删除选中行，d表示删选中块

	4、撤销/恢复
	撤销：输入:u （不属于命令模式）  或者   u			（undo）
	恢复：ctrl + r			恢复（取消）之前的撤销操作

	5、扩展1：光标的快速移动
	①快速将光标移动到指定的行
	按键：数字G    

	②以当前光标为准向上/向下移动n行
	按键：数字↑，数字↓

	③以当前光标为准向左/向右移动n字符
	按键：数字←，数字→

	④末行模式下的快速移动方式：移动到指定的行
	按键：输入英文“:”，其后输入行数数字，按下回车


## 四、模式间的切换（重点）

![](/hexo-private-blog-website/images/Vim模式之间的切换.png)

## 五、末行模式
	进入方式：由命令模式进入，按下“:”或者“/（表示查找）”即可进入
	退出方式：
			a. 按下esc
			b. 连按2次esc键
			c. 删除末行全部输入字符

	①保存操作（write）
	输入：“:w”				保存文件
	输入：“:w  路径”		另存为  这个路径可以是相对路径也可以是绝对路径 那如果是相对，他就会在当前的工作路径下创建这个文件，那如果是别的路径那就是在别的路径下创建这个文件

	②退出（quit）
	输入：“:q”				退出文件

	③保存并退出
	输入：“:wq”				保存并且退出

	④强制 （!）
	输入：“:q!”				表示强制退出，刚才做的修改操作不做保存

	⑤调用外部命令（了解）
	输入：“:!外部命令”
	例如：! ls -la /

	当外部命令执行结束之后按下任意键回到vim编辑器打开的内容

	⑥搜索/查找
	输入：“/关键词”
	例如：我想在passwd文件中搜索“sbin”关键词

	/sbin

	在搜索结果中切换上/下一个结果：N/n		（next）
	如果需要取消高亮，则需要输入：“:nohl”【no highlight】

	⑦替换
	:s/搜索的关键词/新的内容				替换光标所在行的第一处符合条件的内容
	:s/搜索的关键词/新的内容/g			替换光标所在行的全部符合条件的内容
	:%s/搜索的关键词/新的内容			替换整个文档中每行第一个符合条件的内容
	:%s/搜索的关键词/新的内容/g			替换整个文档的符合条件的内容

	%表示整个文件
	g表示全局（global）

	⑧显示行号（临时）
	输入：“:set nu”[number]
	如果想取消显示，则输入：“:set nonu”

	⑨扩展2：使用vim同时打开多个文件，在末行模式下进行切换文件
	查看当前已经打开的文件名称：“:files”

![](/hexo-private-blog-website/images/Vim操作.png)

	在%a的位置有2种显示可能
	%a：a=active，表示当前正在打开的文件；
	#：表示上一个打开的文件

	切换文件的方式：
	a. 如果需要指定切换文件的名称，则可以输入：“:open 已经打开的文件名”

![](/hexo-private-blog-website/images/Vim操作1.png)

	b. 可以通过其他命令来切换上一个文件/下一个文件
	输入：“:bn”切换到下一个文件（back next）
	输入：“:bp”切换到上一个文件（back prev）

## 六、编辑模式

![](/hexo-private-blog-website/images/Vim编辑模式.png)

	重点看前2个进入方式：i（insert）、a（after）。
	退出方式：按下esc键

## 七、实用功能
	1、代码着色

![](/hexo-private-blog-website/images/代码着色.png)

	案例：首先创建简单的c语言程序

	如何控制着色显示与否？
	显示：“:syntax on”			syn
	tax：语法
	关闭显示：“:syntax off”

	2、vim中计算器的使用
	当在编辑文件的时候突然需要使用计算器去计算一些公式，则此时需要用计算器，但是需要退出，vim自身集成了一个简易的计算器。

	a. 进入编辑模式
	b. 按下按键“ctrl + R”，然后输入“=”，此时光标会变到最后一行
	c. 输入需要计算的内容，按下回车

## 八、扩展（3）
	1、vim的配置（重点）
	Vim是一款编辑器，编辑器也是有配置文件的。
	Vim配置有三种情况：
		a. 在文件打开的时候在末行模式下输入的配置（临时的）
		b. 个人配置文件（~/.vimrc，如果没有可以自行新建）
		c. 全局配置文件（vim自带，/etc/vimrc）

	①新建好个人配置文件之后进入编辑

	②在配置文件中进行配置
	比如显示行号：set nu

	配置好之后vim打开文件就会永远显示行号

	问题：如果某个配置项，在个人配置文件与全局配置文件产生冲突的时候应该以谁为准？
	测试步骤：在两个配置文件中针对同一个配置项设置不同的值

	①先在全局的配置中设置不显示行号，在个人的配置文件中设置显示行号，观察结果
	最后显示行号：说明以个人为准

	②先在全局中配置显示行号，在个人中设置不显示行号，观察结果
	最后的显示是不显示行号，说明以个人为准

	结论：如果针对同一个配置项，个人配置文件中存在，则以个人配置文件为准，如果个人配置文件中不存在这一项，则以全局配置文件为准。

	2、异常退出
	什么是异常退出：在编辑文件之后并没有正常的去wq（保存退出），而是遇到突然关闭终端或者断电的情况，则会显示下面的效果，这个情况称之为异常退出：

![](/hexo-private-blog-website/images/CentOS异常退出.png)

	解决办法：将交换文件（在编程过程中产生的临时文件）删除掉即可
	#rm  -f .passwd.swp

	3、别名机制（实用）
	作用：相当于创建一些属于自己的自定义命令

	例如：在windows下有cls命令，在Linux下可能因为没有这个命令而不习惯清屏。现在可以通过别名机制来解决这个问题，可以自己创造出cls命令

	别名机制依靠一个别名映射文件：~/.bashrc
	#vim  ~/.bashrc

![](/hexo-private-blog-website/images/bashrc配置文件.png)

	注意：如果想新创造的命令生效，必须要重新登录当前用户。

	4、退出方式
	回顾：之前vim中退出编辑的文件可以使用“:q”或者“:wq”。

	除了上面的这个语法之外，vim还支持另外一个保存退出方法“:x”。

	说明：
		①“:x”在文件没有修改的情况下，表示直接退出，在文件修改的情况下表示保存并退出；
		②如果文件没有被修改，但是使用wq进行退出的话，则文件的修改时间会被更新；但是如果文件没有被修改，使用x进行退出的话，则文件修改时间不会被更新的；主要是会混淆用户对文件的修改时间的认定。

	因此建议以后使用“:x”来进行对文件的保存退出。
	但是：不要使用X，不要使用X，不要使用X，X表示对文件进行加密操作。

	九、作业
	1、参考作业文件“httpd-vhosts.conf”的描述；
	2、使用别名机制，创建出一个快捷命令“kj”，要求实现按下“kj”回车之后能够实现：
		统计出Apache的服务进程数量。

# Linux自有服务（1）

	自有服务，即不需要用户独立去安装的软件的服务，而是当系统安装好之后就可以直接使用的服务（内置）。

## 一、运行模式
	运行模式也可以称之为运行级别。

	在linux中存在一个进程：init （initialize，初始化），进程id是1。
	查看进程：#ps -ef|grep init

	该进程存在一个对应的配置文件：inittab（系统运行级别配置文件，位置/etc/inittab）

	文件的主要内容：

![](/hexo-private-blog-website/images/init配置文件.png)

	根据上述的描述，可以得知，Centos6.5中存在7中运行级别/模式。
	0 — 表示关机级别（不要将默认的运行级别设置成这个值）
	1 — 单用户模式
	2 — 多用户模式，不带NFS（Network File Syetem）
	3 — 多用户模式，完全的多用户模式（不带桌面的，纯命令行模式）
	4 — 没有被使用的模式（被保留模式）
	5 — X11，完整的图形化界面模式
	6 — 表示重启级别（不要将默认的运行级别设置成这个值）

	与该级别相关的几个命令：
	#init 0		表示关机
	#init 3		表示切换到不带桌面的模式
	#init 5		切换到图形界面
	#init 6 	重启电脑
	注意：init指令需要超级管理员的权限，普通用户无法执行。

	这些命令其实都是调用的init进程，将数字（运行级别）传递给进程，进程去读配置文件执行对应的操作。

	①切换到纯命令行模式下（临时切换，重启之后又恢复）
	#init 3

![](/hexo-private-blog-website/images/init3.png)

	切换之后需要输入用户名和密码，在输入密码的时候没有“*”提示输入，只要自己确认输入的密码没有错误，按下回车即可。

	②回到桌面模式
	#init 5

	③设置模式永久为命令行模式

![](/hexo-private-blog-website/images/修改init配置文件.png)

	将/etc/inittab文件中的initdefault值设置成3，然后重启操作系统。

## 二、用户与用户组管理（重点）
	Linux系统是一个多用户多任务的操作系统，任何一个要使用系统资源的用户，都必须首先向系统管理员申请一个账号，然后以这个账号的身份进入系统。
	用户的账号一方面可以帮助系统管理员对使用系统的用户进行跟踪，并控制他们对系统资源的访问；另一方面也可以帮助用户组织文件，并为用户提供安全性保护。
	每个用户账号都拥有一个惟一的用户名和各自的密码。
	用户在登录时键入正确的用户名和密码后，就能够进入系统和自己的主目录。
	要想实现用户账号的管理，要完成的工作主要有如下几个方面：

	用户账号的添加、删除、修改以及用户密码的管理。
	用户组的管理。

	注意三个文件：
	/etc/passwd				存储用户的关键信息
	/etc/group				存储用户组的关键信息
	/etc/shadow				存储用户的密码信息

	1、用户管理
	①添加用户
	常用语法：#useradd 选项 用户名
	常用选项：
		-g：表示指定用户的用户主组，选项的值可以是用户组的id，也可以是组名
		-G：表示指定用户的用户附加组，选项的值可以是用户组的id，也可以是组名
		-u：uid，用户的id（用户的标识符），系统默认会从500之后按顺序分配uid，如果不想使用系统分配的，可以通过该选项自定义【类似于腾讯QQ的自选靓号情况】
		-c comment：添加注释
	案例：创建用户zhangsan，不带任何选项

	useradd zhangsan

	验证是否成功：
		a. 验证/etc/passwd的最后一行，查看是否有zhangsan的信息；
		b. 验证是否存在家目录（在Centos下创建好用户之后随之产生一个同名家目录）；

	扩展：认识passwd文件

![](/hexo-private-blog-website/images/passwd配置文件.png)

	用户名:密码:用户ID:用户组ID:注释:家目录:解释器shell

	用户名：创建新用户名称，后期登录的时候需要输入；
	密码：此密码位置一般情况都是“x”，表示密码的占位；
	用户ID：用户的识别符；
	用户组ID：该用户所属的主组ID；
	注释：解释该用户是做什么用的；
	家目录：用户登录进入系统之后默认的位置；
	解释器shell：等待用户进入系统之后，用户输入指令之后，该解释器会收集用户输入的指令，传递给内核处理；

	注意：在不添加选项的时候，执行useradd之后会执行一系列的操作
		a. 创建同名的家目录；
		b. 创建同名的用户组；

	案例：添加选项，创建用户lisi，让lisi属于501主组，附加组500，自选靓号666。

	useradd -g 501 -G 500 -u 666 lisi

![](/hexo-private-blog-website/images/用户添加案例.png)

	注意：查看用户的主组可以查看passwd文件，查看附加组可以查看group文件。

	②修改用户
	常用语法：#usermod 选项 用户名
	Usermod：user modify，用户修改
	常用选项：
		-g：表示指定用户的用户主组，选项的值可以是用户组的id，也可以是组名
		-G：表示指定用户的用户附加组，选项的值可以是用户组的id，也可以是组名
		-u：uid，用户的id（用户的标识符），系统默认会从500之后按顺序分配uid，如果不想使用系统分配的，可以通过该选项自定义【类似于腾讯QQ的自选靓号情况】
		-l：修改用户名

	案例：修改zhangsan用户主组为500，附加组改为501
	#usermod -g 500 -G 501 zhangsan

	案例：修改zhangsan用户用户名，改为wangerma
	#usermod -l 新的用户名 旧的用户名
	#usermod -l wangerma zhangsan


	③设置密码
	Linux不允许没有密码的用户登录到系统，因此前面创建的用户目前都处于锁定状态，需要设置密码之后才能登录计算机。

	常用语法：#passwd 用户名
	案例：设置wangerma用户的密码

![](/hexo-private-blog-website/images/修改用户案例.png)

	在设置密码的时候也是没有任何输入提示的，放心输入，确保两次输入的密码一致，按下回车即可。

	也可以使用弱密码，但是不建议，否则会看到以下的提示：

	设置密码之后shadow文件中的体现：能够看出lisi用户没有密码的。

	在设置用户密码之后可以登录帐号，例如此处需要登录wangerma
	切换用户命令：#su [用户名]	（switch user）
	如果用户名不指定则表示切换到root用户。

![](/hexo-private-blog-website/images/密码操作.png)

	切换用户需要注意的事项：
	a. 从root往普通用户切换不需要密码，但是反之则需要root密码；
	b. 切换用户之后前后的工作路径是不变的；
	c. 普通用户没有办法访问root用户家目录，但是反之则可以；

	④删除用户
	常用语法：#userdel 选项 用户名
	Userdel：user delete（用户删除）
	常用选项：

	-r：表示删除用户的同时，删除其家目录；
	案例：删除wangerma用户

![](/hexo-private-blog-website/images/删除用户案例.png)

	注意：已经登录的wangerma用户删除的时候提示删除失败，但是没有登录的lisi用户可以正常删除。

	解决办法：简单粗暴，kill对应用户的全部进程

	提示：所有跟用户操作的命令（除passwd外）只有root超级管理员有权限执行。

	2、用户组管理
	每个用户都有一个用户组，系统可以对一个用户组中的所有用户进行集中管理。不同Linux 系统对用户组的规定有所不同，如Linux下的用户属于与它同名的用户组，这个用户组在创建用户时同时创建。
	用户组的管理涉及用户组的添加、删除和修改。组的增加、删除和修改实际上就是对/etc/group文件的更新。

	文件结构：
	用户组名:密码:用户组ID:组内用户名
	密码：X表示占位符，虽然用户组可以设置密码，但是绝大部分的情况下不设置密码；
	组内用户名：表示附加组是该组的用户名称；

![](/hexo-private-blog-website/images/用户组.png)

	①用户组添加
	常用语法：#groupadd 选项 用户组名
	常用选项：
		-g：类似用户添加里的“-u”，-g表示选择自己设置一个自定义的用户组ID数字，如果自己不指定，则默认从500之后递增；

	案例：使用groupadd指令创建一个新的用户组，命名为Administrators

![](/hexo-private-blog-website/images/用户组添加.png)

	②用户组编辑
	常用语法：#groupmod 选项 用户组名
	常用选项：
		-g：类似用户修改里的“-u”，-g表示选择自己设置一个自定义的用户组ID数字
		-n：类似于用户修改“-l”，表示设置新的用户组的名称
	案例：修改Administrators用户组，将组ID从502改成520，将名称改为admins

	③用户组删除
	常用语法：#groupdel 用户组名

	注意：当如果需要删除一个组，但是这个组是某个用户的主组时，则不允许删除；如果确实需要删除，则先从组内移出所有用户。

![](/hexo-private-blog-website/images/用户组删除.png)

## 三、网络设置
	首先知道网卡配置文件位置：/etc/sysconfig/network-scripts

![](/hexo-private-blog-website/images/网络设置.png)

	在目录中网卡的配置文件命名格式：ifcfg-网卡名称

	ONBOOT：是否开机启动
	BOOTPROTO：ip地址分配方式，DHCP表示动态主机分配协议
	HWADDR：硬件地址，MAC地址

	如果后续需要重启网卡怎么去操作呢？
	#service network restart

![](/hexo-private-blog-website/images/网络操作.png)

	在有的分支版本中可能没有service命令来快速操作服务，但是有一个共性的目录：/etc/init.d
	这个目录中放着很对服务的快捷方式。
	此处重启网卡命令还可以使用：
	#/etc/init.d/network restart

![](/hexo-private-blog-website/images/网络操作1.png)

	扩展1：如果修改网卡的配置文件，但是配置文件的目录层次很深，此时可以在浅的目录中创建一个快捷方式（软连接），方便以后去查找
	#ln -s 原始文件的路径 快捷方式的路径

	通过ls -l可以列出如下的效果：

![](/hexo-private-blog-website/images/网络操作2.png)

	其中，文件类型位置的“l”表示其类型为link（连接类型），后面的“->”指向的是原始文件路径。

	扩展2：如何去重启单个网卡？
	停止某个网卡：#ifdown 网卡名
	开启某个网卡：#ifup 网卡名
	例如：需要停止-启动（重启）eth0网卡，则可以输入
	#ifdown eth0
	#ifup eth0

	提示：在实际工作的时候不要随意禁网卡。

## 四、ssh服务（重点）
	ssh（secure shell，安全外壳协议），该协议有2个常用的作用：远程连接协议、远程文件传输协议。

	协议使用端口号：默认是22
	可以是被修改的，如果需要修改，则需要修改ssh服务的配置文件：
	#/etc/ssh/ssh_config

![](/hexo-private-blog-website/images/ssh服务.png)

	端口号可以修改，但是得注意2个事项：
		a. 注意范围，端口范围是从0-65535；
		b. 不能使用别的服务已经占用的端口；

	服务启动/停止/重启
	#service sshd start/stop/restart
	#/etc/init.d/sshd start/stop/restart

	1、远程终端
	终端工具主要帮助运维人员连接远程的服务器，常见终端工具有：Xshell、secureCRT、Putty等。以putty为例：

	①获取服务器ip地址，可以通过ifconfig命令进行查看，然后顺手测试ip的连接相通性

![](/hexo-private-blog-website/images/远程终端.png)

	②打开putty，输入相关的信息

![](/hexo-private-blog-website/images/远程终端1.png)

	③在弹出key确认的时候点击“是”，以后不会再提示

![](/hexo-private-blog-website/images/远程终端2.png)

	④输入登录信息

![](/hexo-private-blog-website/images/远程终端3.png)

	2、SSH服务文件传输
	可视化的界面传输工具：Filezilla
	安装好之后可以查看到桌面图标：

	①选择“文件”- “站点管理器（Ctrl + S）”

![](/hexo-private-blog-website/images/ssh服务文件传输.png)


![](/hexo-private-blog-website/images/ssh服务文件传输1.png)

	②点击“文件”菜单下方的“▽”选择需要连接的服务器，连接好之后的效果

	③从本地windows上传文件到linux中方式
	支持直接拖拽文件，也可以右键本地需要上传的文件，然后点选“上传”即可

	④下载linux文件到本地
	支持服务器文件直接拖拽到本地，也可以在右侧窗口选择需要下载的文件，右键，点选“下载”。

![](/hexo-private-blog-website/images/ssh服务文件传输2.png)

	扩展3：通过命令行工具来传输文件/文件夹
	工具：PSCP.exe（必须通过cmd命令行打开），为了使用方便可以将其放到环境变量目录中
	如果不清楚哪些路径是环境变量路径，只需要将其放到C:/Windows目录下即可。

![](/hexo-private-blog-website/images/ssh服务文件传输3.png)

	用法：
	a. pscp 选项 用户名@linux主机地址:资源路径 windows本地的地址 （下载到win）
	b. pscp 选项 资源路径 用户名@linux主机地址:远程路径	（上传到linux）
	c. pscp 选项 -ls 用户名@linux主机地址 （列出远程路径下结构）

	①下载到本地windows
	要求将远程linux服务器下的/etc整个目录下载到本地E:\tmp下
	#pscp -r root@192.168.21.128:/etc E:\tmp

	在CMD中输入之后输入密码

![](/hexo-private-blog-website/images/ssh服务文件传输4.png)

	②上传文件到linux
	将“E:\coursedocs\运维学科\北京运维01期\01-基础班\20180329_Linux自有服务”所有的内容传输到linux下root用户的家目录
	#pscp -r “E:\coursedocs\运维学科\北京运维01期\01-基础班\20180329_Linux自有服务” root@192.168.21.128:/root

![](/hexo-private-blog-website/images/ssh服务文件传输5.png)

	五、作业
	1、能够分别使用Filezilla和PSCP工具传输给定文件到“/usr/src/data”目录下，如目录不存在则自行创建。


# Linux自有服务（2）
	自有服务，即不需要用户独立去安装的软件的服务，而是当系统安装好之后就可以直接使用的服务（内置）。

## 一、设置主机名
	回顾：
	#hostname
	#hostname -f		FQDN（全限定域名）

	①临时设置主机名（立竿见影），需要切换用户使之生效
	#hostname 设置的主机名

![](/hexo-private-blog-website/images/设置主机名.png)

	②永久设置主机名（需要重启）
	先找到一个文件
	/etc/sysconfig/network		【主机名的配置文件】

	修改其中的HOSTNAME为自己需要设置的永久主机名

	③修改linux服务器的hosts文件，将yunwei指向本地（设置FQDN）
	Hosts文件的位置：/etc/hosts

![](/hexo-private-blog-website/images/设置主机名1.png)

	问题：不设置FQDN会怎么样？
		①很多开源服务器软件（例如Apache）则无法启动，或出现报错；
		②方便记忆，看到主机名对其作用有一个初步判断；
		③如果不设置则会影响本地的域名的解析（本地访问）；

## 二、chkconfig
	作用：相当于windows下“安全卫士”、“电脑管家”之类的安全辅助工具提供“开机启动项”的一个管理服务。

	在linux下不是所有的软件安装完成之后都有开机启动服务，有的可能需要自己去添加。除此之外还可以查看和删除。

	①开机启动服务查询
	#chkconfig --list

![](/hexo-private-blog-website/images/chkconfig.png)

	其中0-6表示各个启动级别
	例如：以httpd为例，其3级别为关闭（off），则表示其在3启动形式下默认开机不启动
	5对应的也是关闭，则表示其在桌面环境下也是开机不启动。

	再例如：kdump服务，在2，3，4，5的级别下默认开机启动的，其他级别下默认开机不启动

	②删除服务
	#chkconfig --del 服务名
	例如删除httpd服务

	③添加开机启动服务
	#chkconfig --add 服务名			【必须要保证服务正常运行，才可以添加】

![](/hexo-private-blog-website/images/chkconfig1.png)

	④设置服务在某个级别下开机启动/不启动【重点命令】
	#chkconfig --level 连在一起的启动级别 服务名on/off

	案例：设置httpd服务在3，5级别下默认开机启动

![](/hexo-private-blog-website/images/chkconfig2.png)

	案例：设置httpd服务在5的级别下默认开机不启动

## 三、ntp服务
	作用：ntp主要是用于对计算机的时间同步管理操作。

	时间是对服务器来说是很重要的，一般很多网站都需要读取服务器时间来记录相关信息，如果时间不准，则可能造成很大的影响。

	例如：当前虚拟机里的linux时间就是不准确的

![](/hexo-private-blog-website/images/ntp服务.png)

	同时服务器时间方式有2个：一次性同步（手动同步）、通过服务自动同步。


	上游的概念：

![](/hexo-private-blog-website/images/ntp服务.png)

	①一次性同步时间（简单）
	#ntpdate 时间服务器的域名或ip地址
	Ip地址查看可以访问：http://www.ntp.org.cn/pool.php

![](/hexo-private-blog-website/images/ntp服务1.png)

	②设置时间同步服务
	服务名：ntpd
	启动ntpd服务
		#service ntpd start    或者   /etc/init.d/ntpd start

![](/hexo-private-blog-website/images/ntp服务1.png)

	设置ntpd服务开机启动：
	# chkconfig --list|grep ntpd
	# chkconfig --level 35 ntpd on

![](/hexo-private-blog-website/images/ntp服务1.png)

## 四、防火墙服务
	防火墙：防范一些网络攻击。有软件防火墙、硬件防火墙之分。

	防火墙选择让请求通过，从而保证网络安全性。

	在当前的centos6.5中防火墙有一个名称：iptables 【7.x中默认使用的是firewalld】

	①查看iptables是否开机启动

![](/hexo-private-blog-website/images/iptables.png)

	②iptables服务启动/重启/关闭
	#service iptables start/restart/stop
	/etc/init.d/iptables start /restart/stop

	③查看iptables的状态（规则）
	# service iptables status

	如果iptables没有启动，则提示服务没启动，如果已经启动，则显示防火墙的相关的规则信息


![](/hexo-private-blog-website/images/iptables1.png)

	④查看规则的命令
	#iptables -L -n
	含义：
		-L：表示列出规则
		-n：表示将单词表达形式改成数字形式显示

	⑤简单设置防火墙规则
	例如，需要允许80端口通过防火墙，则规则可以用以下的命令来设置
	#iptables -I INPUT -p tcp --dport 80 -j ACCEPT    #允许访问80端口
	Iptables：主命令
	-I：表示将规则放到最前面
	-A：add，添加规则（最后）
	INPUT：进站请求【出站output】
	-p：protocol，指定协议（icmp/tcp/udp）
	--dport：指定端口号
	-j：指定行为结果，允许（accept）/禁止（reject）/丢弃（drop）

![](/hexo-private-blog-website/images/iptables2.png)

	添加完成之后需要保存操作：
	/etc/init.d/iptables save


	测试80端口访问：

## 五、rpm管理（重点）
	作用：rpm的作用类似于windows上的电脑管家中“软件管理”、安全卫士里面“软件管家”等产品，主要作用是对linux服务器上的软件包进行对应管理操作，管理分为：查询、卸载、安装。

	①查询某个软件的安装情况
	#rpm -qa|grep 关键词
	选项：
		-q：查询，query
		-a：全部，all

	案例：查询linux上是否安装firefox

![](/hexo-private-blog-website/images/rpm管理.png)

	案例：查询是否安装qq

	②卸载某个软件
	#rpm -e 软件的名称

![](/hexo-private-blog-website/images/rpm管理1.png)

	火狐卸载的时候是没有依赖关系的，所以可以直接卸载。

	但是在卸载Apache的时候提示无法卸载：

![](/hexo-private-blog-website/images/rpm管理2.png)

	当存在依赖关系的时候又不想去解决这个问题的时候可以：
	#rpm -e 软件包名 --nodeps

	③软件的安装
	要想装软件，和windows下一样，先得找到安装包。
		软件包的获得方式：
			a. 去官网去下载；
			b. 不介意老版本的话，可以从光盘（或者镜像文件）中读取；
	此处以光盘文件为例：
	查看块状设备的信息：
	#lsblk   （list block devices）		查看块状设备的信息

![](/hexo-private-blog-website/images/光盘挂载.png)

	Name：名称
	Size：设备大小
	Type：类型
	MountPoint：挂载点（类似windows下盘符）

	扩展：光盘的挂载和解挂
	a. 解挂操作
		命令：umount
		语法：#umount 当前设备的挂载点（路径）

![](/hexo-private-blog-website/images/光盘挂载.png)

	此时，相当于U盘在windows上已经被弹出了，但是没有拔下电脑USB接口。


	b. 挂载光盘
	命令：mount
	语法：#mount 设备原始地址 要挂载的位置路径
	设备原始地址：地址统一都在/dev下，然后根据大小确定具体name值，拼凑在一起组成原始地址，例如当前：“/dev/sr0”
	要挂载的位置路径：挂载目录一般都在mnt下，也可以在mnt下建目录，此处以“/mnt/dvd”为例

![](/hexo-private-blog-website/images/光盘挂载1.png)

	安装软件的命令：
	#rpm -ivh 软件包完整名称
	选项：
		-i：install，安装
		-v：显示进度条
		-h：表示以“#”形式显示进度条

![](/hexo-private-blog-website/images/光盘挂载2.png)

## 六、cron/crontab计划任务（重点）
	作用：操作系统不可能24小时都有人在操作，有些时候想在指定的时间点去执行任务（例如：每天夜里2点去重新启动Apache），此时不可能真有人每天夜里2点去执行命令，此时可以交给计划任务程序去执行操作。

	语法：#crontab 选项
		常用选项：
			-l：list，列出指定用户的计划任务列表
			-e：edit，编辑指定用户的计划任务列表
			-u：user，指定的用户名，如果不指定，则表示当前用户
			-r：remove，删除指定用户的计划任务列表

	①列出

![](/hexo-private-blog-website/images/cron计划任务.png)

	②编辑计划任务（重点）
	计划任务的规则语法格式，以行为单位，一行则为一个计划：
	分 时 日 月 周 需要执行的命令
	例如：如果想要每天的0点0分执行reboot指令，则可以写成
	0 0 * * * reboot

	取值范围：
	分：0~59
	时：0~23
	日：1~31
	月：1~12
	周：0~7，0和7表示星期天

![](/hexo-private-blog-website/images/cron计划任务1.png)

	四个符号：
	*：表示取值范围中的每一个数字
	-：做连续区间表达式的，要想表示1~7，则可以写成：1-7
	/：表示每多少个，例如：想每10分钟一次，则可以在分的位置写：*/10
	,：表示多个取值，比如想在1点，2点6点执行，则可以在时的位置写：1,2,6

	问题1：每月1、10、22日的4:45重启network服务
	45  4  1,10,22  *  *  service network restart

	问题2：每周六、周日的1:10重启network服务
	10  1  *  *  6,0   service network restart

	问题3：每天18:00至23:00之间每隔30分钟重启network服务
	*/30  18-23  *  *  *   service network restart

	问题4：每隔两天的上午8点到11点的第3和第15分钟执行一次重启
	3,15  8-11  */2  *  *   reboot


	案例：真实测试案例，每1分钟往root家目录中的RT.txt中输入当前的时间信息，为了看到效果使用追加输出
	计划任务：*/1  *   *   *   *  ls ~>> /root/RT.txt


	Crontab权限问题：本身是任何用户都可以创建自己的计划任务。

![](/hexo-private-blog-website/images/cron计划任务2.png)

	但是超级管理员可以通过配置来设置某些用户不允许设置计划任务 ：
	配置文件位于（黑名单）：
		/etc/cron.deny			里面写用户名，一行一个

![](/hexo-private-blog-website/images/cron计划任务3.png)

	还有一个配置文件：（白名单）
		/etc/cron.allow		（本身不存在，自己创建）


	注意：白名单优先级高于黑名单，如果一个用户同时存在两个名单文件中，则会被默认允许创建计划任务。


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
![wechat.jpg](http://ww1.sinaimg.cn/large/006DnxC4gy1gfggvv58jjj30u010sgnq.jpg)
### 财付通打赏：
![qq.jpg](http://ww1.sinaimg.cn/large/006DnxC4gy1gfggwd0rvjj32ai2lxdrm.jpg)


