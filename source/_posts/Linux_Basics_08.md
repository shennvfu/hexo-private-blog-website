---
title: Linux_Basics_08
date: 2020-08-08 17:52:45
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

尚想旧情怜婢仆，也曾因梦送钱财。
诚知此恨人人有，贫贱夫妻百事哀。

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

# Linux的权限管理操作
	Linux的权限操作与用户、用户组是兄弟操作。

## 一、权限概述
	总述：Linux系统一般将文件可存/取访问的身份分为3个类别：owner、group、others，且3种身份各有read、write、execute等权限。
	1、权限介绍
	什么是权限？
	在多用户（可以不同时）计算机系统的管理中，权限是指某个特定的用户具有特定的系统资源使用权力，像是文件夹、特定系统指令的使用或存储量的限制。

	在Linux中分别有读、写、执行权限：
	读权限：
		对于文件夹来说，读权限影响用户是否能够列出目录结构
		对于文件来说，读权限影响用户是否可以查看文件内容

	写权限：
		对文件夹来说，写权限影响用户是否可以在文件夹下“创建/删除/复制到/移动到”文档
		对于文件来说，写权限影响用户是否可以编辑文件内容

	执行权限：
		一般都是对于文件来说，特别脚本文件。

	2、身份介绍
	Owner身份（文件所有者，默认为文档的创建者）
	由于Linux是多用户、多任务的操作系统，因此可能常常有多人同时在某台主机上工作，但每个人均可在主机上设置文件的权限，让其成为个人的“私密文件”，即个人所有者。因为设置了适当的文件权限，除本人（文件所有者）之外的用户无法查看文件内容。

	例如某个MM给你发了一封Email情书，你将情书转为文件之后存档在自己的主文件夹中。为了不让别人看到情书的内容，你就能利用所有者的身份去设置文件的适当权限，这样，即使你的情敌想偷看你的情书内容也是做不到的。

	Group身份（与文件所有者同组的用户）
	与文件所有者同组最有用的功能就体现在多个团队在同一台主机上开发资源的时候。例如主机上有A、B两个团体，A中有a1,a2,a3三个成员，B中有b1,b2两个成员，这两个团体要共同完成一份报告F。由于设置了适当的权限，A、B团体中的成员都能互相修改对方的数据，但是团体C的成员则不能修改F的内容，甚至连查看的权限都没有。同时，团体的成员也能设置自己的私密文件，让团队的其它成员也读取不了文件数据。在Linux中，每个账户支持多个用户组。如用户a1、b1即可属于A用户组，也能属于B用户组【主组和附加组】。

	Others身份（其他人，相对于所有者）
	这个是个相对概念。打个比方，大明、二明、小明一家三兄弟住在一间房，房产证上的登记者是大明（owner所有者），那么，大明一家就是一个用户组，这个组有大明、二明、小明三个成员；另外有个人叫张三，和他们三没有关系，那么这个张三就是其他人了。
	同时，大明、二明、小明有各自的房间，三者虽然能自由进出各自的房间，但是小明不能让大明看到自己的情书、日记等，这就是文件所有者（用户）的意义。

	Root用户（超级用户）
	在Linux中，还有一个神一样存在的用户，这就是root用户，因为在所有用户中它拥有最大的权限 ，所以管理着普通用户。

	3、Linux的权限介绍
	要设置权限，就需要知道文件的一些基本属性和权限的分配规则。在Linux中，ls命令常用来查看文件的属性，用于显示文件的文件名和相关属性。
	#ls -l 路径		【ls -l  等价于 ll】

![](/hexo-private-blog-website/images/权限操作.png)

	标红的部分就是Linux的文档权限属性信息。

	inux中存在用户、用户组和其他人概念，各自有不同的权限，对于一个文档来说，其权限具体分配如下：

