---
title: C_Language_Basics_02
date: 2020-07-26 10:53:18
---

Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

# 一、Linux系统基本操作
  1.1 背景

      前身：UNIX
      发行时间:
              1970  （B语言）
              1972  （C语言）
              1991  （Linux）

      特点：
           开源
           基本用命令进行人机交流
           文件系统内只有一个根目录

      命令：
        如何打开输入命令的窗口（命令行终端）
          ATL + CTRL + T

        查看系统版本：cat /etc/issue
        查看系统位数：getconf LONG_BIT

        下载系统镜像：
                  www.ubuntu.com
## Linux的基本操作命令
      普通：
          1：路径的切换:切换到指定的目录下 返回上一级 返回上一次 直接返回到家目录
                  1)cd:
                        cd ..  返回上一级
                        cd -   返回上一次
                        cd     直接返回到家目录(/home/用户名：普通用户合法工作目录：一般的在这里面操作不需要管理员的权限：sudo)
                        cd      路径（绝对+相对）

                        绝对路径：从根目录开始写的路径
                        相对路径：从当前路径下开始的路径

          2：文件的操作：打开，编码（vi vim gedit。。。）创建、删除、复制、黏贴、压缩、解压

            2.1vim的使用：
                  启动命令：vim 文件的名字
                  进入编辑模式，编写代码：a
                               i:从当前光标进入编辑模式
                               o
                  退出编辑模式：esc
                  进入命令行模式,输入保存的命令：shift + :
                                               输入保存命令：w

                  vim快捷键：
                    在普通模式下的
                        复制：数字+yy(注意看光标所在的位置。复制光标所在的一行)
                        剪切(删除一行)：dd
                        x(删除剪切单个字母)：
                        粘贴：p
                        撤销：u
                        跳到第一行：gg
                        跳到最后一行：G
                        替换:r
                    命令行模式:
                          搜索：
                              /搜索内容
                              ?搜索内容
                2.2 创建（文件和目录）

                    touch 文件名字（绝对路径，相对路径）
                    例子：用相对路径的方式在桌面创建一个world.c

                    mkdir 目录名字
                      例子：用相对路径的方式在桌面创建一个code

                  2.3 删除：（文件和目录）

                      rm    文件
                      rm    目录 -r  （不管文件夹空不空，都可以删除）
                      rmdir 目录     （只能删除空的文件夹）

                      -r：指定系统采用递归的方式删除目标

                  2.4 显示指定目录内的内容 ls

                    常见的使用方式：
                              1： 直接输入ls   默认显示当前路径下的内容
                              2： ls -a       默认显示当前路径下所有的内容（普通+隐藏）
                                  注：每一个目录下都有.和..
                              3： ls -A       默认显示当前路径下所有的内容(忽略.和..)
                              4:  ls 路径 +-l       显示当前目录下的文件的详细信息

                              drwxr-xr-x   2 root root  4096 8月   8  2019 bin
                              d:目录
                              -:普通文件
                              p:管道文件
                              l:链接文件（快捷方式，软链接，硬链接）
                              c:字符设备文件（驱动设备节点，安装了驱动才会生成的）
                              b:块设备文件
                              s:套接字文件（网络编程）
                注：C语言代码编译成功后，会生成一个二进制可执行文件（程序）

                高级语言                      机器码
                编译命令：gcc hello.c -o hello
                运行命令：./hello
                . :当前目录
                ..：上一级目录
                /:分隔符
### C语言的编译过程
                2.5 C语言编译过程:

                    预编译：
                          #include头文件的包含（吧对应的头文件里面的内容直接添加到源文件里面<hello.c>）
                          宏定义的替换
                          gcc -E hello.c -o hello.i  (.i是预编译生成的文件的后缀名)

                    编译：（检查C语言语法）
                          gcc -S hello.i -o hello.s(.s 汇编文件)

                    汇编:（检查汇编的语法）
                          gcc -c hello.s -o hello.o (没有链接库的二进制文件，是不可以执行的)

                          printf("hello world"); //用的时候，吃菜
                          放在C语言标准库libc.so  或者libc.a

                    链接：把二进制文件和库文件链接起来，形成ELF格式的可执行二进制文件
                          补充：
                              查看文件的格式: file 文件名字

                           gcc hello.o -o hello


      开发：
          阅读系统源码（系统引导uboot+内核源码+文件系统）---> 用源码编译驱动
          阅读第三方库（人脸识别：csdn找源码压缩包，库文件：.so或者.a专门存放某些特定功能的函数接口API）
                      根据源码与偏执文件编译出库文件：
                        编译前的配置（库的configure 配置生成makefile）
                        编译       （编译：make ）
                        安装       （生成库文件:make install）


		#include <stdio.h>

		int main()
		{

		  int data;  //没有初始化的局部变量，会赋值随机值
		  int ret;
		  while(1)
		  {
		      ret = scanf("%d",&data);//存放的是data的地址
		      if(ret != 1)
		      {
		        while(getchar() != '\n');  //从缓存区里面获取一个字符数据
		        printf("输入有误，请在输入一次！\n");
		      }
		      if(ret == 1)
		      {
		        printf("输入成功，%d\n",data);
		        break;
		      }
		  }


		  return 0;
		}

		数据溢出
		#include <stdio.h>

		int main()
		{

		  char a = 128;
		  printf("%d\n",a);
		  signed char b = 0xff;
		  printf("%d\n",b);
		  return 0;
		}
