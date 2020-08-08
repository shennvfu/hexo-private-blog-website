---
title: Linux_Basics_02
date: 2020-08-02 10:35:18
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

云想衣裳花想容，春风拂槛露华浓。
若非群玉山头见，会向瑶台月下逢。

这是一个链接 [菜鸟教程][Runoob]

[Runoob]: http://www.runoob.com/

百度 [百度][Baidu]

[Baidu]: http://www.baidu.com

谷歌 [Google][Google]

[Google]: http://google.com.hk

# 1.文本文档编辑命令
	
	1.1 cat  显示文本内的数据
		cat -n
		
	1.2 > 重定向
	
		例子 ：ls > a.txt
				把ls输出的信息重定向到a.txt内
				kill -l > a.txt
				
				echo hello >a.txt
				
		补充：>1   >2
		
	1.3
		head :显示文件首部的数据，（默认显示前10行）
		
		q:不显示文件的标题（文件名）
		v:显示文件的标题（文件名）
		
		
	1.4 tail : 显示文件尾部的数据，（默认显示后10行）
	
	
	1.5 more 
		more 文档名字
		
		进入输入h显示帮助页面
		
	1.6 wc
	-c:统计字符数
	-w：统计词数
	-l：行数
	
## 2.压缩解压和搜索命令

	1）xz
	
		压缩：
			先把目标压缩成tar格式
			
			tar cf 压缩包的名字.tar. 目标
			
			把tar转成xz格式
			
			xz  -z 压缩包的名字.tar.
			
			z:强制压缩
			d:强制解压
			
			
		解压：
			  把xz -- tar
			  再解压tar 
			  
### 3.压缩解压和搜索命令

	3.1)find
		find -name 文件名字
		find -iname 文件名字 
		
		
		i:忽略大小写
		
		例子：在根目录中搜索string.h
		
			find  / -name string.h
			find  -name string.h
			find  -name *.mp3
			
	3.2)grep 
	
		例子：
			在hello.c里面搜索#include 
		
		grep "#include" hello.c -n 
		
		在根目录搜索所有头文件里面是否有#include
		
		find -name *.h | grep "#include" -n -r
		
		
		-n:显示行号
		-r:连同子目录
			
		
		/usr/include
		/use/编译器
		
		