![](/hexo-private-blog-website/images/权限操作1.png)

	十位字符表示含义：
	第1位：表示文档类型，取值常见的有“d表示文件夹”、“-表示文件”、“l表示软连接”、“s表示套接字”等等；
	第2-4位：表示文档所有者的权限情况，第2位表示读权限的情况，取值有r、-；第3位表示写权限的情况，w表示可写，-表示不可写，第4位表示执行权限的情况，取值有x、-。
	第5-7位：表示与所有者同在一个组的用户的权限情况，第5位表示读权限的情况，取值有r、-；第6位表示写权限的情况，w表示可写，-表示不可写，第7位表示执行权限的情况，取值有x、-。
	第8-10位：表示除了上面的前2部分的用户之外的其他用户的权限情况，第8位表示读权限的情况，取值有r、-；第9位表示写权限的情况，w表示可写，-表示不可写，第10位表示执行权限的情况，取值有x、-。

	权限分配中,均是rwx的三个参数组合，且位置顺序不会变化。没有对应权限就用 – 代替。

	例如：以下一个文档权限是怎么样的？

![](/hexo-private-blog-website/images/权限操作2.png)

	a. 其是文件夹类型
	b. 所有者：拥有全部权限（读写执行）
	c. 同组用户：可读、可执行
	d. 其他用户：可读、可执行

## 二、权限设置
	语法：#chmod 选项 权限模式 文档
	注意事项：
		常用选项：
				-R：递归设置权限	（当文档类型为文件夹的时候）
		权限模式：就是该文档需要设置的权限信息
		文档：可以是文件，也可以是文件夹，可以是相对路径也可以是绝对路径。
	注意点：如果想要给文档设置权限，操作者要么是root用户，要么就是文档的所有者。

![](/hexo-private-blog-website/images/权限操作3.png)

	给谁设置：
		u：表示所有者身份owner（user）
		g：表示给所有者同组用户设置（group）
		o：表示others，给其他用户设置权限
		a：表示all，给所有人（包含ugo部分）设置权限
			如果在设置权限的时候不指定给谁设置，则默认给所有用户设置

	权限字符：
		r：读
		w：写
		x：表示执行
		-：表示没有权限

	权限分配方式：
		+：表示给具体的用户新增权限（相对当前）
		-：表示删除用户的权限（相对当前）
		=：表示将权限设置成具体的值（注重结果）【赋值】

	例如：需要给anaconda-ks.cfg文件（-rw-------.）设置权限，要求所有者拥有全部的权限，同组用户拥有读和执行权限，其他用户只读权限。

![](/hexo-private-blog-website/images/权限操作4.png)

	答案：
	①#chmod  u+x,g+rx,o+r  anaconda-ks.cfg

	②#chmod  u=rwx,g=rx,o=r  anaconda-ks.cfg

![](/hexo-private-blog-website/images/权限操作5.png)

	提示：当文档拥有执行权限（任意部分），则其颜色在终端中是绿色。

	#chmod ug=rwx  形式，如果有两部分权限一样则可以合在一起写的

	例如：如果anaconda-ks.cfg文件什么权限都没有，可以使用root用户设置所有人都有执行权限，则可以写成
		①#chmod  +x  anaconda-ks.cfg
		②#chmod  a=x  anaconda-ks.cfg
		③#chmod  a+x  anaconda-ks.cfg

	2、数字形式
	经常会在一些技术性的网页上看到类似于#chmod  777  a.txt  这样的一个权限，这种形式称之为数字形式权限（777）。

	读：r        4
	写：w		 2
	执行：x		 1
	没有任何权限：0

![](/hexo-private-blog-website/images/权限操作6.png)

	例如：需要给anaconda-ks.cfg设置权限，权限要求所有者拥有全部权限，同组用户拥有读执行权限，其他用户只读。
	全部权限（u）：读+写+执行=4+2+1=7
	读和执行（g）：读+执行=4+1=5
	读权限（o）：读=4
	由上得知权限为：754
	#chmod 754 anaconda-ks.cfg