### C语言数据类型
![](/hexo-private-blog-website/images/C语言数据类型.bmp)
### 转义字符
![](/hexo-private-blog-website/images/转义字符.png)
### printf与缓存区
![](/hexo-private-blog-website/images/printf与缓存区.bmp)
### scanf与缓存区
![](/hexo-private-blog-website/images/scanf与缓存区.bmp)
### 支付宝付款:
![](/hexo-private-blog-website/images/alipay.jpg)

-------------------------------------------------------------------------------------------
#### C语言基础
下午：
  一  编码环境：安装vm-tools 共享文件夹
  二、C语言基础

    背景：
        时间：UNXI第二版的发行时间1972
        原由：
              为了开发操作系统

      C语言是一门面向过程的程序语言。

      面向过程：
            宏观：以程序功能为目的，以编码过程为基础，来实现具有实际意义的程序。
            微观: 以程序功能为目的，以编码实现函数API的封装步骤为过程，按照逻辑的步骤一步一步调用即可。

        例子：编写一份C语言程序，实现生成一个随机的整形数组，然后实现从大到小排列里面的数组元素。

            1：封装一个可以生成随机数组的函数
            2：封装实现从大到小排列的函数
            3：封装一个按顺序打印数组里面每一个元素的数组
        C语言程序里面最基本的单位是函数。

    2.1 数据类型

        关键词命名规则：有字母下换线数字组成，开头不能是数字。

        1)布尔类型：表达真和假(0或者1)

          布尔类型怎么用？布尔类型的大小是多少？

          定义bool变量：
            bool mask = 0;
          使用杂项运算符sizeof

      2)各个数据类型的输出格式
        注：数据是以补码的形式存放，正数的补码是其本身，负数的补码是取反+1
          int :
                十进制   %d
                八进制   %o
                十六进制 %x
          全称：signed int (有符号的整形)  -2的31次方  ~ 2的31次方-1
                unsigned int(无符号的整形，只有整形)
          思考：signed int a = 3,在内存中的存储形式？,最高以为称为符号位：0：正  1：负
                以二进制形式存放：
                                00000000...00011

                signed int a = -3,
                以二进制形式存放：
                                10000000...00011
                                11111111...11100
                                                +1
                                11111111...11101

          short : short int
                十进制   %hd
                八进制   %ho
                十六进制 %hx

      数据溢出：
            char a = 128; 0x7f
            01111111
            10000000  （是哪个负数的补码）

      负数：原码取反+1

            01111111
            10000000 -128

      unsigned char a = 0x7f;
      printf("%d\n",++a);

      signed char b = 0xff;
      printf("%d\n",b);

      b = 11111111;
      看11111111是哪个负数的补码？
      11111110
      00000001 1的源码 -1


基本的输入输出函数：
  在标准输入输出的头文件：stdio.h
  文件指针：
  extern struct _IO_FILE *stdin;		/* Standard input stream.  */
  extern struct _IO_FILE *stdout;		/* Standard output stream.  */
  extern struct _IO_FILE *stderr;		/* Standard error output stream.  */

  man手册：
      man man

      1
      可执行程序或 shell 命令  man 1 ls
      2
      系统调用(内核提供的函数)
      3
      库调用(程序库中的函数)


      输出：printf puts putchar fputs

        puts： 打印输出字符串

        函数原型：int puts(const char *s);
        形参：s 存放要打印的字符串的首地址的指针变量
        返回值：

        char buf[] = "hellowrld";
        puts(buf); //buf数组名是数组首元素的地址


      输入：scanf  gets getchar fgets


      练习：
          定义一个整形变量，赋值一个十六进制的数，然输出它对应八进制的数。

          把各个类型的输出形式打一下。

          short的全称

          写一份循环打印helloworld的代码
            特定条件：
                  一秒打印一次
                  不用换行的打印，

            看看现象

          写一份循环输入一个字符数据，然后在打印改数据

     添加和删除用户和组别（必做）
     指针变量的大小？（必做）
     找一下刷新缓存区的API（必做）
     找一下设置缓存区的大小的API（选做）
     理解一下什么叫做C语言程序的生命周期。（必做）

