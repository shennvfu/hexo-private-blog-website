---
title: Linux_Basics_05
date: 2020-08-05 14:15:18
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

	万壑有声含晚籁，数峰无语立斜阳。
	棠梨叶落胭脂色，荞麦花开白雪香。

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

# 一、Makefile



### 1、工程管理工具make



​		**工程管理：最终的目的就是编译源码生成二进制可执行文件**



### 2、Make的作用

​	  编译的步骤：

​	

```
① gcc -c xxx.c -o xxx.o
② gcc xxx.o -o xxx

例子：
	hello.c  ---> helle.o

	world.c  ---> world.o
	
```

​	make是根据过程中文件更新的时间来选择条件编译的，只编译跟新过的.c文件生成新的.o，其他没更改的不编译。



### 3、Makefile的编写规则



​	**make是一个工程管理工具，而make能实现运行起来起到效果需要有一个配置文件（Makefile）。makefile指定编译环境和编译选项。（指定编译器，目标源码，库，插件...）.Makefile是控制Make进行项目管理的配置文件。**

注：**最上面的脚本命令是生成最终目标的命令**

```
编写规则：

目标文件:依赖文件
制表符(TAB)脚本命令
```



#### 3.1、第一版本

---





```makefile
hello:hello.o
	gcc hello.o -o hello

hello.o:hello.c
	gcc -c hello.c -o hello.o
	
clean:#伪目标 没有依赖文件的
	rm *.o -rf
```


#### 3.2  第二版本（Make会自动推导生成.o文件）

---



```makefile
hello:hello.o
	gcc hello.o -o hello
		
clean:#伪目标 没有依赖文件的
	rm *.o -rf
```



#### 3.3  第三版本 （多文件编译）--- 练习

---

```makefile
两份.c:hello.c world.c

main:hello.o world.o
	gcc *.o -o main

hello.o:hello.c
	gcc -c hello.c -o hello.o

world.o:world.c
	gcc -c world.c -o world.o
	
clean:
	rm *.o -rf

```



#### 3.4 第四版本 （自动检索替换后缀名）

---



```makefile
SRCS  =$(wildcard *.c)  #存放所有.c的变量名
OBJS  =$(patsubst %.c,%.o,$(SRCS)) #以SRCS为基础。把.c替换成.o
CC    =gcc
ALIB  =xxx.a
TARGET=main #最终目标变量

$(TARGET):$(OBJS) $(ALIB)
	$(CC) $(OBJS) $(ALIB) -o $(TARGET)
	
%.o:%.c #静态模式
	gcc -c $(^) -o $(@)
	
clean:
	rm $(OBJS) $(TARGET) -rf
```

**注：**

​	**静态模式：一个目标可以表示多个文件的规则，根据目标文件替换多个依赖文件。**

​	**%.o：%.c**

​		**%.o:表示所有.o后缀的文件的集合，%.c：取前面%.o的%（第一个%表示的是:hello world）,后面在追加.c**



**练习：**

​		**使用Makefile文件和静态库结合，编译生成可以在开发板上运行的程序**

​			**注：静态库不需要在make编译，但是在Makefile里面链接**


	SRCS  =$(wildcard *.c)  #存放所有.c的变量名
	OBJS  =$(patsubst %.c,%.o,$(SRCS)) #以SRCS为基础。把.c替换成.o
	CC    =gcc
	TARGET=main #最终目标变量

	$(TARGET):$(OBJS)
		$(CC) $(OBJS) -o $(TARGET)
		
	%.o:%.c #静态模式
		gcc -c $(^) -o $(@)
		
	clean:
		rm $(OBJS) $(TARGET) -rf


一、如何在ARM架构上运行C程序

	1）用交叉编译工具链编译源码
		
		为什么需要换编译器：
			因为主机的CPU架构不同，导致了其指令集（CPU在运行程序的行为规范）不同。
			
			ubuntu:
					64:X64    32:X86
					
			开发板：ARM-Cortex A53
			
	2）如何烧写（下载）程序到开发板
	
	第一种：rx
		1：使用烧写工具rx  (arm-linux-gcc rx.c -o rx)
		
			在CRT输入烧写命令：
								rx 文件名字
								
			输入之后有两种文件的方式：
									1)直接鼠标拖拽要下载的文件按到CRT窗口内，发送xmodem
									2)点击传输，选择发送xmodem，在弹出窗口内选择要下载的文件
									
			设置默认的下载上传路径：点击选项 -->回话选择进行选择
								
								
		2：修改程序的权限后才可以运行程序
		
			在CRT里面输入修改权限的命令：
			
										chmod 777 文件名字
										
										
										chmod a+wrx 文件的名字
										chmod a=wrx 文件的名字
										
	第二种:rz
		
			先用rx把rz烧写到开发板
			
				输入命令：rx rz
				
			修改rz的权限：chmod 777 rz
			
			把rz移动固定存放在二进制的系统目录内/usr/bin
			
			mv rz /usr/bin
			
			
			使用rz下载：rz -y
			-y:覆盖更新
			
	
	第三种：
			U盘拷贝
			
				注意：开发板USB接口支持识别FAT32的文件系统
			
			第一步：把要拷贝的数据拷到U盘
			
			第二步：在CTR里面/mnt/udisk
			
			
	第四种：网络传输TFTP
	
		自己飞Q下载tftp32
		
		第一步：
		
				点击浏览：选择要下载（上传）的目录
				
				选择设置主机IP
				
		第二步：
				主从机互ping
				
		第三步：
				输入传输命令
				
			
			tftp 主机ip地址 -g -r  文件名字
			
			-g ：下载
			-r ：指定下载服务器文件的名字
			-p ：上传
			-l ：指定本地上传文件的名字
			
	补充：查看开发板的系统位数：
	
		uname -a
		\Linux GEC6818 3.4.39-gec #5 SMP PREEMPT Mon Jul 17 16:18:10 CST 2017 armv7l GNU/Linux
				
		armv7l：32位系统
		CPU：64位

		
二、Linux下的库的编译制作与使用

	库的作用：
			专门打包代码段（API + 头文件 + 宏定义...）,实现代码的复用，为了后期开发方便
			
	库的分类：
	
		 静态库：
				在链接静态库编译生成程序时，把库中的代码段拷贝到引用出。
				
				注意：
					生成的二进制会变大
					程序不需要带着静态库放在目标主机内
						
	静态库的编译与制作：
				静态库都是从.o文件开始编译制作的
				
		1)先用gcc生成.o文件（没有链接库文件的二进制）
			gcc -c xx.c -o xxx.o
			
		2)使用ar工具编译生成.a库文件
		
			ar -rcs 库的名字.a .o文件
			
			r: 把代码段插入库中
			c: 创建库文件
			s: 如果库中有<基于对象>,则生成对象对应的符号表 （变量 类型）
			S: 不生成符号表
			
			int a;
			long b[10];
			
		3)
			gcc lib_main.c libtest.a -o main 
			
			
	练习：
		简答的搭一个框架
		
		my_stdio.h  
		hello.c  printf_hello();
		world.c  printf_world();
		
		制作一个包含以上两个接口的静态库
		
	动态库：
		
		动态库的特点：
				
	
		动态库的编译与制作：
				动态库都是从.o文件开始编译制作的
				
				在链接动态库编译生成程序时，根据占位符找到对应引用的代码段执行程序。
				
				注意：
					生成的二进制相对使用静态库的方式 --- 小
					程序要带着动态库放在目标主机内
					
		制作动态库：
				
				第一步：先生成.o文件
						gcc -fPIC -c xxx.c -o xxx.o
						
						-fPIC:编译出的代码与位置无关，如果没有加此参数，那么在调用库里面的API是，
								是采用拷贝代码段到引用处。
				
				第二步：使用gcc生成动态库
				
						gcc -shared xxx.o -o libxxx.so
						
						-shared:指定生成共享库
						
				第三步：
				
						使用动态库：把调用库中接口的.c进行编译
						gcc xxx.c -o xxx -lxxx  -L 库的路径
						
						-l:指定库的名字 （告诉编译器库的名字）
						-L:指定库的路径 （告诉编译器库的路径）
						
				注意：虽然已经告诉编译器库的路径，但是也需要把库放在系统的环境变量里面，
						让系统在运行程序的时候找得到库。
						
						sudo mv libxxx.so /lib
	练习：

		制作一个可以在开发板是使用的库。
	

	作业：
		 预习：
				Makefile
				数据结构：
						链表+Linux内核链表
		 完成：
				minicom实现使用
				多做习题，多写代码
				总结
						