![](/hexo-private-blog-website/images/权限操作7.png)

	面试题：用超级管理员设置文档的权限命令是#chmod -R 731 aaa，请问这个命令有没有什么不合理的地方？
	拥有者：7=4+2+1=读+写+执行
	同组用户：3=2+1=写+执行
	其他用户：1=1=执行

	注意：在写权限的时候千万不要设置类似于上面的这种“奇葩权限”。如果一个权限数字中但凡出现2与3的数字，则该权限有不合理的情况。

	3、注意事项
	使用root用户创建一个文件夹（/oo），权限默认，权限如下：

![](/hexo-private-blog-website/images/权限操作8.png)

	需要在oo目录下创建文件（oo/xx.txt），需要给777权限：

![](/hexo-private-blog-website/images/权限操作9.png)

	切换到test用户（不是文档所有者，也不是同组用户，属于other部分）：

	问题1：test用户是否可以打开oo/xx.txt文件？【能打开】
	问题2：test用户是否可以编辑oo/xx.txt文件？【可以】
	问题3：test用户是否可以删除oo/xx.txt文件？【不可以，同样还不允许创建文件/文件夹、移动文件、重命名文件】

![](/hexo-private-blog-website/images/权限操作9.png)

	在Linux中，如果要删除一个文件，不是看文件有没有对应的权限，而是看文件所在的目录是否有写权限，如果有才可以删除。
	三、属主与属组设置
	属主：所属的用户（文件的主人）
	属组：所属的用户组

	前面的那个root就是属主
	后面的那个root就是属组

![](/hexo-private-blog-website/images/权限操作10.png)

	这两项信息在文档创建的时候会使用创建者的信息（用户名、用户所属的主组名称）。

	如果有时候去删除某个用户，则该用户对应的文档的属主和属组信息就需要去修改。

	1、chown（重点）
	作用：更改文档的所属用户
	语法：#chown  -R  username 文档路径

	案例：将刚才root用户创建的oo目录，所有者更改为test
	#chown  test  oo/

![](/hexo-private-blog-website/images/权限操作11.png)

	2、chgrp（了解）
	作用：更改文档的所属用户组
	语法：#chgrp  -R  groupname  文档的路径
	案例：将刚才root用户创建的oo目录，所有者更改为test，并且将所属用户组也改为test
	#chgrp  test  oo/

![](/hexo-private-blog-website/images/权限操作12.png)

	思考，如何通过一个命令实现既可以更改所属的用户，也可以修改所属的用户组呢？
	答：可以实现的，通过chown命令
		语法：#chown  -R  username:groupname   文档路径

	案例：要求只使用chown指令，将oo目录的所属用户和用户组改回成root，并且包含其子目录

![](/hexo-private-blog-website/images/权限操作12.png)

## 四、扩展（1）
	问题：reboot、shutdown、init、halt、user管理，在普通用户身份上都是操作不了，但是有些特殊的情况下又需要有执行权限。又不可能让root用户把自己的密码告诉普通用户，这个问题该怎么解决？

	该问题是可以被解决的，可以使用sudo（switch user do）命令来进行权限设置。Sudo可以让管理员（root）事先定义某些特殊命令谁可以执行。

	默认sudo中是没有除root之外用户的规则，要想使用则先配置sudo。

	Sudo配置文件：/etc/sudoers

![](/hexo-private-blog-website/images/权限操作13.png)

	a. 配置sudo文件请使用“#visudo”，打开之后其使用方法和vim一致
   
	b. 配置普通用户的权限

![](/hexo-private-blog-website/images/权限操作13.png)

	Root表示用户名，如果是用户组，则可以写成“%组名”
	ALL：表示允许登录的主机（地址白名单）

	(ALL)：表示以谁的身份执行，ALL表示root身份
	ALL：表示当前用户可以执行的命令，多个命令可以使用“,”分割

![](/hexo-private-blog-website/images/权限操作14.png)

	案例：本身test用户不能添加用户，要求使用sudo配置，将其设置为可以添加用户，并且可以修改密码（但是不能修改root用户密码）。
	注意：在写sudo规则的时候不建议写直接形式的命令，而是写命令的完整路径。
	路径可以使用which命令来查看
	语法：#which 指令名称

	在添加好对应的规则之后就可以切换用户，切换到普通用户test，再去执行：

