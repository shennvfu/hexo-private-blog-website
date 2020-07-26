---
title: C_Language_Basics_01
date: 2020-07-25 09:53:18
---

Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

# 一、嵌入式技术背景
1.1 嵌入式只专门生产功能专一的电子产品。（电子产品：所有通电的）

  1.2 嵌入式开发方向：

    涉及操作系统开发的：
                  windows: 电脑，微软产品（闭源）
                  Linux:ubuntu centos 、红帽、中标麒麟...(免费)
                        电脑，手机（安卓：Linux内核、苹果：UNIX）、智能终端：医疗仪器（Qt:C++）、车载系统、银行挂号、智能农业，语音机器人,
                        航拍无人机（地形地貌---系统移植）...
    涉及微型处理器开发的（MCU:单片机、STM32（ucos系统）、zigbee）：收音机、冰箱、洗衣机、空调、注射仪...


    职位：
      Linux命令（SHELL：脚本命令）：Linux服务器维护、系统管理
      C语言：C语言工程师（数据结构）
      系统编程：进程、线程（线程同步，进程间通信）
                智能终端，做特定的系统（一般都是下载一个原生态的系统，自己裁剪）注：.ko文件（编译出来的，安装） + app文件（应用程序）
                驱动开发（再基于操纵系统上，所有硬件都需要驱动）：编写驱动（USB驱动）
      应用层：Qt ：游戏，软件程序
      数据库：SQL

      硬件:画板、焊接...

## 现阶段的知识点
Linux基本操作
    C语言：
      C语言的本质
        数据类型
        运算符
        控制流语句（9个）
        数组
        函数（一:声明、定义、封装、调用）
        指针（一级指针，指针和数组和函数的关系）
        结构体、枚举、联合体
        函数（二、回调函数、递归函数、内联函数、变参函数：printf scanf）
        特殊关键字
        内存分布（程序是在哪里运行的？是怎么会运行的？ ）
                在运行内存里面运行的 CPU --> 系统  --> 程序

                .c里面int a；4个字节

### 记录
    ① root用户输入passwd指令可以直接修改密码(其它用户也可以直接修改);
    而普通用户输入passwd指令只可以修改它自己的密码
    ②嵌入式方向职位：
    QT工程师
    嵌入式物联网工程师
    嵌入式驱动工程师
    嵌入式AI开发工程师
    ③根目录(/)下的目录结构、目录的分类
    bin: binary 该目录中存储的都是些二进制文件，文件都是可以被运行的 可执行程序
    dev：在linux下一切皆文件  device 该目录中主要存放的是外接设备，例如U盘或者新添加的硬盘等。在其中的外接设备是不能直接被使用的，它需要挂载才能使用(类似于Windows下的分配盘符)
    etc：该目录主要存储的是系统的一些配置文件
    home：表示除了root用户以外其它用户的家目录，类似于Windows下的User/用户目录
    proc：process，表示进程，该目录中存储的是linux运行时候的进程
    root：该目录是root用户自己的家目录
    sbin：该目录是存储一些可以被执行的二进制文件，但是必须得有超级管理员的权限的用户才能够执行
    tmp：存放临时文件，当系统运行时候产生的临时文件会存储在这个目录
    usr：该目录是存放用户自己安装的应用程序，类似于Windows下的program files
    var：该目录存放的是程序/系统的日志文件的目录
    mnt：该目录存放的是 当外接设备需要挂载的时候，就需要挂载到mnt目录下
    lib：该目录存放的是linux运行的时候需要加载的一些动态库(类似于Windows下的dll文件)

    个人观点：开源并不意味着免费。
    以上有些是参考的资料，有些是个人观点。



  1、如何修改用户密码
  答：  ubuntu 终端先输入: sudo passwd xxx(用户)，根据提示输入更改密码

  2、根目录文件夹的特点
  答：/bin     存放二进制可执行文件，可以被root和一般的账号使用
      /boot    Ubuntu内核和启动文件
      /dev     设备驱动文件
      /etc     存放一些系统配置文件，比如用户账号和密码文件，各种服务的起始地址。
      /home    系统默认的用户主文件夹，一般创建用户账户的时候，默认的用户主文件夹都会放到此目录下。
      /lib     存放库文件
      /media   此目录下放置可插拔设备，比如SD卡，U盘就是挂载到这个目录中。
      /mnt     用户可使用的挂载点，如果要挂载一些额外的设备，那么就可以挂载到此处。
      /opt     可选的文件和程序存放目录，给第三方软件放置的目录。
      /root    root用户目录，系统管理员目录。
      /sbin    存放一些二进制可执行文件,一般是系统开机过程中所需要的命令。
      /srv     服务相关目录
      /sys     记录内核信息，虚拟文件系统。  
      /tmp     临时目录
      /var     存放一些变化的文件，比如日志文件
      /usr     存放于系统用户有关的文件，会占用很大的存储空间！
      /proc    虚拟文件系统，数据放置到内存中，存放系统运行信息

  3、两个嵌入式方向的职位
  答：   单片机开发
         机器人研发



#### alipay Payment:
![alipay2.jpg](http://ww1.sinaimg.cn/large/006DnxC4gy1gftfn69zcaj30fb0fhjus.jpg)


#### wechat Payment:
![wechat1.jpg](http://ww1.sinaimg.cn/large/006DnxC4gy1gftfo0b23rj30fb0f0tbz.jpg)


#### tenpay Payment:
![tenpay4.jpg](http://ww1.sinaimg.cn/large/006DnxC4gy1gftg6n3n65j30bh0be77h.jpg)