共享文件samba(是在Linux/unix上的一款基于局域网文件共享和打印机上的免费软件)
	

	如果之前有旧版本，执行命令：sudo apt-get remove libwbclient0 samba-common samba
			  删除旧版本的依赖软件包

		安装 sudo apt-get install samba
		配置共享目录/etc/samba/smb.conf
		在最后面添加
			[名字]
				[myshare]
				path=/home/cyz（路径必须要存在）
				browseable=yes 用来指定该共享是否可以浏览。
				writable=yes   writable用来指定该共享路径是否可写。
				read only=no
				guest ok=yes  意义同“public”。public用来指定该共享是否允许guest账户访问
		
		
		
		
		重启服务器
			sudo service smbd restart
			sudo service nmbd restart
                使用：电脑的运行中输入（\\ubuntu的ip，例如：\\192.168.1.8  注意：要关闭window系统的防火


	 8:38 2020/7/28
	makefile 编译工具

	环境变量 内核  shell解释器

	shell解释器 os内核 环境变量  

	如何获取系统内核源码  1.www.kernel.org  偶数表示长期支持版  奇数表示开发版本  2.使用命令进行下载 在命令行终端输入下载命令 sudo apt-get install 内核版本  /usr/src 存放下载好的内核版本的压缩包 
	reboot -w (模拟一次重启，把日志信息存放到/var/log/wtmp里面)    
	last 查看最后一次系统重启的信息  uname -a 查看系统信息   poweroff -n 关机时，不采用sync操作(强制把缓存区里面的数据刷进磁盘当中，需要自己手动) 非正常关机 poweroff -w (模拟一次重启，把日志信息存放到/var/log/wtmp里面) 日志：用来存放系统操作的一些信息
	Linux系统在运行的程序通常会把一些系统消息和错误消息写入对应的系统日志中，若是一旦出现问题，用户就可以通过查看日志来迅速定位，及时解决故障，所以学会查看日志文件也是在日常维护中很重要的操作。 poweroff -f 强制关机  poweroff -d 关闭操作系统时，不将操作写入日志文件“/var/log/wtmp”中添加相应的记录
	ps -ef 查看系统后台进程 
	kill 默认发送的是9号信号 也就是SIGKILL 18 SIGCONT 19 SIGSTOP 继续和停止  kill 进程号：直接终止对应的进程 kill -l：显示信号列表  kill -n 信号数/信号名字 进程号 killall -信号数 进程名 
	pidof 进程名 查看进程号 -s 只显示一个  pgrep  ps -ef | grep bash 过滤   前面命令的输出结果是后面命令的输入
	pgrep 通过一部分进程名字来显示符合的进程的进程号
	top 动态显示系统性能信息与运行信息  ssh服务 ftp服务
	top下的部分参数信息：   Tasks: 246 total, 进程总数
				1 running,        正在运行
				170 sleeping,     睡眠
				0 stopped,        停止
				0 zombie          挂起 zombie僵尸进程
	strcrp 

	系统配置 环境变量  当前用户 只在当前用户下创建环境变量 全局 每个用户都配置这个环境变量 
	.so动态库文件  ldd：查询程序所需要链接的库文件(动态库)
	硬链接 ln 源文件 链接的名字 特点：源文件和链接文件大小一样大 虽然占空间，但是没有源文件，硬链接还是可以使用 可以跨文件系统：能否使用ln命令直接跨系统创建链接(不能) 能否把链接复制到跨系统(能) 每个文件默认初始的硬链接数为：1(源文件本身就是自己的硬链接) 
	软链接：ln -s 源文件 链接的名字 特点：源文件肯定比链接文件大(节省内存消耗) 不能跨文件系统：能否使用ln命令直接跨系统创建链接(不能) 能否把链接复制到跨系统(能，但是变成源文件的形式)
	tty硬件的一个端口 一个创口   uptime 显示系统负载  平均负载数  5s 10s 15s 一般的，一个CPU核心运行负载数小于3，表示系统性能很好
	一个CPU核心运行负载数大于5，表示系统性能快崩溃  一个CPU两个核心，负载数为6   
	who 当前登录用户的信息  
	检测系统状态命令  ifconfig  查看网卡信息  ifconfig 网卡型号 IP地址  更改IP地址  service network-manager restart 重启网络服务
	uname -a 显示系统的全部信息 uname -n 网络主机名 uname -m 系统位数(系统CPU架构) uname -v 系统版本
	alias 取别名 alias l='ls' (只在当前生效，重启之后失效) 永久使用：写入系统的配置文件 当前用户的配置文件: ~/.brashrc(隐藏的文件) 全局的配置文件(系统环境变量):/etc/profile 最后记得生效该配置文件(让系统重新加载该配置文件):source ~/.brashrc  source /etc/profile 或者重新启动系统让该配置文件生效
	寄存器  控制形参的个数  有效的控制形参的个数
	int (*(*fun(int *(*p) (int *)))[5]) (int *)

	头文件中的ifndef/define/endif有什么作用  防止该头文件被重复引用


	10:40 2020/7/29
	which 查看对应文件的路径
	压缩 先把目标压缩成tar格式 tar cvf 压缩包的名字.tar 目标路径
	把tar转成xz格式 xz -d 压缩包的名字.tar
	z:强制压缩
	d:强制解压
	解压缩: 把xz -- tar 再解压tar  
	10:56 2020/7/29
	不用wc命令去查看代码文件 
	find -iname 文件名字  i: 忽略大小写   /usr/include /usr/编译器 用户家目录 合法空间  gcc编译器  arm编译器  交叉编辑工具类 
	标准出错 stddef.h  
	find -r 递归搜索

	VMware Workstation Pro 15 激活许可证
	VF750-4MX5Q-488DQ-9WZE9-ZY2D6
	UU54R-FVD91-488PP-7NNGC-ZFAX6
	YC74H-FGF92-081VZ-R5QNG-P6RY4
	YC34H-6WWDK-085MQ-JYPNX-NZRA2 


	17:29 2020/7/29
	共享文件samba

		如果之前有旧版本，执行命令：sudo apt-get remove libwbclient0 samba-common samba
				  删除旧版本的依赖软件包

			安装 sudo apt-get install samba
			配置共享目录/etc/samba/smb.conf
			在最后面添加
				[名字]
					[myshare]
					path=/home/gec（路径必须要存在）
					browseable=yes 用来指定该共享是否可以浏览。
					writable=yes   writable用来指定该共享路径是否可写。
					read only=no
					guest ok=yes  意义同“public”。public用来指定该共享是否允许guest账户访问
			
			
			
			
			重启服务器
				sudo service smbd restart
				sudo service nmbd restart
	                使用：电脑的运行中输入（\\ubuntu的ip，例如：\\192.168.1.8  注意：要关闭window系统的防火墙）
	samba用于局域网


	17:32 2020/7/29
	出于安全性考虑，POSIX阵营的OS(如Linux)一般不推荐直接以root登录并操作。在默认状态下，Ubuntu只能以普通用户登录，如果登录之后需要root权限，则使用sudo来临时获取，但不是每一个普通用户都可以使用sudo，要使得一个普通用户能够通过sudo来获取管理员权限，则必须在配置文件/etc/sudoers中做相应的配置。在安装完操作系统后，系统中只有一个普通用户，就是上面我们输入的那个用户，这个用户是可以用sudo来临时获得管理员权限的，但是如果再新添加一个用户，如foo，那么这个foo默认情况下是无法使用sudo来临时获得管理员权限的。接下来，通过配置/etc/sudoers来使foo可以使用sudo步骤如下：
	首先在命令行中输入sudo visudo 然后在rootALL=(ALL)ALL 下面手动添加一行:fooALL=(ALL)ALL
	这一行的作用是将用户foo添加到sudoer中。第一个ALL代表所有的主机名，等号后面的(ALL)代表foo可以以任意的用户运行后面的命令，最后一个ALL指的是foo可以用sudo运行所有shell命令。然后按提示保存(Ctrl+O组合键)并退出(Ctrl+X组合键)。另外使foo可以使用sudo命令还有一个更加简单的方法：第一，使用su命令切换到管理员账户;第二，运行命令usermod foo -a -G sudo;第三，运行su foo 命令切换到foo账户，此时foo就可以使用sudo来获取管理员权限了。

	下载安装gnome桌面环境  sudo apt-get install gnome-session-fallback
	网络配置
	sudo gedit /etc/network/interfaces 在里面配置网卡信息  如
	auto eth0
	iface eth0 inet static(dhcp) 如果是配置dhcp就不用配置下面的ip地址、网关、和子网掩码了
	address x.x.x.x
	gateway x.x.x.x
	netmask x.x.x.x
	其中：
	auto eth0 代表系统自动识别且启动第0个以太网卡
	static 代表设置固定IP(如果要让系统自动获取IP地址，请将static改成dhcp，同时可以删除下面三行)
	address是Ubuntu的IP地址，gateway是路由器的IP地址。
	netmask是子网掩码。如果用户的网络是C类子网，那么子网掩码就是255.255.255.0  如果用户的网络是B类子网，那么子网掩码就是255.255.0.0;如果不知道用户的网络具体情况，可以在Windows的命令行中敲入ipconfig/all 来查看。
	配置DNS服务器 编辑/etc/resolv.conf 在文件的最末一行添加以下信息
	nameserver 4.4.4.4
	nameserver 114.114.114.114 / 223.5.5.5
	可以填写离自己比较近的DNS地址，详情咨询百度
	为主机配置网关地址。在终端下输入 sudo route add default gw x.x.x.x
	重新加载网络配置文件，并重新启动网络服务。在终端下输入以下两行命令： sudo /etc/init.d/networking force-reload  sudo /etc/init.d/networking restart
	如果有必要，重启系统 sudo shutdown -r now / sudo shutdown -h now

	APT软件管理器  APT将会帮助用户解决软件的互相依赖问题
	需要将选定站点的软件列表信息，全部下载到本地(/var/lib/apt/lists) var目录是用于存放日志的  软件列表信息是指如软件名称、依赖关系版本号等 这个过程称为update sudo apt-get update
	软件的下载安装和卸载  sudo apt-get install xxx     sudo apt-get remove xxx

	vim 插件
	ctag 负责建立标签 为实现文本间关键词实现跳转提供基础
	Taglist是一个vim插件 帮助我们罗列程序中所有出现关键词的地方
	sudo apt-get install ctags
	tags文件是实现跳转功能的 就是它把我们送到我们想要去的地方  如笔者在自己的程序里写了一个库函数printf，在某个时刻笔者想查看这个库函数本身是怎么实现的，那么只需要把光标停在关键词上，再按一下组合键(Ctrl+]) 就会立刻跳转到库函数printf的源代码的地方，按一下组合键(Ctrl+O)就可以跳回来。当然如果printf是库函数对一个系统调用的封装，就可以顺着tags给我们提供的道路跳到内核去查看源代码是怎么写的，当然这期间可能会有不止简单的两层封装定义，但我们一次次跳转即可深入其里，了解其内幕。
	一开始，需要库函数的源代码和Linux内核的源代码，我们的目的就是在需要时可以跳转到这些地方的某些文件当中去查看相关的资料信息，有了上面的ctags工具之后，我们就可以在源代码的顶层目录处执行下面这条命令: ctags -R 比如，想要我们的程序能随时去库函数里查询原型，那么就可以在库函数源代码的顶层目录下执行上面那条命令，假如我们的库路径是~/downloads/glibc-2.9 那么代码如下 cd ~/Downloads/glibc-2.9   ctags -R 命令中的选项-R的意思是：递归地进入当前目录下的所有子目录，把在该目录下的所有文件的关键词(包括函数名、宏、文件名等关联到一起，并且写入一个tags文件)。当然，如果想让我们的函数可以跳转到内核，那么我们应在内核源代码的顶层目录下执行以上命令。然后，在/etc/vim/vimrc文件末尾，添加以下信息： au BufEnter /home/vincent/* setlocal tags+=/home/vincent/glibc-2.9/tags 当然，要把上面相应的路径换成自己计算机上的具体路径。其中/home/vincent/*的意思是：在该路径下的所有文件(因为用了通配符*)都可以通过tags文件实现跳转(包括其子目录)，而这个tags文件，就是由后面这个路径/home/vincent/glibc-2.9/tags指定的。也许读者会问，干脆写成/*就行啦，那么系统中的任何一个文件我们都可以跟glibc-2.9关联，实现跳转，当然可以这么做，但有时我们也许并不需要这么做。
	这就可以了，我们现在就可以享受自由跳转的乐趣了，但我们还可以加更多的东西，比如把内核源代码也添加进来，必要时我们就跳到内核中去看看怎么实现。如法炮制，现在内核源代码顶层目录指向指令ctags -R,然后在/etc/vim/vimrc文件末尾再添加一句话即可，当然添加时要把tags所在的路径替换成内核源代码的路径。例如，添加以下信息(注意/home/vincent要换成自己的系统的家目录路径)：au BufEnter /home/vincent/* setlocal tags+=/home/vincent/Linux-2.6.31/tags 当然我们还需要一个非常重要的vim命令ts，因为要跳转的关键词可能出现在库函数中，也可能出现在内核源码中，还可能同时都有对此关键字的定义，这时就要在vim命令模式下输入:ts来罗列出所有出现该声明关键词的地方(显然应先把光标停在想要跳转的关键词上面)，然后按相应的序号再进行跳转。罗列的次序跟我们在vimrc中写au指令的顺序相关，谁写在上面就先罗列谁。
	Taglist是vim的一个插件 可以方便地在终端侧边显示出当前程序所有的函数、宏等信息，支持鼠标双击跳转，对于规模比较大的代码而言，这是一个非常实用的功能。Taglist的使用非常简单，只需要在网上下载一个配置文件即可，可以用下面这个链接下载：http://download.csdn.net/detail/vincent040/6529593  下载完了解压会出现两个文件夹(doc和plugin)然后就可以把这两个文件夹放到主目录下的隐藏文件夹.vim中(如果没有这个隐藏目录的话就自己创建一个)完成之后，用vim打开我们的程序源码，输入命令：Tlist打开列表，再输入一次关闭列表，即可。
	man -k
	man -f
	man man man命令帮助我们查找需要的信息，而这些信息被归类为以下几大类别
	1.Shell命令(默认已安装)
	2.系统调用
	3.库函数
	4.特殊文件(通常出现在/dev目录下)
	5.文件的特殊格式或协定(如/etc/passwd的格式)
	6.游戏
	7.杂项(如一些宏定义)
	8.系统管理员命令(通常只能由管理员执行)
	9.非标准内核例程
	如果Ubuntu当前没有安装完整的man手册，那可能查不到所需要的帮助资料，这时我们的第一个任务就是要在联网的情况下下载并安装相关的man帮助文档，命令如下：
	sudo apt-get install manpages
	sudo apt-get install manpages-dev
	sudo apt-get install manpages-posix
	sudo apt-get install manpages-posix-dev

	系统调用(System Calls)指定是操作系统向上层应用提供的接口函数，UNIX/Linux的系统调用有家族式的简约美德，它们通常功能健壮单一。
	系统调用使得应用程序不需要直接访问内核也能使用内核提供的功能，这有利于降低应用程序的复杂性，可移植性，有利于对操作系统接口做出统一规范的标准，也使得内核更加安全。
	POSIX全称Potable Operating System Interfaces(可移植性操作系统接口)，它实际上是一组规范不同操作系统向上层应用程序提供的接口函数的标准，使得各种操作系统具有可移植性。X透露出它与UNIX/Linux是一个阵营的，事实上Linux是遵循POSIX标准的典范。

	20:35 2020/7/29
	使用了映射文件夹感觉还是共享文件好用
	在尝试了重装VMware tools等奇奇怪怪的方法以后仍未看到我心心念念的共享文件夹。
	十分恼火。
	不过皇天不负有心人，最终还是让我找到了解决办法。
	我使用的VMware是15.5.0版本，而Ubuntu为18.04，
	安装了VMwareTools-10.0.12-4448491后，共享文件夹出现了，但是更新到VMwareTools-10.3.10-13959562后就又没有了。最终是使用命令：
	sudo vmhgfs-fuse .host:/ /mnt/hgfs/ -o allow_other -o uid=1000
	重新挂载了共享文件夹后改变了它的权限，终于可以看到了，完美解决！
	0:53 2020/7/30
	使用了映射文件夹感觉还是共享文件好用
	在尝试了重装VMware tools等奇奇怪怪的方法以后仍未看到我心心念念的共享文件夹。
	十分恼火。
	不过皇天不负有心人，最终还是让我找到了解决办法。
	我使用的VMware是15.5.0版本，而Ubuntu为18.04，
	安装了VMwareTools-10.0.12-4448491后，共享文件夹出现了，但是更新到VMwareTools-10.3.10-13959562后就又没有了。最终是使用命令：
	sudo vmhgfs-fuse .host:/ /mnt/hgfs/ -o allow_other -o uid=1000
	重新挂载了共享文件夹后改变了它的权限，终于可以看到了，完美解决！
	0:53 2020/7/30
	使用了映射文件夹感觉还是共享文件好用
	在尝试了重装VMware tools等奇奇怪怪的方法以后仍未看到我心心念念的共享文件夹。
	十分恼火。
	不过皇天不负有心人，最终还是让我找到了解决办法。
	我使用的VMware是15.5.0版本，而Ubuntu为18.04，
	安装了VMwareTools-10.0.12-4448491后，共享文件夹出现了，但是更新到VMwareTools-10.3.10-13959562后就又没有了。最终是使用命令：
	sudo vmhgfs-fuse .host:/ /mnt/hgfs/ -o allow_other -o uid=1000
	重新挂载了共享文件夹后改变了它的权限，终于可以看到了，完美解决！
	8:51 2020/7/30
	共享文件samba

		如果之前有旧版本，执行命令：sudo apt-get remove libwbclient0 samba-common samba
				  删除旧版本的依赖软件包

			安装 sudo apt-get install samba
			配置共享目录/etc/samba/smb.conf
			在最后面添加
				[名字]
					[myshare]
					path=/home/gec（路径必须要存在）
					browseable=yes 用来指定该共享是否可以浏览。
					writable=yes   writable用来指定该共享路径是否可写。
					read only=no
					guest ok=yes  意义同“public”。public用来指定该共享是否允许guest账户访问
			
			
			
			
			重启服务器
				sudo service smbd restart
				sudo service nmbd restart
	                使用：电脑的运行中输入（\\ubuntu的ip，例如：\\192.168.1.8  注意：要关闭window系统的防火墙）

	samba专门用于局域网

	系统设置打不开，请重新安装gnome-control-center
	sudo apt-get install gnome-control-center

	栈是后进先出

	Ubuntu 下载安装gnome桌面环境 sudo apt-get install gnome-session-fallback
	配置网络信息 sudo gedit /etc/network/interfaces
	配置DNS服务器 sudo gedit /etc/resolv.conf
	为主机配置网关地址 sudo route add default gw x.x.x.x
	重新加载网络配置文件，并重新启动网络服务 sudo /etc/init.d/networking force-reload   sudo /etc/init.d/networking restart
	重启系统 sudo shutdown -r now
	将选定站点的软件列表信息全部下载到本地(/var/lib/apt/lists) var 是存放日志的目录  软件列表信息如软件名称，依赖关系，版本号等等
	这个过程称为update    sudo apt-get update
	UbuntuAPT软件管理器  APT会帮助用户解决软件的互相依赖问题
	我们可以使用APT提供的相关命令来下载安装软件 下载安装和卸载的命令是 sudo apt-get install xxx 卸载： sudo apt-get remove xxx
	vim插件 ctag  ctag负责建立标签 为实现文本间关键词实现跳转提供基础 Taglist插件 帮助我们罗列程序中所有出现关键词的地方
	sudo apt-get install ctags 如果不行试试exuberant-ctags 
	Taglist是vim的一个插件,可以方便地在终端侧边显示出当前程序所有的函数、宏等信息，支持鼠标双击跳转，对于规模比较大的代码而言，是一个非常实用的功能
	ctag文件是实现跳转功能的 就是它可以把我们送到我们想要去的地方 如果别人在他自己的程序里写了一个库函数printf。那在某个时刻他想去查看这个库函数本身是怎么实现的那么他只需要把光标停在关键词上，再按一下组合键Ctrl+] 就会立即跳转到库函数printf的源代码的地方，再按一下组合键Ctrl+O就可以跳转回来。当然如果printf是库函数对一个系统调用的封装，就可以顺着tags


	如果后续需要重启网卡怎么去操作呢？
	#service network restart

	在有的分支版本中可能没有service命令来快速操作服务，但是有一个共性的目录：/etc/init.d
	这个目录中放着很对服务的快捷方式。
	此处重启网卡命令还可以使用：
	#/etc/init.d/network restart

	二、开发板的开发环境的搭建（嵌入式Linux交叉开发环境）

		1:
			串口线（电脑和开发板）+ 串口驱动 + 超级终端（CRT）
			
			第一步：链接串口线
			
			第二步：检查windows是否安装串口驱动
					查看设备管理器中的COM和LPT
						记住端口号和协议
						
			第三步：打开超级终端（CRT），进行配置
			
				波特率：使用串口线传输的速度 
				
						115200 bit/s
						
					流控：RTS/CTS(去掉)
					
					
		2：配置开发板不自启动物联网试验箱项目
		
			打开配置文件 vi /etc/profile
			
				#注释掉最后的启动命令
				
				
		3：永久设置开发板IP
		
			把设置IP的命令写入配置文件vi /etc/profile的末尾
			
			ifconfig eth0 xxx.xxx.xxx.xxx 
			(开发板和windows和Ubuntu的IP不能冲突)
			
			三个机器互ping，检测一下
			
			
		4：检测触摸屏：
		
			在CRT输入：cat /dev/input/event0
	14:37 2020/7/30
	开发板的嵌入式操作系统，包含 Linux 和 Android 操作系统。我们出厂时会烧写或者固化 其中一个操作系统在里面。本手册讲述如何固化嵌入式操作系统到我们的开发板中。
	开发板的嵌入式操作系统，包含 Linux 和 Android 操作系统。我们出厂时会烧写或者固化 其中一个操作系统在里面。本手册讲述如何固化嵌入式操作系统到我们的开发板中。
	注意事项 我们把编译好的镜像系统文件，通过 SD 或者 USB 的下载方式，固化到板载的 eMMC 储 存器中（ROM），以下简称为“‘刷机”。 方法一：通过 fastboot 工具，USB 下载方式 
	方法二：通过 SD 卡方式 使用 fastboot 工具烧写 Linux 和 android 映像时，核心板必须存在 uboot（引导程序），因为烧写时需要使用 uboot 上的 fastboot 功能， 在板子不存在 uboot 时，请使用 SD 卡烧写方式。 使用 fastboot 烧写时，电脑上必须存在串口接口或者拥有 usb 转串口模块，使其连接电脑 与开发板，让电脑能够通过串口与开发板通信
	开发板启动顺序 6818 开发板硬件配置固定了开发板启动顺序如下： 
	1st：从 TF 卡启动 
	2nd：从 EMMC 启动 
	3rd：从 USB 启动 开发板上电后首先从 TF 卡启动，若 SD0 插入了启动卡则从 SD 启动； 如果 SD0 未插卡 或者插入的不是启动卡，则启动失败；然后从板载 EMMC(SD2)启动，若 EMMC 中已经烧录 固件则启动成功，否则启动失败，最后尝试从 USB 启动。
	Windows 下使用 fastboot 烧写（推荐） 安装串口工具 secureCRT 1、 下载并安装 secureCRT 工具，打开工具，点击左上角“快速链接”按钮：
	2、 使用串口线或 USB 转串口模块连接开发板与电脑，打开 Windows 的设备管理器，查看串 口端口号：
	可以看到串口端口号为 COM4
	3、 回到 secureCRT 工具界面，设置“快速链接”的配置。选择协议为 Serial，端口为 COM4， 波特率为 115200，取消勾选流控 RTS/CTS：
	4、 点击连接后，打开开发板电源，secureCRT 终端输出开发板启动信息，说明 secureCRT 配 置完成：
	安装 fastboot 1、 说明：在多数情况下，在 Windows 下使用 fastboot 工具烧写并不需要把 fastboot 安装 到系统中，只需要解压 fastboot 工具并在解压目录中运行工具进行烧写即可。
	烧写 Linux 映像 
	1、使用串口线或 USB 转串口模块连接开发板与电脑。 
	2、打开 secureCRT 终端连接开发板串口。 
	3、打开开发板电源，在 secureCRT 中查看串口打印的启动信息，在 uboot 启动的 3 秒内按 任意键进入 uboot 命令行模式，执行如下指并回车：
	secureCRT 终端下将打印如下信息： Fastboot Partitions: mmc.2: ubootpak, img : 0x200, 0x78000 mmc.2: 2ndboot, img : 0x200, 0x4000 mmc.2: bootloader, img : 0x8000, 0x70000 mmc.2: boot, fs : 0x100000, 0x4000000 mmc.2: system, fs : 0x4100000, 0x2f200000 mmc.2: cache, fs : 0x33300000, 0x1ac00000 mmc.2: misc, fs : 0x4e000000, 0x800000 mmc.2: recovery, fs : 0x4e900000, 0x1600000 mmc.2: userdata, fs : 0x50000000, 0x0 Support fstype : 2nd boot factory raw fat ext4 emmc nand ubi ubifs Reserved part : partmap mem env cmd DONE: Logo bmp 311 by 300 (3bpp), len=280854 DRAW: 0x47000000 -> 0x46000000 Load USB Driver: android Core usb device tie configuration done OTG cable Connected!
	4、插入 micro USB 线连接到电脑。 
	5、解压 fastboot 工具压缩包到一个目录下，把 Linux 映像文件 ubootpak.bin、boot.img、 qt-rootfs.img 全部复制到该目录中。 
	6、右键使用记事本编辑 Windows 脚本文件 auto.bat，查看烧写映像文件名是否与我们编译 出来的 android 映像文件名相同，不相同则重命名 android 映像文件名。 脚本文件 auto.bat 的内容： fastboot flash ubootpak ubootpak.bin fastboot flash boot boot.img fastboot flash system qt-rootfs.img fastboot reboot fastboot 文件夹下各文件如图：
	7、确认无误后，退出编辑，双击打开（或右键管理员权限打开）auto.bat，可以看到 Windows 下会打开命令终端，打印出如下信息：
	6、在 secureCRT 终端下，会打印出如下信息，说明烧写成功：
	7、烧写完成后，Windows 命令框会自动退出，按下重启键重新启动开发板。在 uboot 启动 的 3 秒内按任意键进入 uboot 命令行模式，执行如下指令，设置系统启动环境变量，保存后重 新启动即烧写成功： setenv bootcmd "ext4load mmc 2:1 0x48000000 uImage;bootm 0x48000000" save 执行完以上指令，即可正常启动 Linux 系统了。每执行一条指令，在液晶屏上都会有相 应的界面提示，用户可以很清晰的观察升级的状态。
	烧写 android 映像 
	1、使用串口线或 USB 转串口模块连接开发板与电脑。 
	2、打开 secureCRT 终端连接开发板串口。 
	3、打开开发板电源，在 secureCRT 中查看串口打印的启动信息，在 uboot 启动的 3 秒内按 任意键进入 uboot 命令行模式，执行如下指并回车： fastboot secureCRT 终端下将打印如下信息： 
	Fastboot Partitions: mmc.2: ubootpak, img : 0x200, 0x78000 mmc.2: 2ndboot, img : 0x200, 0x4000 mmc.2: bootloader, img : 0x8000, 0x70000 mmc.2: boot, fs : 0x100000, 0x4000000 mmc.2: system, fs : 0x4100000, 0x2f200000 mmc.2: cache, fs : 0x33300000, 0x1ac00000 mmc.2: misc, fs : 0x4e000000, 0x800000mmc.2: recovery, fs : 0x4e900000, 0x1600000 mmc.2: userdata, fs : 0x50000000, 0x0 Support fstype : 2nd boot factory raw fat ext4 emmc nand ubi ubifs Reserved part : partmap mem env cmd DONE: Logo bmp 311 by 300 (3bpp), len=280854 DRAW: 0x47000000 -> 0x46000000 Load USB Driver: android Core usb device tie configuration done OTG cable Connected!
	4、插入 micro USB 线连接到电脑。 
	5、解压 fastboot 工具压缩包到一个目录下，把 android 映像文件 ubootpak.bin、boot.img、 system.img、cache.img、userdata.img 全部复制到该目录中。 
	6、右键使用记事本编辑 Windows 脚本文件 auto.bat，查看烧写映像文件名是否与我们编译 出来的 android 映像文件名相同，不相同则重命名 android 映像文件名。脚本文件 auto.bat 的内 容：fastboot flash ubootpak ubootpak.bin fastboot flash boot boot.img fastboot flash system system.img fastboot flash cache cache.img fastboot flash userdata userdata.img fastboot 文件夹下各文件如图：
	7、确认无误后，退出编辑，双击打开（或右键管理员权限打开）auto.bat，可以看到 Windows 下会打开命令终端，打印出如下信息：
	6、在 secureCRT 终端下，会打印出如下信息，说明烧写成功：
	7、烧写完成后，Windows 命令框会自动退出，按下重启键重新启动开发板。在 uboot 启动 的 3 秒内按任意键进入 uboot 命令行模式，执行如下指令，设置系统启动环境变量，保存后重 新启动即烧写成功： setenv bootcmd "ext4load mmc 2:1 0x48000000 uImage;ext4load mmc 2:1 0x49000000 root.img.gz; bootm 0x48000000" 
	save
	Linux 下使用 fastboot 烧写（不推荐） 
	安装串口终端 minicom 
	1、使用如下指令安装： sudo apt-get install minicom 
	2、如果是使用 USB 转串口模块，目前市面上大多都是 pl2303 方案，需要输入如下命令查询 驱动是否正常加载： 
	lsmod |grep pl2303 
	返回如下信息则加载正常： 
	lqm@lqm:~$ lsmod |grep 
	pl2303 pl2303 11756 1 
	usbserial 33100 3 pl2303 
	3、查看串口设备名：
	dmesg | tail -f
	返回：ERROR! H2M_MAILBOX still hold by MCU. command fail 
	---> RTMPFreeTxRxRingMemory 
	<--- RTMPFreeTxRxRingMemory RTUSB disconnect successfully 
	usb 2-4: USB disconnect, address 3 
	pl2303 ttyUSB0: pl2303 converter now disconnected from ttyUSB0 
	pl2303 2-4:1.0: device disconnected 
	usb 2-4: new full speed USB device using ohci_hcd and address 5 
	pl2303 2-4:1.0: pl2303 converter detected 
	usb 2-4: pl2303 converter now attached to ttyUSB0 
	exit 0 
	其中 ttyUSB0 就是串口设备名。
	4、输入命令配置串口参数： 
	sudo minicom -s 
	选择 Serial port setup，  
	设置串口终端：选择 A，输入正确的串口终端，如果直接使用串口，通常设置为 ttyS0， 如果使用 USB 转串口，通常设置为 ttyUSB0，回车。ttyS0 以及 ttyUSB0 中的 0 代表串口 0。
	设置波特率以及数据位：选择 E，输入 115200 8N1，回车。115200 代表波特率，8N1 代表 8 个数据位，1 个停止位。  
	设置流控：选择 F 和 G，都设置为 No，表示不使用流控，回车。 选择 Save setup as dfl 保存设置。 
	5、常见使用问题： 非正常关闭 minicom，会在/var/lock 下创建几个文件 LCK*，这几个文件阻止了 minicom 的运行，将它们删除后即可恢复正常。
	安装 fastboot 工具 
	1、执行如下指令安装 fastboot： 
	sudo apt-get install android-tools-fastboot 
	2、在使用 fastboot 时，需要获取 root 权限，为了解除这个限制，可以使用如下方法，让 使用 fastboot 时不需要管理员权限。 到/etc/udev/rules.d/目录下，新建 51-android.rules 文件，内容如下： （注意， OWNER 里面填的”gec”务必换成自己 ubuntu 系统的用户名）
	# adb protocol on passion (Nexus One) SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="4e12", MODE="0666",OWNER="gec" # adb protocol on crespo/crespo4g (Nexus S) SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="4e22", MODE="0666",OWNER="gec" # fastboot protocol on crespo/crespo4g (Nexus S) SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="4e20", MODE="0666",OWNER="gec" # fastboot protocol on stingray/wingray (Xoom) SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="708c", MODE="0666",OWNER="gec" # fastboot protocol on maguro/toro (Galaxy Nexus) SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="4e30", MODE="0666",OWNER="gec" # fastboot protocol on x210/x4412/x6818 SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="0002", MODE="0666",OWNER="gec"
	烧写 Linux 映像
	1、 使用串口线或 USB 转串口模块连接开发板与电脑。 
	2、 执行如下指打开 minicom： minicon 
	3、 打开开发板电源，在 minicom 中查看串口打印的启动信息，在 uboot 启动的 3 秒内按 任意键进入 uboot 命令行模式，执行如下指并回车： fastboot minicom 终端下将打印如下信息：
	Fastboot Partitions: mmc.2: ubootpak, img : 0x200, 0x78000 mmc.2: 2ndboot, img : 0x200, 0x4000 mmc.2: bootloader, img : 0x8000, 0x70000 mmc.2: boot, fs : 0x100000, 0x4000000 mmc.2: system, fs : 0x4100000, 0x2f200000 mmc.2: cache, fs : 0x33300000, 0x1ac00000 mmc.2: misc, fs : 0x4e000000, 0x800000 mmc.2: recovery, fs : 0x4e900000, 0x1600000 mmc.2: userdata, fs : 0x50000000, 0x0 Support fstype : 2nd boot factory raw fat ext4 emmc nand ubi ubifs Reserved part : partmap mem env cmd DONE: Logo bmp 311 by 300 (3bpp), len=280854 DRAW: 0x47000000 -> 0x46000000 Load USB Driver: android Core usb device tie configuration done OTG cable Connected! ------------------------------------------ 
	4、 插入 micro USB 线连接到电脑。 
	5、 另外开启一个 ubuntu 命令行终端，输入如下指令更新 Linux 系统各个部分需要的映 像：fastboot flash ubootpak ubootpak.bin fastboot flash boot boot.img fastboot flash system qt-rootfs.img 
	6、烧写完成后，Windows 命令框会自动退出，按下重启键重新启动开发板。在 uboot 启动 的 3 秒内按任意键进入 uboot 命令行模式，执行如下指令，设置系统启动环境变量，保存后重 新启动即烧写成功：
	setenv bootcmd "ext4load mmc 2:1 0x48000000 uImage;bootm 0x48000000" save 执行完以上指令，即可正常启动 Linux 系统了。每执行一条指令，在液晶屏上都会有相应 的界面提示，用户可以很清晰的观察升级的状态。
	烧写 android 映像 
	1、 使用串口线或 USB 转串口模块连接开发板与电脑。 
	2、 执行如下指打开 minicom： minicon 
	3、 打开开发板电源，在 minicom 中查看串口打印的启动信息，在 uboot 启动的 3 秒内按 任意键进入 uboot 命令行模式，执行如下指并回车： fastboot 
	minicom 终端下将打印如下信息：
	Fastboot Partitions: mmc.2: ubootpak, img : 0x200, 0x78000 mmc.2: 2ndboot, img : 0x200, 0x4000 mmc.2: bootloader, img : 0x8000, 0x70000 mmc.2: boot, fs : 0x100000, 0x4000000 mmc.2: system, fs : 0x4100000, 0x2f200000 mmc.2: cache, fs : 0x33300000, 0x1ac00000 mmc.2: misc, fs : 0x4e000000, 0x800000 mmc.2: recovery, fs : 0x4e900000, 0x1600000 mmc.2: userdata, fs : 0x50000000, 0x0 Support fstype : 2nd boot factory raw fat ext4 emmc nand ubi ubifs Reserved part : partmap mem env cmd DONE: Logo bmp 311 by 300 (3bpp), len=280854 DRAW: 0x47000000 -> 0x46000000 Load USB Driver: android Core usb device tie configuration done OTG cable Connected! ------------------------------------------ 
	4、 插入 micro USB 线连接到电脑。 
	5、 另外开启一个 ubuntu 命令行终端，输入如下指令更新 android 系统各个部分需要的映 像：
	fastboot flash ubootpak ubootpak.bin fastboot flash boot boot.img fastboot flash system system.img fastboot flash cache cache.img fastboot flash userdata userdata.img 
	6、烧写完成后，Windows 命令框会自动退出，按下重启键重新启动开发板。在 uboot 启动 的 3 秒内按任意键进入 uboot 命令行模式，执行如下指令，设置系统启动环境变量，保存后重 新启动即烧写成功： setenv bootcmd "ext4load mmc 2:1 0x48000000 uImage;ext4load mmc 2:1 0x49000000 root.img.gz; bootm 0x48000000" save 执行完以上指令，即可正常启动 android 系统了。每执行一条指令，在液晶屏上都会有相 应的界面提示，用户可以很清晰的观察升级的状态。

	第三章 使用 SD 卡烧写镜像 
	注意事项 使用 SD 卡烧写必须准备一张不小于 2GB 的 micro SD 卡以及 micro SD 卡读卡器。使用 SD 卡烧写会清空 SD 卡中原有内容，请提前做好备份。 
	Windows 下制作 SD 启动卡（推荐） 
	1、注意事项： WinPM 这个软件是为 windowsXP 而设置的，使用 windows7 或者 windows8 或者 windows10 的同学记得选择该软件，右键 —> 兼容性建议 —> 尝试建议性操作 -->启动 程序）
	2、整个流程是在 Windows 下要先通过 WinPM 这个软件来对 SD 卡进行分区，然后再使用 IROM_Fusing_Tool_6818.exe 的软件，将我们编译好的 bootloader 固化到 SD 卡里面，然后再选 择从 SD 卡启动我们的开发板进行烧写。
	以下是详细步骤： 
	1）打开 VinPM，这个软件位于光盘文件的 Tools 目录下面。 
	2）选择 SD 卡，注意：选择你正确的 SD 卡盘符
	3）预留 250M 的空间给 uboot。
	4）选择是 FAT32 的格式
	5）选择确定
	6）选择剩余的未分配的空间，右键选择装载
	7）系统会给我们新建的这个盘分一个盘符，我设置为 K，点击确定即可
	8）对我们的 SD 卡分区好之后，点击应用点击确定，就可以对我们的 SD 卡进行分区，建立分 区表信息。
	9）分区完成之后，点击关闭即可。
	10）对 SD 卡分好区之后，以管理员权限运行 IROM_Fusing_Tool_6818.exe 工具。点击 Browse 浏览到已经编译好的 uboot，所需烧写的 uboot 文件在 out/release 目录下，文件名字为： ubootpak.bin，如下图所示：
	11）选择我们需要烧写的选择正确的盘符（我们上面设置 SD 的盘符是 K，这里选择 K）。然 后点击 START，显示 Fusing image done 表示烧写完成。完成 uboot
	至此，SD 启动卡的制作已经完成。 
	Linux 下制作 SD 启动卡
	1、输入如下命令查看 linux 系统原有的设备节点。 
	cat /proc/partitions 
	返回：
	major minor #blocks name 
	8 0 36700160 sda 
	8 1 512000 sda1 
	8 2 36187136 sda2 
	253 0 34144256 dm-0 
	253 1 2031616 dm-1 
	2、使用读卡器连接 SD 卡与电脑，输入如下命令查看设备节点，经过对比原有设备节点获得 SD 卡设备节点名为 sdb
	cat /proc/partitions 
	返回：
	major minor #blocks name 
	8 0 36700160 sda 
	8 1 512000 sda1 
	8 2 36187136 sda2
	253 0 34144256 dm-0 
	253 1 2031616 dm-1 
	8 16 3879936 sdb 
	8 17 3875840 sdb1 
	3、输入如下命令，修改 SD 卡中的分区 
	sudo fdisk /dev/sdb 
	返回：
	Command (m for help): 
	输入 d 并回车，删除所有分区。返回：
	Selected partition 1 
	Command (m for help): 
	输入 w 并回车，保存所有已经修改的分区信息。
	返回： The partition table has been altered! 
	Calling ioctl() to re-read partition table. 
	Syncing disks. 
	拨掉 SD 卡，再插入 PC 机上，输入如下命令查看现有的设备节点： 
	cat /proc/partitions 
	返回：
	major minor #blocks name 
	8 0 36700160 sda 
	8 1 512000 sda1 
	8 2 36187136 sda2 
	253 0 34144256 dm-0 
	253 1 2031616 dm-1 
	8 16 3879936 sdb 
	至此，SD 卡原分区/dev/sdb1 被删除。
	4、使用 gparted 工具给 SD 卡预留 256M 空间，用于存放 uboot 映像。使用如下命令安装 gparted 工具：
	sudo apt-get install gparted 
	使用如下命令使用 gparted 打开 SD 卡分区表： 
	sudo gparted /dev/sdb 
	弹出：
	5、选择分区->新建，预留 256M 空间给 uboot，剩下的分区使用 fat32 格式，设置如下图所示， 设置完成后点击添加，选择菜单中的应用全部操作，完成 SD 卡的分区。
	6、进入映像生成目录 out/release，执行如下指令烧写 uboot 到 SD 卡：（注意，这里/dev/sdb 为 SD 卡的节点，可查询节点名称后再执行下面的烧写脚本。） 
	sudo ./x6818-sdmmc.sh /dev/sdb ubootpak.bin 
	返回如图：
	这时，该 SD 卡就可以引导开发板启动 uboot 了。
	使用 SD 启动卡烧写 Linux 映像 
	1、使用读卡器连接已经制作好的 SD 启动卡与电脑，SD 启动卡的制作方法请参考上文 《Linux 下制作 SD 启动卡》或《Windows 下制作 SD 启动卡》。 
	2、在SD卡中，新建名为gec-Linux的文件夹，把编译好的的目标文件ubootpak.bin、boot.img、 qt-rootfs.img、env.txt 四个文件复制并粘贴到该目录下，如图：
	3、env.txt 为传给 uboot 的启动参数，他记录了硬件模块的名字，以及系统启动时文件加载 的地址。env.txt 内容如下： **************************linux3.4.39 & qt5.4 **************************** bootcmd=ext4load mmc 2:1 0x48000000 uImage;bootm 0x48000000 bootargs=lcd=at070tn92 tp=gslx680-linux root=/dev/mmcblk0p2 rw rootfstype=ext4 ubootpak=1 boot=1 system=1 userdata=0 cache=0
	使用 SD 启动卡烧写 Android 映像 1、使用读卡器连接已经制作好的 SD 启动卡与电脑，SD 启动卡的制作方法请参考上文 《Linux 下制作 SD 启动卡》或《Windows 下制作 SD 启动卡》。 
	2、在 SD 卡中，新建名为 GEC-android 的文件夹，把编译的目标文件生成目录 out/release 中的 ubootpak.bin,boot.img，system.img，userdata.img 以及 cache.img，env.txt 六个文件复制并 粘贴到该目录下，如图：
	3、env.txt 为传给 uboot 的启动参数，他记录了硬件模块的名字，以及系统启动时文件加载 的地址。env.txt 内容如下： bootcmd=ext4load mmc 2:1 0x48000000 uImage;ext4load mmc 2:1 0x49000000 root.img.gz;bootm 0x48000000 bootargs=lcd=at070tn92 tp=ft5x06 cam=OV5645 
	4、将 SD 卡插到开发板的 SD0 卡槽，打开开发板电源，这时系统将会自动升级。等待数秒 后，开发板会自动进行烧写。烧写完毕将会自动重启，进入 android 系统。 
	说明：默认 uboot 会自动判断 SD 卡的 GEC-android 目录中的映像文件是否有更新，一旦发 现映像和烧写到开发板上的不同，则会自动更新映像，否则不会再次更新；如果需要强制进入 升级模式，开机上电时按住返回键（K2，调试串口中会打印 force update）即可重新进入 SD 卡烧写模式，开始自动烧写。
	4、将 SD 卡插到开发板的 SD0 卡槽，打开开发板电源，这时系统将会自动升级。等待数秒 后，开发板会自动进行烧写。烧写完毕将会自动重启，进入 Linux 系统。 
	说明：默认 uboot 会自动判断 SD 卡的 gec-Linux 目录中的映像文件是否有更新，一旦发现 映像和烧写到开发板上的不同，则会自动更新映像，否则不会再次更新；如果需要强制进入升级模式，开机上电时按住返回键（K2，调试串口中会打印 force update）即可重新进入 SD 卡 烧写模式，开始自动烧写。

	17:04 2020/7/30
	file 文件名 查看文件的格式  ELF是可执行文件的格式

		1. linux下常见压缩格式:   
			○ .gz -- gzip
			○ .bz2 - bzip2
		2. 常用压缩命令:
			○ tar - 打包
				§ 参数：
					□ c - 创建压缩文件
					□ x - 释放压缩文件
					□ v - 打印提示信息（可不写） 在终端打印
					□ f - 指定压缩包的名字
					□ z - 使用gzip压缩文件 - xxx.tar.gz
					□ j - 使用bzip2的方式压缩文件 -- xxx.tar.bz2
					□ 这时每个人都遵循的一种模式，但是这不是必须的
				§ 压缩：
					□ tar 参数 压缩包的名字 原材料 -- gz
						® tar zcvf test.tar.gz file dir  原材料有一个就写一个  有多个就写多个
				§ 解压缩
						® tar zxvf test.tar.gz -C 解压目录
			○ rar 
				§ rar需要安装
					□ sudo apt-get install rar
				§ 压缩：
				rar a 压缩包名（不用指定后缀） 压缩内容   a代表压缩  压缩内容其实也就是原材料
					□ 压缩目录加参数 -r
				§ 解压缩：
				rar x 压缩包名 解压目录  x是释放的意思 
			○ zip/unzip
				§ 压缩：
				zip 参数 压缩包名 原材料
					□ 如果有目录： -r
				§ 解压缩：
				unzip 压缩包的名字 -d 解压目录
				
		3. 总结
			○ 压缩：
			tar/rar/zip 参数 压缩包名  原材料
			○ 解压缩
			tar/rar/unzip 参数 压缩包名 参数 解压路径
				§ rar 解压缩到指定目录不需要指定参数
				§ unzip 不需要解压参数

	19:27 2020/7/30
	背景：虚拟机ubuntu18.04桥接模式下，配置静态ip；
	1、配置静态ip：
	vim /etc/network/interfaces
	具体配置如下：
	auto lo
	iface lo inet loopback

	auto ens33
	iface ens33 inet static      //dhcp
	address 192.168.123.xxx # 与物理主机的IP最后这组要不一样
	gateway 192.168.123.xxx# 与物理主机相同的网关
	netmask 255.255.255.0 # 物理主机一样的DNS

	问题1：，不能ping通主机
	注意点：网关必须和主机保持一致；主机防火墙关闭；
	网络模式选择桥接模式后，不选复制物理网络连接状态。
	问题2：无法联网(虚拟机和主机之间能互相ping通；并且能ping通8.8.8.8，但是无法ping通baidu.com)
	解决方案：ping 命令是属于ICMP协议，ping ip地址有效。若直接ping网址（域名），需要配置DNS。
	所以编辑添加nameserver如下：

	2、修改DNS
	vi /etc/systemd/resolved.conf
	{vi /etc/resolv.conf (Ubuntu早期版本)}
	DNS= 8.8.8.8

	PS:设置IP的方式
	（1）ifconfig 或ip addr
	（2）nmtui //调出图形接口
	（3）vim /etc/network/interfaces
	重启network服务：systemctl restart network
	service network-manager restart
	22:22 2020/7/30
	交叉编译是在一个平台上生成另一个平台上的可执行代码。同一个体系结构可以运行不同的操作系统；同样，同一个操作系统也可以在不同的体系结构上运行。
	gcc和arm-linux-gcc 用的是不同的指令集，交叉编译是用RSIC(精简指令集) ，它们编译生成的可执行文件只被对应的环境识别，gcc是linux系统下用来将代码编译成一个可执行程序的方法。编译出来的是适用于linux系统的可执行二进制文件，arm-linux-gcc交叉编译是告诉编译器，编辑的环境是linux，但是生成的是arm识别的可执行文件。
	arm-linux-gcc是一种在linux环境编写，生成在arm上运行的可执行程序的编译方式，这就是交叉编译，编写环境和运行环境分离的一种手段。而不同的系统机器码含义不同，所以arm-linux-gcc编译出来的机器码无法在Ubuntu上执行，只能在arm平台上执行。
	gcc是x86架构指令用的，arm-linux-gcc 是RSIC(精简指令集) ARM架构上面用的，它们会把c源码编译成不同的汇编指令然后生产不同平台的二进制可执行的文件，gcc是linux系统下bai面用来将代码编译成一个可执行程序的手段。编译出来的是适用于linux系统的可执行二进制文件，arm-linux-gcc就是告诉编译器，我编写的环境是linux，但是我希望生成的可执行程序是在arm上面跑的。这就是交叉编译。编写环境和执行环境分离的一种手段。
	gcc是linux系统下面用来将代码编译成一个可执行程序的手段。编译出来的是适用于linux系统的可执行二进制文件。可执行程序其实就是一堆的0101二进制机器码。这些机器码代表什么含义只有机器本身能理解。
	而arm-linux-gcc就是告诉编译器，我编写的环境是linux，但是我希望生成的可执行程序是在arm上面跑的。这就是交叉编译，编写环境和执行环境分离的一种手段。
	minicom是一个串口通信工具，就像Windows下的超级终端。
	可用来与串口设备通信，如调试交换机和Modem等。它的Debian软件包的名称就叫minicom，用apt-get install minicom即可下载安装。
	安装完成后，请不要着急打开软件。需先进行配置。具体步骤如下：
	1.linux下的所有操作面向用户的都是文件操作，在对串口操作之前，我们应该先确认自己对该文件有没有读写权限。
	ls -l /dev/ttyUSB*
	linux下的usb串口命名为ttyUSB，运行上面命令，可以看到有几个设备挂载。
	我们这里是：
	crw-rw---- 1 root dialout 188, 0 Apr 10 17:10 /dev/ttyUSB0
	只有ttuUSB0.
	再用lsusb查看：
	Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
	Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
	Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
	usb 004正是我们挂上去的usb转串口线缆，使用的芯片是PL2303。
	但是正如上面显示，ttyUSB0这个设备是root所有的，所以，我们以普通用户身份打开minicom是没法访问该文件的。
	配置minicom的参数
	运行$ sudo minicom -s

	便进入了minicom的配置界面，使用上下键选择Serial port setup，回车。此时光标在“change which setting”后面停留，它的上面有如下菜单：
	Serial port setup [Enter]
	+-------------------------------------------------------------+
	| A - Serial Device : /dev/ttyUSB0 |
	| B - Lockfile Location : /var/lock |
	| C - Callin Program : |
	| D - Callout Program -: |
	| E - Bps/Par/Bits : 115200 8N1 |
	| F - Hardware Flow Control : No |
	| G - Software Flow Control : No |
	| |
	| Change which setting? |
	+-------------------------------------------------------------+
	我们只需输入上面对应的字母，就可以进如相应的菜单进行设置。设置完成，回车，光标会回到“change which setting”后面，如此重复。完成按回车返回主菜单即可。
	注意：如果沒有使用USB转串口，而是直接使用串口，那么Serial Device要配置为/dev/ttyS0。

	返回主菜单后，选择“Save setup as df1”，将其保存为默认设置，然后选择 Exit推出。需退出后重新打开minicom，软件才会使用上述参数进行初始化。

	minicom使用
	如果上面设置顺利，打开minicom
	sudo minicom
	重新给开发板上电后，此时，窗口里就有信息打印出来了。
	minicom基本操作如下：
	1）需使用Ctrl+a 进入设置状态
	2）按z进入设置菜单
	（1）S键：发送文件到目标系统中；
	（2）W键：自动卷屏。当显示的内容超过一行之後，自动将後面的内容换行。这个功能在查看内核的啓动信息时很有用。
	（3）C键：清除屏幕的显示内容；
	（4）B键：浏览minicom的历史显示；
	（5）X键：退出mInicom，会提示确认退出。
	8:29 2020/7/31
	交叉编译是在一个平台上生成另一个平台上的可执行代码。同一个体系结构可以运行不同的操作系统；同样，同一个操作系统也可以在不同的体系结构上运行。
	gcc和arm-linux-gcc 用的是不同的指令集，交叉编译是用RSIC(精简指令集) ，它们编译生成的可执行文件只被对应的环境识别，gcc是linux系统下用来将代码编译成一个可执行程序的方法。编译出来的是适用于linux系统的可执行二进制文件，arm-linux-gcc交叉编译是告诉编译器，编辑的环境是linux，但是生成的是arm识别的可执行文件。
	arm-linux-gcc是一种在linux环境编写，生成在arm上运行的可执行程序的编译方式，这就是交叉编译，编写环境和运行环境分离的一种手段。而不同的系统机器码含义不同，所以arm-linux-gcc编译出来的机器码无法在Ubuntu上执行，只能在arm平台上执行。
	gcc是x86架构指令用的，arm-linux-gcc 是RSIC(精简指令集) ARM架构上面用的，它们会把c源码编译成不同的汇编指令然后生产不同平台的二进制可执行的文件，gcc是linux系统下bai面用来将代码编译成一个可执行程序的手段。编译出来的是适用于linux系统的可执行二进制文件，arm-linux-gcc就是告诉编译器，我编写的环境是linux，但是我希望生成的可执行程序是在arm上面跑的。这就是交叉编译。编写环境和执行环境分离的一种手段。
	gcc是linux系统下面用来将代码编译成一个可执行程序的手段。编译出来的是适用于linux系统的可执行二进制文件。可执行程序其实就是一堆的0101二进制机器码。这些机器码代表什么含义只有机器本身能理解。
	而arm-linux-gcc就是告诉编译器，我编写的环境是linux，但是我希望生成的可执行程序是在arm上面跑的。这就是交叉编译，编写环境和执行环境分离的一种手段。
	minicom是一个串口通信工具，就像Windows下的超级终端。
	可用来与串口设备通信，如调试交换机和Modem等。它的Debian软件包的名称就叫minicom，用apt-get install minicom即可下载安装。
	安装完成后，请不要着急打开软件。需先进行配置。具体步骤如下：
	1.linux下的所有操作面向用户的都是文件操作，在对串口操作之前，我们应该先确认自己对该文件有没有读写权限。
	ls -l /dev/ttyUSB*
	linux下的usb串口命名为ttyUSB，运行上面命令，可以看到有几个设备挂载。
	我们这里是：
	crw-rw---- 1 root dialout 188, 0 Apr 10 17:10 /dev/ttyUSB0
	只有ttuUSB0.
	再用lsusb查看：
	Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
	Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
	Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
	usb 004正是我们挂上去的usb转串口线缆，使用的芯片是PL2303。
	但是正如上面显示，ttyUSB0这个设备是root所有的，所以，我们以普通用户身份打开minicom是没法访问该文件的。
	配置minicom的参数
	运行$ sudo minicom -s

	便进入了minicom的配置界面，使用上下键选择Serial port setup，回车。此时光标在“change which setting”后面停留，它的上面有如下菜单：
	Serial port setup [Enter]
	+-------------------------------------------------------------+
	| A - Serial Device : /dev/ttyUSB0 |
	| B - Lockfile Location : /var/lock |
	| C - Callin Program : |
	| D - Callout Program -: |
	| E - Bps/Par/Bits : 115200 8N1 |
	| F - Hardware Flow Control : No |
	| G - Software Flow Control : No |
	| |
	| Change which setting? |
	+-------------------------------------------------------------+
	我们只需输入上面对应的字母，就可以进如相应的菜单进行设置。设置完成，回车，光标会回到“change which setting”后面，如此重复。完成按回车返回主菜单即可。
	注意：如果沒有使用USB转串口，而是直接使用串口，那么Serial Device要配置为/dev/ttyS0。

	返回主菜单后，选择“Save setup as df1”，将其保存为默认设置，然后选择 Exit推出。需退出后重新打开minicom，软件才会使用上述参数进行初始化。

	minicom使用
	如果上面设置顺利，打开minicom
	sudo minicom
	重新给开发板上电后，此时，窗口里就有信息打印出来了。
	minicom基本操作如下：
	1）需使用Ctrl+a 进入设置状态
	2）按z进入设置菜单
	（1）S键：发送文件到目标系统中；
	（2）W键：自动卷屏。当显示的内容超过一行之後，自动将後面的内容换行。这个功能在查看内核的啓动信息时很有用。
	（3）C键：清除屏幕的显示内容；
	（4）B键：浏览minicom的历史显示；
	（5）X键：退出mInicom，会提示确认退出。

	重启网络服务：

	      sudo /etc/init.d/networking force-reload  ==> 重新加载网路配置文件

	      sudo /etc/init.d/networking restart

	如果遇到输入arm-linux-gcc -v后弹出-bash:arm-linux-gcc -v: 没有那个文件或目录这种情况为ubuntu没有安装32位的lib库导致，安装32位lib库后即可解决这个问题
	apt-get install lib32z1

	apt-get install ia32-libs

	apt-get install lib32ncurses5 lib32z1

	apt-get install lib32stdc++6

	apt-get install lib32z1

	Ubuntu的CPU是因特尔 intel  串口协议  CPU架构
	rx 文件名相同权限只修改一次  rz每次都要修改
	U盘拷贝 需要先查看文件系统  开发板USB接口只支持识别FAT32的文件系统

	一、如何在ARM架构上运行C程序

		1）用交叉编译工具链编译源码
			
			为什么需要换编译器：
				因为主机的CPU架构不同，导致了其指令集（CPU在运行程序的行为规范）不同。
				
				ubuntu:
						64:X64    32:X86
						
				开发板：ARM-Cortex A53
				
		2）如何烧写（下载）程序到开发板
		
		第一种：rx
			1：使用烧写工具rx  (arm-linux-gcc rx.c -o rx)
			
				在CRT输入烧写命令：
									rx 文件名字
									
				输入之后有两种文件的方式：
										1)直接鼠标拖拽要下载的文件按到CRT窗口内，xmodem
										2)点击传输，选择xmodem，在弹出窗口内选择要下载的文件
										
				设置默认的下载上传路径：点击选项 -->回话选择进行选择
									
									
			2：修改程序的权限后才可以运行程序
			
				在CRT里面输入修改权限的命令：
				
											chmod 777 文件名字
											
											
											chmod a+wex 文件的名字
											chmod a=wex 文件的名字
											
		第二种:rz
			
				先用rx把rz烧写到开发板
				
					输入命令：rx rz
					
				修改rz的权限：chmod 777 rz
				
				把rz移动固定存放在二进制的系统目录内/usr/bin
				
				mv rz /usr/bin
				
				
				使用rz下载：rz -y
				-y:覆盖更新
				
		
		第三种：
				U盘拷贝
				
					注意：开发板USB接口支持识别FAT32的文件系统
				
				第一步：把要拷贝的数据拷到U盘
				
				第二步：在CTR里面/mnt/udisk
					


	armv71是32位的系统 CPU64位
	armv8以后的版本是64位的系统 CPU64位
	库的作用 专门打包代码段的  文件  库文件  API：函数 链接库生成可执行文件  静态库后缀名.a   gcc:main 调用库
	编译链接生成程序时使用库文件   在使用静态库编译生成程序时，是根据引用出拷贝对应的代码段  你如果用的是静态库那么这个可执行程序就变得很大了因为它是直接替换代码段 在
	库的作用：专门打包代码段(API + 头文件 + 
	编译时链接别人的库就行  编译出来的库文件也是属于二进制的
	静态库：在链接静态库编译生成程序时，把库中的代码段拷贝到引用处
	注意：生成的二进制文件会变大
	程序不需要带着静态库放在目标主机内  因为二进制文件里面已经有了这个库文件中的代码段了
	静态库的编译与制作：
	静态库都是从.o文件开始编译制作的  
	1先用gcc生成.o文件 (没有链接库文件的二进制文件)
	gcc -c xx.c -o xx.o
	2使用ar工具编译生成.a库文件
	ar -rcs 库的名字 .o文件 r：把代码段插入库中 c：创建库文件(备份文件) s：如果库中有基于对象的，则是生成对象对应的符号表(变量 类型)  符号表：统计变量的类型 S：不生成符号表
	库文件里面只存放要封装的函数API 不需要main函数  因为这是给别人使用的  
	编译 gcc xx.c xxx.a -o xx  注意：声明需要在main函数中声明 gcc 需要调用库的.c文件 库文件 生成可执行文件的名称
	使用静态库是在调用处直接把代码段拷贝过去因此编译出来的可执行文件会比较大、

	占位符 需要告诉编译器库的名字和路径  使用动态库是在调用处生成占位符然后根据占位符去找到代码段  所以在编译时程序需要带着动态库放在目标主机内 因为它是根据占位符去引用库中的代码段的

	minicom不稳定 使用不稳定
	数据结构 链表 Linux内核链表  动态内存 链表 二叉树   
	数据结构 控制数据的存放形式 优化内存 实现访问数据的效率

	/lib/x86_64-linux-gnu/libc.so.6 -> libc-2.27.so
	9:09 2020/8/1
	Linux自有服务是指 在不去安装其它软件的同时它自己这个命令我们就能直接用了  那就可以认为其实这个服务呢是系统内置的
	自有服务，即不需要用户独立去安装的软件的服务，而是当系统安装好之后就可以直接使用的服务(内置)。
	运行模式也可以称之为运行级别。

	在linux中存在一个进程：init （initialize，初始化），进程id是1。 那它其实呢是我们这台计算机启动之后第一个运行的进程 它是一个进程那它运行的时候可能会需要一系列的信息
	查看进程：#ps -ef|grep init
	该进程存在一个对应的配置文件：inittab（系统运行级别配置文件，位置/etc/inittab）  init进程在执行的时候会读取这里面的信息

	文件的主要内容：
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
	#init 6 		重启电脑
	注意：init指令需要超级管理员的权限，普通用户无法执行。

	这些命令其实都是调用的init进程，将数字（运行级别）传递给进程，进程去读配置文件执行对应的操作。

	①切换到纯命令行模式下（临时切换，重启之后又恢复）
	#init 3
	切换之后需要输入用户名和密码，在输入密码的时候没有“*”提示输入，只要自己确认输入的密码没有错误，按下回车即可。

	②回到桌面模式
	#init 5

	③设置模式永久为命令行模式
	将/etc/inittab文件中的initdefault值设置成3，然后重启操作系统。

	ubuntu18.04 ssh 远程系统拒绝连接 解决方法
	错误提示是这个： The remote system refused the connection.

	 

	原因是 Ubuntu 没安装  一个软件,

	 

	废话不多说 ，上解决方法：

	执行该条命令,安装 ,安装完会自动启动 ：sudo apt install openssh-server -y

	提示1 ： 如果出现 ” 无法打开锁 “ 类似的提示 可以重启系统 ，切换到 root 用户再次执行 

	也可以直接切换到 root 用户尝试

	提示 2：如果还是链接不上 请 用 root 权限在终端输入 ufw status 查询防火墙是否开启 如开启 输入 ： ufw disable 命令关闭防火墙即可链接

	root连接ubuntu18.04“拒绝访问”的解决方法

	1、设置root账户  

	1
	sudo passwd root
	2、ssh远程登陆拒绝访问：修改SSH配置文件  

	1
	sudo vim /etc/ssh/sshd_config
	找到并用#注释掉这行：PermitRootLogin prohibit-password   // 允许root登录，但是禁止root用密码登录（默认值在文件中是被注释掉的）

	新建一行 添加：PermitRootLogin yes   //允许root登录，设为yes。

	重启服务
	#sudo service ssh restart
	1、设置root账户  

	1
	sudo passwd root
	2、ssh远程登陆拒绝访问：修改SSH配置文件  

	1
	sudo vim /etc/ssh/sshd_config
	找到并用#注释掉这行：PermitRootLogin prohibit-password   // 允许root登录，但是禁止root用密码登录（默认值在文件中是被注释掉的）

	新建一行 添加：PermitRootLogin yes   //允许root登录，设为yes。

	重启服务
	#sudo service ssh restart

	11:42 2020/8/1
	nfs network file system 网络文件系统  (没网)
	uboot系统引导--->内核源码--->文件系统
	14:41 2020/8/1
	nfs network file system 网络文件系统  (没网)
	init 3 完整多用户  你如果切换到完整多用户模式下之后 注意啊这个时候呢是不带桌面的 也就是纯命令行模式
	init 5 切换到图片界面
	init 6 重启电脑
	这些命令其实都是调用的init进程，数字(运行级别)传递给进程，进程去读取这个配置文件执行对应的操作
	注意：init指令需要超级管理员的权限，普通用户无法执行。大部分命令都需要超级管理员权限
	切换之后需要输入用户名和密码，在输入密码的时候没有“*”提示输入，只要自己确认输入的密码没有错误，按下回车即可。
	tty是终端设备 是终端设备的一个编号
	用户与用户组管理（重点）
	Linux系统是一个多用户多任务的操作系统，任何一个要使用系统资源的用户，都必须首先向系统管理员申请一个账号，然后以这个账号的身份进入系统。
	用户的账号一方面可以帮助系统管理员对使用系统的用户进行跟踪，并控制他们对系统资源的访问；另一方面也可以帮助用户组织文件，并为用户提供安全性保护。
	每个用户账号都拥有一个惟一的用户名和各自的密码。
	用户在登录时键入正确的用户名和密码后，就能够进入系统和自己的主目录。
	要想实现用户账号的管理，要完成的工作主要有如下几个方面：

	用户账号的添加、删除、修改以及用户密码的管理。
	用户组的管理。

	注意三个文件： (三个系统自带的配置文件)
	/etc/passwd				存储用户的关键信息
	/etc/group				存储用户组的关键信息
	/etc/shadow				存储用户的密码信息

	用户管理  那用户一开始肯定是除了root之外没有其它用户的 那如果要给xx新建账号那这时候就需要去添加用户了
	①添加用户
	常用语法：#useradd 选项 用户名
	常用选项：
		-g：表示指定用户的用户主组，选项的值可以是用户组的id，也可以是组名
		-G：表示指定用户的用户附加组，选项的值可以是用户组的id，也可以是组名
		-u：uid，用户的id（用户的标识符），系统默认会从500之后按顺序分配uid，如果不想使用系统分配的，可以通过该选项自定义【类似于腾讯QQ的自选靓号情况】
		-c comment：添加注释
	案例：创建用户zhangsan，不带任何选项
	验证是否成功： 当一个用户创建成功后，它有哪些特点
		a. 验证/etc/passwd的最后一行，查看是否有zhangsan的信息；
		b. 验证是否存在家目录（在Centos下创建好用户之后随之产生一个同名家目录）；
	用户组 假设第一组有个id为1 第十六组有个id叫16 那这个-g呢就是表示我在添加用户的时候可以通过-g来指定这个用户它加入到哪个组里面去
	是加入到第一组里面去还是加入到第十六组里面去就是这个意思
	在Linux里面它允许我们一个用户同时属于多个组，可以属于多个组，但是你必须得有一个主组就是你主要属于哪一个组，剩下的都认为是附加的
	扩展：认识passwd文件
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
	在设置密码的时候也是没有任何输入提示的，放心输入，确保两次输入的密码一致，按下回车即可。

	也可以使用弱密码，但是不建议，否则会看到以下的提示：
	设置密码之后shadow文件中的体现：能够看出lisi用户没有密码的。
	在设置用户密码之后可以登录帐号，例如此处需要登录wangerma
	切换用户命令：#su [用户名]	（switch user）
	如果用户名不指定则表示切换到root用户。
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
	注意：已经登录的wangerma用户删除的时候提示删除失败，但是没有登录的lisi用户可以正常删除。

	解决办法：简单粗暴，kill对应用户的全部进程
	提示：所有跟用户操作的命令（除passwd外）只有root超级管理员有权限执行。
	11:21 2020/8/2
	CentOS 添加用户 useradd 不用加上选项-m 它就会在/home/自动创建一个同名家目录
	而Ubuntu 添加用户 useradd 需要加上选项-m 它才会在/home自动创建一个同名家目录
	如果需要指定一个用户有多个附加组那么就需要使用usermod不能再次使用useradd了不然它会告诉你该用户已经存在
	/etc/group下的是附加组的名字不是用户的名字 
	在添加附加组时不要写root 千万不要写0  注意：修改用户名的时候被修改的用户名是放在选项后面的  因为被修改的用户名它属于操作对象所以把它放在后面 新用户名由于是你要更改用户名这个操作的一个可选操作所以呢它是选项的值因为选项的值所以我们放在选项后面也就是紧挨着选项最后呢才加上我们这个被修改的用户名 一旦反了就报错了
	我们创建好的一个用户如果说它没有密码那这个用户是处于被锁定状态的那它是登录不了的所以为了安全你必须得有密码 所以我们要设置密码
	Linux不允许没有密码的用户登录到系统，因此前面创建的用户目前都处于锁定状态，需要设置密码之后才能登录。

	输入sudo minicom -s，注意前边一定要加sudo，否则在咱们配置完后会出现cannot write to /etc/minicom/minirc.dfl的权限问题！
	使用方向键 选择 Serial port setup，按Enter键，进入设置环境，如下图
	输入a或者A，选择串口设备，在这里我使用的是USB转串口，并且我的开发板连接到了COM1上，将/dev/tty8修改为/dev/ttyUSB0
	注意：使用USB转串口，那么串口COM1对应ttyUSB0, COM2对应ttyUSB1；如果没有使用USB转串口，而是直接使用串口，那么串口COM1对应ttyUSB0, COM2对应ttyUSB1。
	配置完串口设备后，按Enter键，再输入E，配置波特率，按默认配置即可  115200 8N1 （波特率：115200，数据位：8，奇偶校验位：N 无，停止位：1）。

	   配置完波特率，按Enter键，再输入F，配置硬件流控，选择NO

	   再继续配置软件流控，也选择NO。都配置完后，按下Enter键返回上一界面，选择save setup as dfl（即将其保存位默认配置），再选择Exit，关闭minicom。

	使用

	   再次输入命令  sudo minicom，是刚才的配置生效，可以看到串口输出信息

	补充说明：这是我在网上看到的，先记下来，以后使用的时候再看。

	   在通过串口用xmodem协议烧写内核时会提示没有xmodem协议，所以还必须安装软件包：lrzsz
	       sudo apt-get install lrzsz
	       这时候就可以正常地用minicom通过串口烧写内核了。

	下次在输入minicon 即可直接进入。
	命令minicom是进入串口超级终端画面，而minicom -s为配置minicom。
	说明/dev/ttyS0 对应为串口0 为你连接开发板的端口。

	注意：非正常关闭minicom，会在/var/lock下创建几个文件LCK*，这几个文件阻止了minicom的运行，将它们删除后即可恢复


	组合键的用法是：先按Ctrl+A组合键，然后松开这两个键，再按Z键。另外还有一些常用的组合键。
	（1）S键：发送文件到目标系统中；
	（2）W键：自动卷屏。当显示的内容超过一行之后，自动将后面的内容换行。这个功能在查看内核的启动信息时很有用。
	（3）C键：清除屏幕的显示内容；
	（4）B键：浏览minicom的历史显示；
	（5）X键：退出mInicom，会提示确认退出。

	3、配置文件所在目录
	Ctrl + A --> O
	+-----[configuration]------+
	| Filenames and paths      |
	| File transfer protocols -|
	| Serial port setup        |
	| Modem and dialing        |
	| Screen and keyboard      |
	| Save setup as dfl        |
	| Save setup as..          |
	| Exit                     |
	+--------------------------+

	选择"Filenames and paths"
	+-----------------------------------------------------------------------+
	| A - Download directory : /home/crliu                                    |
	| B - Upload directory   : /tmp                                         |
	| C - Script directory   :                                              |
	| D - Script program     : runscript                                    |
	| E - Kermit program     :                                              |
	| F - Logging options                                                   |
	|                                                                       |
	|    Change which setting?                                              |
	+-----------------------------------------------------------------------+

	（1）A - download 下载文件的存放位置（开发板 ---> PC）
	开发板上的文件将被传输到PC机上的/home/crliu目录下。
	（2）B - upload 从此处读取上传的文件（PC ---> 开发板）
	PC机向开发板发送文件，需要发送的文件在/tmp目录下（PC机上的目录）。做了此项配置后，每次向开发板发送文件时，只需输入文件名即可，无需输入文件所在目录的绝对路径。

	打开终端，输入安装命令：
	        sudo apt-get install minicom

	安装好后打开minicom
	        sudo minicom
	打开后进入欢迎界面，最下面有一行提示，表示同时按下ctrl-A，之后松开再按下Z会进入帮助页面，不区分大小写。
	进入帮助页面，会列出各种命令的快捷键
	在帮助页面可以看到配置minicom的快捷键是o，此时按下o会进入配置界面，也可以在欢迎界面或使用过程中按下ctrl-a o的方式打开。
	按上下键选择Serial-port-setup（串口配置），进入串口配置界面,此时按下左边对应的字母可以使光标跳转到对应的条目，可以对配置进行修改，修改后如果要保存，需要按下enter，如果不保存，按下esc。设置好后按下enter或esc退出界面。
	选择save setup as dfl，可以将刚刚的修改作为默认配置，以后启动都按照这个配置启动。
	按下esc退出配置界面，此时如果串口有数据便会显示出来
	按下Ctrl+a n可以显示时间戳，常用的是simple的时间戳，再按一次显示extended的时间戳，前者只显示到秒，后者精确到毫秒。
	按下ctrl-a x 或者ctrl-a q可以退出minicom

	9:34 2020/8/3
	Makefile可以节省编译的时间
	数据结构是用来控制和组织数据的一种方式
	数据结构概念：编写关于数据结构的程序 来控制系统(CPU)来控制组织数据的(读、写、存储)的方式，具有较好的运行，存储效率。
	数据结构基本分类：线性结构：
			  	顺序结构(数组、字符串、顺序表、栈(不是内存分布当中的那个栈)、队列)
			  	链状结构：链表(单向(循环、不循环)、双向(循环、不循环)、内核(双向循环))、栈、队列
			  非线性结构：
				树(二叉树)、图
	\0 NULL的区别 1、NULL; NULL 即空指针，在C和C++中的形式不一样，NULL 在c中用（void*）0表示，在c++中用0表示。
	2、'\0':'\0'表示字符串结束，它在ASCII中的值为0（数值0，非字符‘0’）
	所以在数值上NULL,'\0',0是一样的，都是0，但'0'就不同了，在ASCII码中编码为48，所以字符0和上述三个值不同。
	在内存中NULL 和'\0' 和'0'都是一个8位的char类型，NULL 和'\0' 值一样，都是0，以数字方式读取就是0，以字符串读取时就是'\0'或者null（和编译器有关），而‘0’在内存存储着48，以字符读取就是'0',以数字读取就是48，至于0，可能是char ,int ,float,double等类型，但是值和NULL和'\0'一样，都是0

	C/C++语言中NULL、'\0’和0的区别
	 

	    注：本文参考了http://blog.csdn.net/mylinx/article/details/6873253及书籍《征服C指针》（[日]前桥和弥著）。

	    NULL、'\0'和0的值是一样的，都是0，不过它们的表现形式不一样：

	    1. NULL: 即空指针，不过在C和C++中并不一样。在VS 2013的库文件string.h中可以看到如果定义。

	复制代码
	1 /* Define NULL pointer value */
	2 #ifndef NULL
	3 #ifdef __cplusplus
	4 #define NULL    0
	5 #else  /* __cplusplus */
	6 #define NULL    ((void *)0)
	7 #endif  /* __cplusplus */
	8 #endif  /* NULL */
	复制代码
	    可以看出，在C中，NULL表示的是指向0的指针，而在C++中，NULL就直接跟0一样了。但有一点值得注意的是：在C语言中，“当常量0处于应该作为指针使用的上下文中时，它就作为空指针使用”（《征服C指针》）。例如，下边的指针定义和初始化是没问题的（即没警告也没报错）：

	 int * p = 0;    /* C language */
	     但如果定义成如下的样子呢？

	 int * p = 3;    /* C language */
	　　很明显，这样子做是有问题的。这一句可以编译通过，但在VS 2013中有这样的警告：“warning C4047: “初始化”:“int *”与“int”的间接级别不同”。

	　　我又试了一下这一句在C++中的情况，VS 2013就直接报错了：“ ‘int’ 类型的值不能用于初始化 ‘int *’ 类型的实体”。

	　　因此，为了防止混淆，在C/C++中，当要将一个指针赋值为空指针的时候，都应该将它赋为NULL，而不是0。

	　　

	　　2. ‘\0’：‘\0’是一个“空字符”常量，它表示一个字符串的结束，它的ASCII码值为0。注意它与空格' '（ASCII码值为32）及'0'（ASCII码值为48）不一样的。

	　　在《征服C指针》中，作者还提到了一种错误的程序写法：使用NULL来结束字符串。例如下边的程序就是有问题的：

	char str[4] = { '1', '2', '3', NULL };    /* C language */
	　　在VS 2013中，会的这样的警告：“warning C4047: “初始化”:“char”与“void *”的间接级别不同”。而在C++中，这一句是没有问题的。

	　　还有一点值得注意，如下的程序在C/C++中都是没有问题的：

	char str[4] = { '1', '2', '3', 0 };    / C/C++ language */
	　　但为了防止混淆，在C/C++中，当要给一个字符串添加结束标志时，都应该用‘\0’而不是NULL或0。

	 

	　　综上所述，当我们要置一个指针为空时，应该用NULL，当我们要给一个字符串添加结束标志时，应该用‘\0’。


## 省钱利器：
![](/hexo-private-blog-website/images/淘宝客11.jpg)
![](/hexo-private-blog-website/images/淘宝客12.jpg)
![](/hexo-private-blog-website/images/淘宝客13.jpg)

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