![](/hexo-private-blog-website/images/权限操作14.png)

	此时要想使用刚才的规则，则以以下命令进行：
	#sudo 需要执行的指令

![](/hexo-private-blog-website/images/权限操作15.png)

	在输入sudo指令之后需要输入当前的用户密码进行确认的操作（不是root用户密码），输入之后在接下来5分钟内再次执行sudo指令不需要密码。

	特别注意：此处按照案例要求，不能让test用户修改root密码，因此规则还需要调整，不然其可以修改root密码的：

![](/hexo-private-blog-website/images/权限操作16.png)

	禁止修改root密码的配置（先允许全部，再拒绝root密码设置）： /usr/bin/passwd [A-Za-z]*, !/usr/bin/passwd root

	补充：在普通用户下怎么查看自己具有哪些特殊权限呢？
	#sudo  -l

	最后：sudo不是任何Linux分支都有的命令，常见centos与ubuntu都存在sudo命令。

	作业：给普通用户设置一个关机命令执行权限。

# Linux的网络基础
## 一、网络相关概述
	1、网络发展
	信息传递
	远古时期，人们就通过简单的语言、壁画等方式交换信息
	千百年来，人们一直在用语言、图符、钟鼓、烟火、竹简、纸书等传递信息
	古代人的烽火狼烟、飞鸽传信、驿马邮递
	现代社会中，交通警的指挥手语、航海中的旗语等
	这些信息传递的基本方式都是依靠人的视觉与听觉

	电的产生
	1831年，法拉第制出了世界上最早的第一台发电机
	1866年，德国人西门子（Siemens）制成世界上第一台大功率发电机
	1837年，美国人塞缪乐·莫乐斯成功地研制出世界上第一台电磁式电报机
	1844年5月24日，莫乐斯在国会大厦联邦最高法院会议厅进行了“用莫尔斯电码”发出了人类历史上的第一份电报，从而实现了长途电报通信

	网络诞生
	1957年，前苏联发射了第一颗人造卫星，震惊了美国
	1958年美国成立了国防部高级研究计划署（ARPA，Advanced Research Projects Agency），应对冷战形势，ARPA是一个管理机构，没有实验室和科学家


![](/hexo-private-blog-website/images/网络的诞生.png)

	1969年，ARPANET（阿帕网）开始联机，因此1969年被称为Internet元年

	网络分类（记忆）
	局域网（Local Area Network，LAN）是指范围在几百米到十几公里内办公楼群或校园内的计算机相互连接所构成的计算机网络。
	城域网（Metropolitan Area Network，MAN）所采用的技术基本上与局域网相类似，只是规模上要大一些。城域网既可以覆盖相距不远的几栋办公楼，也可以覆盖一个城。
	广域网（Wide Area Network，WAN）通常跨接很大的物理范围，如一个国家。

	除了上述的划分，网络还可以按照所有者分为公网、私网是两种Internet的接入方式。公网接入方式：上网的计算机得到的IP地址是Internet上的非保留地址，公网的计算机和Internet上的其他计算机可随意互相访问。私网则反之。


	2、ip地址（重点记忆）
	IP是英文Internet Protocol的缩写，意思是“网络之间互连的协议”，也就是为计算机网络相互连接进行通信而设计的协议。

	IP地址类型分为：公有地址、私有地址。
	公有地址
	公有地址（Public address）由Inter NIC（Internet Network Information Center因特网信息中心）负责。这些IP地址分配给注册并向Inter NIC提出申请的组织机构。通过它直接访问因特网。

	私有地址（重点）
	私有地址（Private address）属于非注册地址，专门为组织机构内部使用。以下列出留用的内部私有地址： 
	A类 10.0.0.0--10.255.255.255
	B类 172.16.0.0--172.31.255.255
	C类 192.168.0.0--192.168.255.255

	IP地址按类型可以分为三类：