① 
添加用户可以使用useradd指令 useradd 选项 用户名
常用选项：
-g: 表示指定用户的用户主组，选项的值可以是用户组的id，也可以是组名
-G: 表示指定用户的用户附加组，选项的值可以是用户组的id，也可以是组名
-u: uid，用户的id（用户的标识符)，系统默认会从500之后按顺序分配uid，如果不想使用系统分配的，可以通过该选项自定义
也可以直接创建用户 如：useradd zhangsan 含义 创建一个为张三的用户名
注意：在不添加选项的时候，执行useradd之后会执行一系列的操作
a. 创建同名的家目录；
b. 创建同名的用户组；
添加用户组可以使用usergroup指令 groupadd 选项 用户组名
常用选项：
-g：类似用户添加里的“-u”，-g表示选择自己设置一个自定义的用户组ID数字，如果自己不指定，则默认从500之后递增
如:用groupadd指令创建一个新的用户组，命名为Administrators 
groupadd Administrators
删除用户可以使用userdel指令 userdel 选项 用户名
常用选项：
-r：表示删除用户的同时，删除其家目录
删除用户组可以使用groupdel指令 groupdel 用户组名
注意：当如果需要删除一个组，但是这个组是某个用户的主组时，则不允许删除；如果确实需要删除，则先从组内移出所有用户。

② 
一个任何类型的指针变量所占用的字节大小都是4个字节
这个指针变量所占用的字节大小与地址总线有关，而32位的操作系统就是指地址总线是32位的操作系统
也就是说操作系统的位数决定了指针变量所占的字节数

③ 
fflush
int fflush(FILE* stream);
用于清空文件缓冲区，如果文件是以写的方式打开的，则把缓冲区内容写入文件。
此函数包含在stdio.h头文件中，用来强制将缓冲区中的内容写入文件。
函数功能：清除一个流，即清除文件缓冲区.

④
如果我们没有自己设置缓冲区的话，系统会默认为标准输入输出设置一个缓冲区，这个缓冲区的大小通常是512个字节的大小。
缓冲区大小由 stdio.h 头文件中的宏 BUFSIZ 定义，如果希望查看它的大小，包含头文件，直接输出它的值即可：
printf(“%d”, BUFSIZ);
缓冲区的大小是可以改变的，也可以将文件关联到自定义的缓冲区，详情可以查看 setvbuf() 和 setbuf() 函数。

⑤
程序的生命周期是从源程序开始的
源程序经过编译最终生成可执行文件
这个过程分为四个阶段完成，执行这四个阶段的程序（预编译、编译、汇编、链接）一起构成了编译系统。
预编译：实现include头文件的包含和宏定义的替换 gcc -E hello.c -o hello.i
编译：检查c语言语法 gcc -S hello.i -o hello.s
汇编：检查汇编的语法 gcc -c hello.s -o hello.o
链接：把二进制文件和库文件链接起来 gcc hello.o -o hello
最终得到可执行的二进制目标文件，加载到运行内存中，由系统运行执行。

#### alipay Payment:
![alipay2.jpg](http://ww1.sinaimg.cn/large/006DnxC4gy1gftfn69zcaj30fb0fhjus.jpg)
![alipay3.jpg](http://ww1.sinaimg.cn/large/006DnxC4gy1gftfnd2mwgj30fb0f3juw.jpg)

#### wechat Payment:
![wechat1.jpg](http://ww1.sinaimg.cn/large/006DnxC4gy1gftfo0b23rj30fb0f0tbz.jpg)
![wechat01.jpg](http://ww1.sinaimg.cn/large/006DnxC4gy1gftfo7ojvvj30fb0eujul.jpg)

#### tenpay Payment:
![tenpay4.jpg](http://ww1.sinaimg.cn/large/006DnxC4gy1gftg6n3n65j30bh0be77h.jpg)
![tenpay2.jpg](http://ww1.sinaimg.cn/large/006DnxC4gy1gftfsqq26qj30bh0bdgom.jpg)
![tenpay5.jpg](http://ww1.sinaimg.cn/large/006DnxC4gy1gftg9w5ytlj30bh0bhjuk.jpg)