#### 4.ubuntu上网的问题	

	4.1：
		安装vmwared是否没有安装虚拟机的网络适配器（看windows看里面的适配器）
		
		先点击--》编辑 --》虚拟网络编辑器 -->点击重新配置
		
		没有就重装vmware
		
	4.2： 
		更新软件源
		
		上网找最新域名（最新的源）复制到一个配置文件
		sudo vim /etc/apt/sources.list
	4.3:检测局域网是否连通（windows和Linux）
	
	先获取两个主机的IP
		ipconfig  widnows
		ifconfig  linux
		
		ping xxx.xxx.xxx.xxx
		
	阿里软件源：
	
	# deb cdrom:[Ubuntu 16.04 LTS _Xenial Xerus_ - Release amd64 (20160420.1)]/ xenial main restricted
	deb-src http://archive.ubuntu.com/ubuntu xenial main restricted #Added by software-properties
	deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted
	deb-src http://mirrors.aliyun.com/ubuntu/ xenial main restricted multiverse universe #Added by software-properties
	deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted
	deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted multiverse universe #Added by software-properties
	deb http://mirrors.aliyun.com/ubuntu/ xenial universe
	deb http://mirrors.aliyun.com/ubuntu/ xenial-updates universe
	deb http://mirrors.aliyun.com/ubuntu/ xenial multiverse
	deb http://mirrors.aliyun.com/ubuntu/ xenial-updates multiverse
	deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse
	deb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse #Added by software-properties
	deb http://archive.canonical.com/ubuntu xenial partner
	deb-src http://archive.canonical.com/ubuntu xenial partner
	deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted
	deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted multiverse universe #Added by software-properties
	deb http://mirrors.aliyun.com/ubuntu/ xenial-security universe
	deb http://mirrors.aliyun.com/ubuntu/ xenial-security multiverse
	
	
	国内有很多Ubuntu的镜像源，包括阿里的、网易的，还有很多教育网的源，比如：清华源、中科大源。
	我们这里以清华源为例讲解如何修改Ubuntu 18.04里面默认的源。

	1、输入命令修改sources.list文件，当然需要超级权限，所以要加sudo；

	sudo gedit /etc/apt/sources.list
	1
	编辑/etc/apt/sources.list文件

	2、在文件最前面添加以下条目(操作前请做好相应备份)：

	deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
	deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
	deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
	deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
	deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
	deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
	deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
	deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
	deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
	deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse

	3、修改完成后，保存文件，警告什么的都不理，然后运行下面的命令。

	sudo apt-get update
	sudo apt-get upgrade
	1
	2
	4、到此完成国内源更新。

	附加其他源内容，需要的自己添加进sources.list文件里面就ok了。

	阿里源：

	deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
	deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
	deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
	deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
	deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
	deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
	deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
	deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
	deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
	deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse


	中科大

	deb https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
	deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
	deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
	deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
	deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
	deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
	deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
	deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
	deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
	deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse

	163源

	deb http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
	deb http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
	deb http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
	deb http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
	deb http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse
	deb-src http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
	deb-src http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
	deb-src http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
	deb-src http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
	deb-src http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse

	Ubuntu 18.04 在系统设置里面是找不到软件源设置界面按钮的（找了好久都没找到T-T），所以需要一些指令帮助启动图形界面以便设置软件源。
	1、输入命令

	sudo update-manager -c -d
	1
	2、会弹出Software Updater提示框，这就好办了，点击左下角Settings…按钮，久违的软件源设置就会出来啦。

	国内几个主要的ubuntu 18.04 软件源

	1、阿里源

	deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
	deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
	deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
	deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
	deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
	deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
	deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
	deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
	deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
	deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse

	2、网易源

	deb http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
	deb http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
	deb http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
	deb http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
	deb http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse
	deb-src http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
	deb-src http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
	deb-src http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
	deb-src http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
	deb-src http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse

	3、中科大源

	deb https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
	deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
	deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
	deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
	deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
	deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
	deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
	deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
	deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
	deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse

	4、清华源

	deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
	deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
	deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
	deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
	deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
	deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
	deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
	deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
	deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
	deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse

	5、浙大源

	deb http://mirrors.zju.edu.cn/ubuntu/ bionic main multiverse restricted universe
	deb http://mirrors.zju.edu.cn/ubuntu/ bionic-backports main multiverse restricted universe
	deb http://mirrors.zju.edu.cn/ubuntu/ bionic-proposed main multiverse restricted universe
	deb http://mirrors.zju.edu.cn/ubuntu/ bionic-security main multiverse restricted universe
	deb http://mirrors.zju.edu.cn/ubuntu/ bionic-updates main multiverse restricted universe
	deb-src http://mirrors.zju.edu.cn/ubuntu/ bionic main multiverse restricted universe
	deb-src http://mirrors.zju.edu.cn/ubuntu/ bionic-backports main multiverse restricted universe
	deb-src http://mirrors.zju.edu.cn/ubuntu/ bionic-proposed main multiverse restricted universe
	deb-src http://mirrors.zju.edu.cn/ubuntu/ bionic-security main multiverse restricted universe
	deb-src http://mirrors.zju.edu.cn/ubuntu/ bionic-updates main multiverse restricted universe

	1、查看当前系统的codename

	$ lsb_release -a


	2、历史版本codename

	Ubuntu 发布版本的官方名称是 Ubuntu X.YY ，其中 X 表示年份（减去2000），YY 表示发布的月份。

	Ubuntu 没有像其它软件一样有 1.0 版本，是因为其第一个版本是发布于 2004 年。所以Ubuntu的生日是10月20日。

	samba配置

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
                使用：电脑的运行中输入（\\ubuntu的ip，例如：\\192.168.1.8  注意：要关闭window系统的防火墙）


	file 文件名 查看文件的格式  ELF是可执行文件的格式

	1. linux下常见压缩格式:   
		.gz -- gzip
		.bz2 - bzip2
	2. 常用压缩命令:
		○ tar - 打包
			§ 参数：
				c - 创建压缩文件
				x - 释放压缩文件
				v - 打印提示信息（可不写） 在终端打印
				f - 指定压缩包的名字
				z - 使用gzip压缩文件 - xxx.tar.gz
				j - 使用bzip2的方式压缩文件 -- xxx.tar.bz2
				这时每个人都遵循的一种模式，但是这不是必须的
			§ 压缩：
				tar 参数 压缩包的名字 原材料 -- gz
					tar zcvf test.tar.gz file dir  原材料有一个就写一个  有多个就写多个
			§ 解压缩
					tar zxvf test.tar.gz -C 解压目录
		rar 
			rar需要安装
				□ sudo apt-get install rar
			压缩：
			rar a 压缩包名（不用指定后缀） 压缩内容   a代表压缩  压缩内容其实也就是原材料
				□ 压缩目录加参数 -r
			解压缩：
			rar x 压缩包名 解压目录  x是释放的意思 
		○ zip/unzip
			压缩：
			zip 参数 压缩包名 原材料
				如果有目录： -r
			解压缩：
			unzip 压缩包的名字 -d 解压目录
			
	3. 总结
		压缩：
		tar/rar/zip 参数 压缩包名  原材料
		解压缩
		tar/rar/unzip 参数 压缩包名 参数 解压路径
			rar 解压缩到指定目录不需要指定参数
			unzip 不需要解压参数

		xz压缩格式转为tar压缩格式：
			xz -d xxx.tar.xz
		tar压缩格式转为xz压缩格式
			xz -z xxx.tar.xz
		1）xz
	
		压缩：
			先把目标压缩成tar格式
			
			tar cf 压缩包的名字.tar. 目标
			
			把tar转成xz格式
			
			xz  -z 压缩包的名字.tar.
			
			z:强制压缩
			d:强制解压
			
			
		解压：
			  把xz -- tar
			  再解压tar 

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


		配置 vim

		vim的配置是在用户主目录下的 ~/.vimrc 文件中完成的，如果没有的话，需要自己新建一下。

		编辑 ~/.vimrc 文件，写入以下内容：

		filetype off
		set rtp+=~/.vim/bundle/vundle/
		call vundle#rc()

		if filereadable(expand("~/.vimrc.bundles"))
		  source ~/.vimrc.bundles
		endif
		为了防止配置文件太乱，我们可以通过~/.vimrc.bundles管理我们安装的插件。

		.vimrc.bundles配置文件

		首先创建文件~/.vimrc.bundles，然后添加代码如下：

		if &compatible
		  set nocompatible
		end

		filetype off
		set rtp+=~/.vim/bundle/vundle/
		call vundle#rc()

		" Let Vundle manage Vundle
		Bundle 'gmarik/vundle'

		" Define bundles via Github repos
		" 标签导航
		Bundle 'majutsushi/tagbar'
		Bundle 'vim-scripts/ctags.vim'
		" 静态代码分析
		Bundle 'scrooloose/syntastic'
		" 文件搜索
		Bundle 'kien/ctrlp.vim'
		" 目录树导航
		Bundle "scrooloose/nerdtree"
		" 美化状态栏
		Bundle "Lokaltog/vim-powerline"
		" 主题风格
		Bundle "altercation/vim-colors-solarized"
		" python自动补全
		Bundle 'davidhalter/jedi-vim'
		Bundle "klen/python-mode"
		" 括号匹配高亮
		Bundle 'kien/rainbow_parentheses.vim'
		" 可视化缩进
		Bundle 'nathanaelkane/vim-indent-guides'
		if filereadable(expand("~/.vimrc.bundles.local"))
		  source ~/.vimrc.bundles.local
		endif

		filetype on
		如上述代码所示，我们通过Bundle指定各个插件在Github的地址，填写规则是"用户名/仓库名"。书写规则有三种，这里使用的是最常见的一种，其它书写方法这里就不说了。

		安装插件

		我们已经指定好了各个插件的路径，接下里就是安装各个插件了。在shell中输入vim，进入命令行模式输入BundleInstall。

		运行这个命令就开始自行安装我们之前指定的各个插件了。这个过程需要连网，下载并安装好各个插件之后会提示Done!

		注意：由于tagbar依赖于ctags，所以我们还需要用指令安装ctags：

		yum install ctags -y

		插件配置

		1、基础配置

		已经安装好了各个插件，接下里就可以直接用了吗？答案是否定的，我们还需要继续对自己安装的插件进行配置。配置这里也很简单，下面是我的配置，编写~/.vimrc：

		配置vim
		第一步：在终端创建.vimrc文件命令为:   $vi ~/.vimrc   

		(ps:表示手动设置一个配置文件 :vimrc  , 这里把.vimrc文件创建在当前用户的根目录下)

		第二步：在.vimrc文件中添加配置内容：

		 set shortmess=atI   " 启动的时候不显示那个援助乌干达儿童的提示  
		 winpos 5 5          " 设定窗口位置  
		 set lines=40 columns=155    " 设定窗口大小  
		 set nu              " 显示行号  
		 set go=             " 不要图形按钮   
		 syntax on           " 语法高亮  
		 autocmd InsertLeave * se nocul  " 用浅色高亮当前行  
		 autocmd InsertEnter * se cul    " 用浅色高亮当前行  
		 set showcmd         " 输入的命令显示出来，看的清楚些  
		 set statusline=%F%m%r%h%w\ [FORMAT=%{&ff}]\ [TYPE=%Y]\ [POS=%l,%v][%p%%]\ %{strftime(\"%d/%m/%y\ -\ %H:%M\")}   "状态行显示的内容  
		 set laststatus=1    " 启动显示状态行(1),总是显示状态行(2)  
		 set foldmethod=manual   " 手动折叠  
		 set background=dark "背景使用黑色 
		 set nocompatible  "去掉讨厌的有关vi一致性模式，避免以前版本的一些bug和局限  
		 " 显示中文帮助
		 if version >= 603
		 set helplang=cn
		 set encoding=utf-8
		 endif
		set fencs=utf-8,ucs-bom,shift-jis,gb18030,gbk,gb2312,cp936
		set termencoding=utf-8
		set encoding=utf-8
		set fileencodings=ucs-bom,utf-8,cp936
		set fileencoding=utf-8
		autocmd BufReadPost *
		    \ if line("'\"") > 0 && line("'\"") <= line("$") |
		    \     exe "normal g'\"" |
		    \ endif
		let mapleader=","
		nnoremap <space> :
		vnoremap <space> :
		nnoremap <C-h> <C-W>h
		nnoremap <C-j> <C-W>j
		nnoremap <C-k> <C-W>k
		nnoremap <C-l> <C-W>l
		inoremap <C-h> <Esc><C-W>h
		inoremap <C-j> <Esc><C-W>j
		inoremap <C-k> <Esc><C-W>k
		inoremap <C-l> <Esc><C-W>l
		let OpenDir=system("pwd")
		nmap <silent> <leader>cd :exe 'cd ' . OpenDir<cr>:pwd<cr>
		let g:Tlist_Auto_Update=1
		let g:Tlist_Process_File_Always=1
		let g:Tlist_Exit_OnlyWindow=1
		let g:Tlist_Show_One_File=1
		let g:Tlist_WinWidth=25
		let g:Tlist_Enable_Fold_Column=0
		let g:Tlist_Auto_Highlight_Tag=1
		let g:NERDTreeWinPos="right"
		let g:NERDTreeWinSize=25
		let g:NERDTreeShowLineNumbers=1
		let g:NERDTreeQuitOnOpen=1
		if has("cscope")
		    set csto=1
		    set cst
		    set nocsverb
		    if filereadable("cscope.out")
		        cs add cscope.out
		    endif
		    set csverb
		endif
		let g:OmniCpp_DefaultNamespaces=["std"]
		let g:OmniCpp_MayCompleteScope=1
		let g:OmniCpp_SelectFirstItem=2
		if has("gdb")
		    set asm=0
		    let g:vimgdb_debug_file=""
		    run macros/gdb_mappings.vim
		endif
		let g:LookupFile_TagExpr='"./tags.filename"'
		let g:LookupFile_MinPatLength=2
		let g:LookupFile_PreserveLastPattern=0
		let g:LookupFile_PreservePatternHistory=1
		let g:LookupFile_AlwaysAcceptFirst=1
		let g:LookupFile_AllowNewFiles=0
		source $VIMRUNTIME/ftplugin/man.vim
		let g:snips_author="Du Jianfeng"
		let g:snips_email="cmdxiaoha@163.com"
		let g:snips_copyright="SicMicro, Inc"
		function! RunShell(Msg, Shell)
		    echo a:Msg . '...'
		    call system(a:Shell)
		    echon 'done'
		endfunction
		nmap  <F2> :TlistToggle<cr>
		nmap  <F3> :NERDTreeToggle<cr>
		nmap  <F4> :MRU<cr>
		nmap  <F5> <Plug>LookupFile<cr>
		nmap  <F6> :vimgrep /<C-R>=expand("<cword>")<cr>/ **/*.c **/*.h<cr><C-o>:cw<cr>
		nmap  <F9> :call RunShell("Generate tags", "ctags -R --c++-kinds=+p --fields=+iaS --extra=+q .")<cr>
		nmap <F10> :call HLUDSync()<cr>
		nmap <F11> :call RunShell("Generate filename tags", "~/.vim/shell/genfiletags.sh")<cr>
		nmap <F12> :call RunShell("Generate cscope", "cscope -Rb")<cr>:cs add cscope.out<cr>
		nmap <leader>sa :cs add cscope.out<cr>
		nmap <leader>ss :cs find s <C-R>=expand("<cword>")<cr><cr>
		nmap <leader>sg :cs find g <C-R>=expand("<cword>")<cr><cr>
		nmap <leader>sc :cs find c <C-R>=expand("<cword>")<cr><cr>
		nmap <leader>st :cs find t <C-R>=expand("<cword>")<cr><cr>
		nmap <leader>se :cs find e <C-R>=expand("<cword>")<cr><cr>
		nmap <leader>sf :cs find f <C-R>=expand("<cfile>")<cr><cr>
		nmap <leader>si :cs find i <C-R>=expand("<cfile>")<cr><cr>
		nmap <leader>sd :cs find d <C-R>=expand("<cword>")<cr><cr>
		nmap <leader>zz <C-w>o
		nmap <leader>gs :GetScripts<cr>
		autocmd BufNewFile *.cpp,*.[ch],*.sh,*.java exec ":call SetTitle()"
		func SetTitle()
		    if &filetype == 'sh'
		        call setline(1,"\#########################################################################")
		        call append(line("."), "\# File Name: ".expand("%"))
		        call append(line(".")+1, "\# Author: jianqiao")
		        call append(line(".")+2, "\# mail: 123456789@qq.com")
		        call append(line(".")+3, "\# Created Time:".strftime("%c"))
		        call append(line(".")+4,"\#########################################################################")
		        call append(line(".")+5, "\#!/bin/bash")
		        call append(line(".")+6, "") 
		    else
		        call setline(1, "/*************************************************************************")
		        call append(line("."), "    > File Name: ".expand("%"))
		        call append(line(".")+1, "    > Author: jianqiao")
		        call append(line(".")+2, "    > Mail: 123456789@qq.com ")
		        call append(line(".")+3, "    > Created Time: ".strftime("%c"))
		        call append(line(".")+4," ************************************************************************/")
		        call append(line(".")+5, "") 
		    endif
		    if &filetype == 'cpp'
		        call append(line(".")+6, "#include <iostream>")
		 
		        call append(line(".")+7, "")
		        call append(line(".")+8, "using namespace std;")
		        call append(line(".")+9, "")
		        call append(line(".")+10, "int main(void){")
		        call append(line(".")+11, "")
		        call append(line(".")+12, "    return 0;")
		        call append(line(".")+13, "}")
		    endif
		    if &filetype == 'c'
		        call append(line(".")+6, "#include <stdio.h>")
		        call append(line(".")+7, "")
		        call append(line(".")+8, "int main(void){")
		        call append(line(".")+9, "")
		        call append(line(".")+10,"  return 0;")
		        call append(line(".")+11, "}")
		    endif
		    autocmd BufNewFile *normal G
		endfunc
		inoremap ( ()<Esc>i>
		注意：以下的配置是在全局配置文件 /etc/profile中配置
		sudo vim /etc/profile
		set ts=4
		set expandtab
		set autoindent
		set nu
		set nocompatible
		set number
		set autoindent
		set smartindent
		set showmatch
		set ruler
		set incsearch
		set tabstop=4
		set shiftwidth=4
		set softtabstop=4
		set cindent
		set nobackup
		set clipboard+=unnamed


		配置vim
		第一步：在终端创建.vimrc文件命令为:   $vi ~/.vimrc   

		(ps:表示手动设置一个配置文件 :vimrc  , 这里把.vimrc文件创建在当前用户的根目录下)

		第二步：在.vimrc文件中添加配置内容：

		复制代码
		 set shortmess=atI   " 启动的时候不显示那个援助乌干达儿童的提示  
		 winpos 5 5          " 设定窗口位置  
		 set lines=40 columns=155    " 设定窗口大小  
		 set nu              " 显示行号  
		 set go=             " 不要图形按钮   
		 syntax on           " 语法高亮  
		 autocmd InsertLeave * se nocul  " 用浅色高亮当前行  
		 autocmd InsertEnter * se cul    " 用浅色高亮当前行  
		 set showcmd         " 输入的命令显示出来，看的清楚些  
		 set statusline=%F%m%r%h%w\ [FORMAT=%{&ff}]\ [TYPE=%Y]\ [POS=%l,%v][%p%%]\ %{strftime(\"%d/%m/%y\ -\ %H:%M\")}   "状态行显示的内容  
		 set laststatus=1    " 启动显示状态行(1),总是显示状态行(2)  
		 set foldmethod=manual   " 手动折叠  
		 set background=dark "背景使用黑色 
		 set nocompatible  "去掉讨厌的有关vi一致性模式，避免以前版本的一些bug和局限  
		 " 显示中文帮助
		 if version >= 603
		 set helplang=cn
		 set encoding=utf-8
		 endif
		set fencs=utf-8,ucs-bom,shift-jis,gb18030,gbk,gb2312,cp936
		set termencoding=utf-8
		set encoding=utf-8
		set fileencodings=ucs-bom,utf-8,cp936
		set fileencoding=utf-8
		autocmd BufReadPost *
		    \ if line("'\"") > 0 && line("'\"") <= line("$") |
		    \     exe "normal g'\"" |
		    \ endif
		let mapleader=","
		nnoremap <space> :
		vnoremap <space> :
		nnoremap <C-h> <C-W>h
		nnoremap <C-j> <C-W>j
		nnoremap <C-k> <C-W>k
		nnoremap <C-l> <C-W>l
		inoremap <C-h> <Esc><C-W>h
		inoremap <C-j> <Esc><C-W>j
		inoremap <C-k> <Esc><C-W>k
		inoremap <C-l> <Esc><C-W>l
		let OpenDir=system("pwd")
		nmap <silent> <leader>cd :exe 'cd ' . OpenDir<cr>:pwd<cr>
		let g:Tlist_Auto_Update=1
		let g:Tlist_Process_File_Always=1
		let g:Tlist_Exit_OnlyWindow=1
		let g:Tlist_Show_One_File=1
		let g:Tlist_WinWidth=25
		let g:Tlist_Enable_Fold_Column=0
		let g:Tlist_Auto_Highlight_Tag=1
		let g:NERDTreeWinPos="right"
		let g:NERDTreeWinSize=25
		let g:NERDTreeShowLineNumbers=1
		let g:NERDTreeQuitOnOpen=1
		if has("cscope")
		    set csto=1
		    set cst
		    set nocsverb
		    if filereadable("cscope.out")
		        cs add cscope.out
		    endif
		    set csverb
		endif
		let g:OmniCpp_DefaultNamespaces=["std"]
		let g:OmniCpp_MayCompleteScope=1
		let g:OmniCpp_SelectFirstItem=2
		if has("gdb")
		    set asm=0
		    let g:vimgdb_debug_file=""
		    run macros/gdb_mappings.vim
		endif
		let g:LookupFile_TagExpr='"./tags.filename"'
		let g:LookupFile_MinPatLength=2
		let g:LookupFile_PreserveLastPattern=0
		let g:LookupFile_PreservePatternHistory=1
		let g:LookupFile_AlwaysAcceptFirst=1
		let g:LookupFile_AllowNewFiles=0
		source $VIMRUNTIME/ftplugin/man.vim
		let g:snips_author="Du Jianfeng"
		let g:snips_email="cmdxiaoha@163.com"
		let g:snips_copyright="SicMicro, Inc"
		function! RunShell(Msg, Shell)
		    echo a:Msg . '...'
		    call system(a:Shell)
		    echon 'done'
		endfunction
		nmap  <F2> :TlistToggle<cr>
		nmap  <F3> :NERDTreeToggle<cr>
		nmap  <F4> :MRU<cr>
		nmap  <F5> <Plug>LookupFile<cr>
		nmap  <F6> :vimgrep /<C-R>=expand("<cword>")<cr>/ **/*.c **/*.h<cr><C-o>:cw<cr>
		nmap  <F9> :call RunShell("Generate tags", "ctags -R --c++-kinds=+p --fields=+iaS --extra=+q .")<cr>
		nmap <F10> :call HLUDSync()<cr>
		nmap <F11> :call RunShell("Generate filename tags", "~/.vim/shell/genfiletags.sh")<cr>
		nmap <F12> :call RunShell("Generate cscope", "cscope -Rb")<cr>:cs add cscope.out<cr>
		nmap <leader>sa :cs add cscope.out<cr>
		nmap <leader>ss :cs find s <C-R>=expand("<cword>")<cr><cr>
		nmap <leader>sg :cs find g <C-R>=expand("<cword>")<cr><cr>
		nmap <leader>sc :cs find c <C-R>=expand("<cword>")<cr><cr>
		nmap <leader>st :cs find t <C-R>=expand("<cword>")<cr><cr>
		nmap <leader>se :cs find e <C-R>=expand("<cword>")<cr><cr>
		nmap <leader>sf :cs find f <C-R>=expand("<cfile>")<cr><cr>
		nmap <leader>si :cs find i <C-R>=expand("<cfile>")<cr><cr>
		nmap <leader>sd :cs find d <C-R>=expand("<cword>")<cr><cr>
		nmap <leader>zz <C-w>o
		nmap <leader>gs :GetScripts<cr>
		autocmd BufNewFile *.cpp,*.[ch],*.sh,*.java exec ":call SetTitle()"
		func SetTitle()
		    if &filetype == 'sh'
		        call setline(1,"\#########################################################################")
		        call append(line("."), "\# File Name: ".expand("%"))
		        call append(line(".")+1, "\# Author: jianqiao")
		        call append(line(".")+2, "\# mail: 123456789@qq.com")
		        call append(line(".")+3, "\# Created Time:".strftime("%c"))
		        call append(line(".")+4,"\#########################################################################")
		        call append(line(".")+5, "\#!/bin/bash")
		        call append(line(".")+6, "") 
		    else
		        call setline(1, "/*************************************************************************")
		        call append(line("."), "    > File Name: ".expand("%"))
		        call append(line(".")+1, "    > Author: jianqiao")
		        call append(line(".")+2, "    > Mail: 123456789@qq.com ")
		        call append(line(".")+3, "    > Created Time: ".strftime("%c"))
		        call append(line(".")+4," ************************************************************************/")
		        call append(line(".")+5, "") 
		    endif
		    if &filetype == 'cpp'
		        call append(line(".")+6, "#include <iostream>")
		 
		        call append(line(".")+7, "")
		        call append(line(".")+8, "using namespace std;")
		        call append(line(".")+9, "")
		        call append(line(".")+10, "int main(void){")
		        call append(line(".")+11, "")
		        call append(line(".")+12, "    return 0;")
		        call append(line(".")+13, "}")
		    endif
		    if &filetype == 'c'
		        call append(line(".")+6, "#include <stdio.h>")
		        call append(line(".")+7, "")
		        call append(line(".")+8, "int main(void){")
		        call append(line(".")+9, "")
		        call append(line(".")+10,"  return 0;")
		        call append(line(".")+11, "}")
		    endif
		    autocmd BufNewFile *normal G
		endfunc
		inoremap ( ()<Esc>i>
		注意：以下的配置是在全局配置文件 /etc/profile中配置
		sudo vim /etc/profile
		set ts=4
		set expandtab
		set autoindent
		set nu
		set nocompatible
		set number
		set autoindent
		set smartindent
		set showmatch
		set ruler
		set incsearch
		set tabstop=4
		set shiftwidth=4
		set softtabstop=4
		set cindent
		set nobackup
		set clipboard+=unnamed

		Vm Linux虚拟机Open-vm-tools替代VMware tools（共享文件使用不了但是可以实现 Ctrl+C和Ctrl+V）
		之前在安装Ubuntu虚拟机的时候，出现下面的提示，经过参看相关的文档，查询了一下关于Open-vm-tools的介绍。

		Open VM Tools (open-vm-tools) 是适用于 Linux 客户机操作系统的 VMware Tools 的开源实现。VMware 提供了操作系统特定软件包 (OSP) 充当 VMware Tools 的打包和分发机制。官方都认可了相关的工具。以后就可以安装该Open VM Tools 。

		下面就介绍一下如何安装Open VM Tools 来替换VMware tools，该工具包支持多个linux，这里就以Ubuntu虚拟机系统为例来进行安装。

		下面是具有open-vm-tools的操作系统列表
		Red Hat Enterprise Linux 7.0 及更高版本
		SUSE Linux Enterprise 12 及更高版本
		Ubuntu 14.04 及更高版本
		Centos 7 及更高版本
		FreeBSD 10.3、10.4和11.1
		Debian 7.x 及更高版本
		Oracle Linux 7 及更高版本
		Fedora 19 及更高版本
		openSUSE 11.x 及更高版本

		使用快捷键Ctrl+Alt+T打开Ubuntu系统的终端

		sudo apt install open-vm-tools
		yum install open-vm-tools -y

		经过一段时间的安装，就完成了Open-vm-tools的安装。

		根据上面的提示建议，还要进行open-vm-tools-desktop（支持双向拖放文件），在终端窗口输入如下的命令进行安装。

		#sudo apt install open-vm-tools-desktop
		yum install open-vm-tools-desktop -y

		colorscheme：
		delek elflord


		sudo apt-get install xfce4-terminal
		ubuntu18终端背景图片设置，以及配置快捷键
		1. 安装软件xfce4
		打开终端以此输入
		sudo apt-get update 
		#更新软件源
		sudo apt-get install xfce4-terminal
		#安装xfce4终端，由于Ubuntu18自带的终端不会设置背景图片.

		2.配置
		在右下角的dash菜单栏中找到Xfce4终端；打开
		在“编辑”—>“首选项”---->“外观”---->"背景图片"选择你想设置的图片；
		更多终端设置，自行探索；

		3.设置快捷键
		正常使用是我们按ctr(常用^表示)+Alt+T打开我们使用的终端；现在我们设置新终端的快捷方式；

		在右上角–>有线连接---->有线设置---->设备—>键盘 将其选中
		在键盘的最下面有个"+"加号；点击
		然后输入：
		名称：你想对快捷键设置的名字
		命令：xfce4-terminal
		命令是你在原先终端中输入的可行命令

		最后点击设置快捷键，然后在弹出的窗口中同时按你想设置的快捷键，但是不能和已有的快捷键重复；我设置的是ctrl+alt+R;

		个人笔记：
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
		ctag文件是实现跳转功能的 就是它可以把我们送到我们想要去的地方 如果别人在他自己的程序里写了一个库函数printf。那在某个时刻他想去查看这个库函数本身是怎么实现的那么他只需要把光标停在关键词上，再按一下组合键Ctrl+] 就会立即跳转到库函数printf的源代码的地方，再按一下组合键Ctrl+O就可以跳转回来。

		如果后续需要重启网卡怎么去操作呢？
		#service network restart

		在有的分支版本中可能没有service命令来快速操作服务，但是有一个共性的目录：/etc/init.d
		这个目录中放着很对服务的快捷方式。
		此处重启网卡命令还可以使用：
		#/etc/init.d/network restart

		ARM 开发板 型号GEC6818
		LED 底板 刷机 系统移植  刷机与系统移植差不多  
		主体芯片  CPU S5P6818 s:是三星生产的  ARM设计的CPU电路架构  ARM专门提供专利  
		CPU频率 1.4Ghz+ +超频率 启动开发板之后在查看相关信息 如位数  CPU电路架构比较重要  
		CPU架构 Cortex -A53  Cortex 系列 ARM公司设计出来的 手机也有相应的  
		Cortex: 
		A:专门搭载高性能的处理系统  Ubuntu(桌面版) 安卓(开源) Linux(服务器版本)  
		A8(苹果4：A8来搭载的 魅族m9) A9 A53(低功率的CPU核心) A72 A76  多核CPU 小米6 A53 A72 
		M:专门做微型处理器的  STM32(M3,4) 低功耗的MCU(微型处理器)
		R:时钟频率和较高的实时性结合的车载功能 专门做实时性很强的，车载的一些功能  数据检测 等等  
		可以去了解ARM的发展历史
		运行内存(DRAM):笔记本标配16G   开发板是1G  软件优势 要非常贴切硬件   
		闪存(NAND FLASH):开发板8G  闪存也存储器  断电之后数据会保存到磁盘  
		串口接口 由硬件控制转为由系统控制  驱动  安装驱动  只要有操作系统就要有驱动 没有驱动硬件就体现不了作用
		SD卡接口  内存卡槽   
		外围电路 
		LCD: 分辨率:800*480  像素点
		网口
		串口
		音频输入输出
		电平
		电源: 5V
		SDNI 

		18:06 2020/7/29
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


		交叉编译是在一个平台上生成另一个平台上的可执行代码。同一个体系结构可以运行不同的操作系统；同样，同一个操作系统也可以在不同的体系结构上运行。
		gcc和arm-linux-gcc 用的是不同的指令集，交叉编译是用RSIC(精简指令集) ，它们编译生成的可执行文件只被对应的环境识别，gcc是linux系统下用来将代码编译成一个可执行程序的方法。编译出来的是适用于linux系统的可执行二进制文件，arm-linux-gcc交叉编译是告诉编译器，编辑的环境是linux，但是生成的是arm识别的可执行文件。
		arm-linux-gcc是一种在linux环境编写，生成在arm上运行的可执行程序的编译方式，这就是交叉编译，编写环境和运行环境分离的一种手段。而不同的系统机器码含义不同，所以arm-linux-gcc编译出来的机器码无法在Ubuntu上执行，只能在arm平台上执行。
		gcc是x86架构指令用的，arm-linux-gcc 是RSIC(精简指令集) ARM架构上面用的，它们会把c源码编译成不同的汇编指令然后生产不同平台的二进制可执行的文件，gcc是linux系统下bai面用来将代码编译成一个可执行程序的手段。编译出来的是适用于linux系统的可执行二进制文件，arm-linux-gcc就是告诉编译器，我编写的环境是linux，但是我希望生成的可执行程序是在arm上面跑的。这就是交叉编译。编写环境和执行环境分离的一种手段。
		gcc是linux系统下面用来将代码编译成一个可执行程序的手段。编译出来的是适用于linux系统的可执行二进制文件。可执行程序其实就是一堆的0101二进制机器码。这些机器码代表什么含义只有机器本身能理解。
		而arm-linux-gcc就是告诉编译器，我编写的环境是linux，但是我希望生成的可执行程序是在arm上面跑的。这就是交叉编译，编写环境和执行环境分离的一种手段。



		gec6818开发板刷机教程：
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