![](/hexo-private-blog-website/images/ip地址.png)

	网络运维相关技能：ip分类、子网划分、划分vlan、ACL、综合布线、各种Serve的搭建。

	127.0.0.1			本机ip

	3、网卡

![](/hexo-private-blog-website/images/网卡.png)

	网卡是一个网络组件，属于硬件范畴，主要负责计算机之间数据的封装和解封。

	MAC地址：网卡的物理地址，网卡设备的编号，默认情况是全球唯一的（16进制）。

![](/hexo-private-blog-website/images/MAC地址.png)

	与IP地址的区别：
		长度不同。IP地址为32位，MAC地址为48位。
		分配依据不同。
		网络寻址方式不同。OSI参考模型，ip地址是基于第三层工作（网络层），mac地址是第二层（数据链路层）

	4、网线
	网线是连接局域网必不可少的。在局域网中常见的网线主要有双绞线（RJ45接口）、铜轴电缆、光缆三种。

![](/hexo-private-blog-website/images/网线.png)

	5、交换机
	交换机（Switch）意为“开关”，是一种用于电（光）信号转发的网络设备，交换机它可以为接入交换机的任意两个网络节点提供独享的电信号通路。

![](/hexo-private-blog-website/images/网线.png)

	目前，交换机品牌比较有名的是：华为、华三（h3c）、思科、锐捷。

	6、路由器
	路由器（Router）又称网关设备（Gateway）是用于连接多个逻辑上分开、相对独立的网络。

![](/hexo-private-blog-website/images/路由器.png)

	7、拓扑结构图（扩展）
	所谓“拓扑”就是把实体抽象成与其大小、形状无关的“点”，而把连接实体的线路抽象成“线”，进而以图的形式来表示这些点与线之间关系的方法，其目的在于研究这些点、线之间的相连关系。表示点和线之间关系的图被称为拓扑结构图。
	常见的几种拓扑结构图：

![](/hexo-private-blog-website/images/拓扑结构图.png)

![](/hexo-private-blog-website/images/拓扑结构图1.png)

## 二、网络相关命令
	1、ping
	作用：检测当前主机与目标主机之间的连通性（不是100%准确，有的服务器是禁ping）
	语法：#ping 主机地址（ip地址、主机名、域名等）

	例如：测试和baidu.com之间的连通性。

![](/hexo-private-blog-website/images/ping.png)

	该命令可以跨平台，windows下也可以使用，语法一致。（区别在于Linux下默认一直发送，windows下默认发送4个数据包）

![](/hexo-private-blog-website/images/ping.png)

	2、netstat
	作用：表示查看网络的连接信息
	语法：#netstat  -tnlp		（-t：tcp协议，-n：将字母转化成数字，-l：列出状态为监听，-p：显示进程相关信息）
		  #netstat  -an		（-a：表示全部，-n：将字母转化为数字）

	TCP/IP协议需要使用这个命令。
	3、traceroute
	作用：查找当前主机与目标主机之间所有的网关（路由器，会给沿途各个路由器发送icmp数据包，路由器可能会不给响应）。
	该命令不是内置命令，需要安装，但是目前的已经安装好了（之前选了开发工具）。

![](/hexo-private-blog-website/images/netstat.png)

	语法：#traceroute  主机地址

![](/hexo-private-blog-website/images/traceroute.png)

	类似于查看快递的跟踪路由：

![](/hexo-private-blog-website/images/traceroute.png)

	扩展：在windows下也有类似的命令：tracert  主机地址

![](/hexo-private-blog-website/images/traceroute1.png)

	在线工具网址：http://tool.chinaz.com
[在线工具网址][http://tool.chinaz.com]

[http://tool.chinaz.com]:http://tool.chinaz.com

	4、arp
	地址解析协议，即ARP（Address Resolution Protocol），是根据IP地址获取（MAC）物理地址的协议。

![](/hexo-private-blog-website/images/arp.png)

![](/hexo-private-blog-website/images/arp1.png)

	当一个主机发送数据时，首先查看本机MAC地址缓存中有没有目标主机的MAC地址， 如果有就使用缓存中的结果；如果没有，ARP协议就会发出一个广播包，该广播包要求查询目标主机IP地址对应的MAC地址，拥有该IP地址的主机会发出回应，回应中包括了目标主机的MAC地址，这样发送方就得到了目标主机的MAC地址。如果目标主机不在本地子网中，则ARP解析到的MAC地址是默认网关的MAC地址。

![](/hexo-private-blog-website/images/arp1.png)

	常用语法：#arp -a		查看本地缓存mac表
			  #arp -d 主机地址			删除指定的缓存记录

![](/hexo-private-blog-website/images/arp2.png)

	该命令在windows下同样适用。

	5、tcpdump(了解)
	作用：抓包，抓取数据表
	常用语法：
		#tcpdump 协议 port 端口
		#tcpdump 协议 port 端口 host 地址
		#tcpdump -i 网卡设备名

	查看22端口（ssh）的数据包：

![](/hexo-private-blog-website/images/tcpdump.png)

	00:09:17.xxxx			监听数据的时分秒
	IP：使用的协议类型
	192.168.21.1			数据包的一个方向（来自）
	>					数据的流向
	192.168.21.136		数据包的另外一个方向（到达）


![](/hexo-private-blog-website/images/tcpdump1.png)

## 三、项目上线流程（必须掌握）

	1、服务器选配购买
	项目上线服务器必须是外网服务器。

	一般服务器有2种情况：购买真实服务器、购买云服务器。

	购买真实服务器一次性成本过高，所以现在基本都是选择云服务器。

	云服务的厂商：阿里云、腾讯云、知道创宇（加速乐）、华为云、盛大云、新浪云（sae）、亚马逊云等等。
	以后以阿里云为例：
	官网：http://www.aliyun.com

[阿里云][aliyun]

[aliyun]:http://www.aliyun.com

	①打开阿里云官网，选择产品中的“云服务器ECS”

![](/hexo-private-blog-website/images/服务器.png)

	在页面上点击“立即购买”：

![](/hexo-private-blog-website/images/服务器1.png)

	②选择具体的配置

![](/hexo-private-blog-website/images/服务器1.png)


![](/hexo-private-blog-website/images/服务器2.png)


![](/hexo-private-blog-website/images/服务器3.png)


	安全组需要先在控制面板中创建，创建好之后才能在这里进行选择（安全组类似于防火墙，可以设置相关规则）：


![](/hexo-private-blog-website/images/服务器4.png)

	进入后台查看信息：

![](/hexo-private-blog-website/images/云服务器.png)

	需要重置密码的话，则可以选择右侧“更多”选择“重置密码”，然后重启服务器，最后可以通过远程终端连接服务器：

![](/hexo-private-blog-website/images/云服务器.png)

	2、域名购买
	①在首页产品中找到域名注册

![](/hexo-private-blog-website/images/云服务器1.png)

	域名注册得先查看是否可以注册：

![](/hexo-private-blog-website/images/云服务器1.png)

	选择需要的域名：

![](/hexo-private-blog-website/images/云服务器1.png)

	确认购买信息：

![](/hexo-private-blog-website/images/云服务器2.png)

	购买之后就可以在后台控制面板中去查看域名情况。

	3、域名备案
	备案：当申请域名的人要想在国内使用域名，则需要向当地的通信管理局（省级）去申请报备。
	备案前提：想要使用境内服务器的话，则必须得备案。

	在管理后台点击“ICP备案系统”

![](/hexo-private-blog-website/images/云服务器3.png)

	点击新增主体备案：

![](/hexo-private-blog-website/images/云服务器4.png)

	填写完基本信息之后点击增加网站：

![](/hexo-private-blog-website/images/云服务器4.png)

	备案服务号可以在控制台顶部去获取：













