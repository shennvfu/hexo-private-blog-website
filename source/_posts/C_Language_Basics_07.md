---
title: C_Language_Basics_07
date: 2020-07-27 15:31:18
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

	应怜屐齿印苍苔，小扣柴扉久不开。
	春色满园关不住，一枝红杏出墙来。

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

# C语言基础
一、补充

  1.1 malloc与free的使用不当的情况：

    int * p = (int *)malloc(4*sizeof(int));
    if(p == NULL)
      {
        perror("malloc");
        return -1;
      }

      /。。。。。/
      p+1;//p没改变，
      p++;//不建议采取，因为p自身发生改变了

      free(一定时空间的首地址);//如果形参里面不是空间的首地址就会出现内存错误


    1.2:
        char * str1 = "hello"; //"hello"成为字符串常量，常量区（堆空间）

        *str1  = 's'; //h  这样会出错

        char str2[] = "hello"; //如果是在函数里面定义的，局部变量（栈空间）

        *str2 = 's';


    1.3:

        char * str1 = "hello";

        //str1是h的地址   *str1获取到的是h
        //str1+1是e的地址 *（str1+1）获取到的是e

        for(int n=0; n<strlen(str1); n++)
          printf("%c",*(str1+n));

        printf("\n");

-------------------------------------------------------------------------------------------------------------------------------------------------

二、calloc函数

  2.1 man 3 calloc  :申请nmemb块的空间

      头文件：#include <stdlib.h>
      原型： void *calloc(size_t nmemb, size_t size);

      成功：返回空间首地址
      失败：NULL

      nmemb: 块的个数
      size：每一块的大小（每一块的大小都是一样的，字节为单位）

      malloc(4*sizeof(int)); //这是申请空间，不清空
      calloc(4,sizeof(int)); //这是申请空间，会清空0
      calloc(8,sizeof(short));


      int * array = (int *)calloc(4,sizeof(int));
      if(array == NULL)
      {
        perror("calloc:");
        return -1;
      }

      *(array+1) = 100;

      array[2] = 101;

      for(int n=0; n<4; n++)
        printf("%d\n",array[n]);



		#include <stdio.h>
		#include <stdlib.h>
		#include <errno.h>

		int main()
		{

		    int * array = (int *)calloc(4,sizeof(int));
		    if(array == NULL)
		    {
		      perror("calloc:");
		      return -1;
		    }

		    *(array+1) = 100;

		    array[2] = 101;

		    for(int n=0; n<4; n++)
		      printf("%d\n",array[n]);

		    free(array);

		    return 0;
		}



		#include <stdio.h>
		#include <stdlib.h>
		#include <errno.h>

		int main()
		{

		   int * p = (int *)malloc(4*sizeof(int));
		   if(p == NULL)
		   {
		     perror("malloc");
		     return -1;
		   }
		  // int *new_p = p;
		   p++;
		   /*......*/

		  // free(new_p);
		  free(p);

		    return 0;
		}




三、函数指针变量
	
	函数指针变量：存放函数的地址（函数也是一种变量）
	
	int a()
	{
		printf("a\n");
		return 0;
	}
	
	
	
	int main()
	{
		printf("%p----%p\n",a,&a);
		return 0;
	}
	
	函数指针变量的定义与赋值、初始化：
		定义能存放返回值类型为int，没有形参的函数的地址的指针变量
		
			定义：int (*变量名)();
			赋值： 变量名 = 函数的地址；
			
				  int (*fun_p)() = &a;
			初始化：
				  int (*fun_p)() = &a;
				  
			函数指针变量解引用就是调用函数：
			
			(*fun_p)();//调用了a函数
			 a();
	
	通过函数的指针调用函数：回调函数
	函数返回指针：指针函数（他是一个函数，返回值为指针）
	函数指针：他是一个指针，存放的是函数的地址
	
	练习：

	1:int * b(int *p_1,char *p_2)
	{
		printf("a\n");
		return NULL;
	}
	
	一般定义函数指针变量的思路:
		返回值类型  （*函数指针变量名）   （形参类型）
		
	定义：int * (*new_p)(int *,char*);
		  int * (*new_p)(int *p_1,char* p_2);
		  
	2:
		封装一个函数，形参是数组类型，返回值类型为字符指针类型，
		在定义函数指针变量，实现对这个指针变量解引用调用该函数。
		
		char* Fun(char array[])
		{
			printf("%s\n",array);
			
			return NULL;
		}
		
		int main()
		{
			char * (*p)(char []) = &Fun;
			//char * (*p)(char array[]) = &Fun;
			
			//定义一个字符串，后面可以传参
			char buf[] = "hello world";
			
			(*p)(buf);
			
		
			return 0;
		}
		  
	函数指针变量当函数的形参
	数组指针变量当函数的形参
	


		#include <stdio.h>

		int a()
		{
			printf("a\n");
			return 0;
		}
			
		int main()
		{
			printf("%p----%p\n",a,&a);
			return 0;
			
		}



		#include <stdio.h>

		int A();
		int A()
		{
			printf("hello!\n");
			
			return 0;
		}

		int main()
		{
			//初始化一个函数指针变量存放A函数的地址
			int (*p)() = &A;
			//函数指针解引用
			(*p)();
			
			return 0;
		}


	#include <stdio.h>
	
	int * b(int *p_1,char *p_2)
	{
		printf("a\n");
		return NULL;
	}
	
	int a(int data)
	{
		
		return 0;
	}
	
	int main()
	{
		//定义一个函数指针变量存放b函数的地址
		//int *(*new_p)(int *,char*);
		int *(*new_p)(int *p_1,char* p_2);
		
		//赋值
		new_p = &b;
		
		//解引用
		(*new_p)(NULL,NULL);
		
		return 0;	
	}



		/*
		 封装一个函数，形参是数组类型，返回值类型为字符指针类型，
		 在定义函数指针变量，实现对这个指针变量解引用调用该函数。
		*/

		#include <stdio.h>

		char * Fun(int array[])
		{
			printf("----\n");
			return 0;
		}


		int main()
		{
			char *(*p)(int []) = &Fun;
			int array[] = {1,2,3};
			(*p)(array);
			
			return 0;
		}



		#include <stdio.h>

		char* Fun(char array[])
		{
			printf("%s\n",array);
					
			return &(array[0]);//返回一个字符变量的地址
		}
				
		int main()
		{
			char * (*p)(char []) = &Fun;
			//char * (*p)(char array[]) = &Fun;
					
			//定义一个字符串，后面可以传参
			char buf[] = "hello world";
					
			char * str = (*p)(buf);
			
			printf("%c\n",*str);
					
				
			return 0;
		}





------------------------下午------------------------------------------
补充：
	存储类：
			1:自动变量 auto:只能给局部变量使用
							 int a; auto int a;
							 
			2:寄存器存储：获取数据效率比内存快
						register int a; //大小为寄存器的大小
						
						printf("%p\n",&a);			//这种变量在内存中是没有地址的

			3：static
			
					1）在函数里面定义的
					int A()
					{
						static int a = 11;
						a++;
						printf("%d\n",a);
						return 0;
					}
					设置变量存储域 （他只初始化一次）
					设置变量作用域
					
					2）
					在函数外面定义
					a.c
					static int a = 11;	
					int A()
					{
						
						a++;
						printf("%d\n",a);
						return 0;
					}
					b.c
					int main()
					{
						a++;
						printf("%d\n",a);
						return 0;
					}
					
			补充：全局变量是在函数外面定义的，当前模块都可以使用这个变量

			4: extern 外部（一般在定义全局变量的时候使用）
			
									a.c
					int a = 11;	
					int A()
					{
						
						a++;
						printf("%d\n",a);
						return 0;
					}
					
					b.c
					
					//如果用到的变量不是在当前模块中定义，
					//需要修饰为外部变量
					extern int a; //声明为外部变量
					a = 12;
					int main()
					{
						a++;
						printf("%d\n",a);
						return 0;
					}
					-----------------------------------
					int a = 11;
					int main()
					{
						int a = 14;
						a++;
						printf("%d\n",a);
						return 0;
					}
					
					局部变量没初始化的时候，默认赋值随机值
					全局变量：
							int a[10];   0000000000
							int a;       0
							int *a;       NULL
					
					
	练习：
			1：
			
	


		#include <stdio.h>

		int A();
		int A()
		{
			static int a = 11;  //静态局部变量  只初始化一次
			a++;
			printf("%d\n",a);
			return 0;
		}

		int main()
		{
			A();// 12
			A();// 12
			
			return 0;
		}


		/*字符串常量*/

		#include <stdio.h>

		int main()
		{
		  //定义一个指针变量，然后把常量区的字符串数据赋值过去
		  char * p = "hello";

		  printf("%c\n",*p);

		//  *p = 's';

		  //printf("%c\n",*p);

		  char str2[] = "hello"; //如果是在函数里面定义的，局部变量（栈空间）

		  *str2 = 's';

		  printf("%c\n",*str2);


		  return 0;
		}



		#include <stdio.h>
		#include <string.h>

		/*遍历打印整个字符串*/
		int main()
		{
		  char * str1 = "hello";

		  //str1是h的地址   *str1获取到的是h
		  //str1+1是e的地址 *（str1+1）获取到的是e

		  for(int n=0; n<strlen(str1); n++)
		    printf("%c",*(str1+n));

		  printf("\n");


		  return 0;
		}



		#include <stdio.h>
		#include <errno.h>
		#include <stdlib.h>
		#include <string.h>

		char * My_Strcat(char * str1,char *str2);
		char * My_Strcat(char * str1,char *str2)
		{
		  //考虑能不能拼接
		  /*1：*str2 == ‘\0’,str2没有字符串数据*/
		  /*2：str1对应的空间不够存放  主动申请，不需要判断*/
		  /*3：参数为NULL*/

		  if (str1 == NULL || str2 == NULL)
		  {
		    printf("字符串操作的地址为空！\n");
		    return NULL;
		  }
		  else if(*str2 == '\0')
		  {
		    printf("str2为空，不能拼接\n");
		    return NULL;
		  }
		  else
		  {
		    //在进行拼接  --- 遍历字符串，在赋值
		    //1：申请空间
		    char * str_p = (char *)malloc((strlen(str1)+strlen(str2))*sizeof(char));
		    if(str_p == NULL)
		    {
		      perror("malloc:");
		      return NULL;
		    }
		    //2：获取str1对应的字符串最后的地址 （循环遍历）  //str1 : hello   str1指向的地址  ，需要得到一个指向o后面的地址
		    //第一种方法，str1没有改变
		    int n=0;
		    while(1)
		    {
		      if(*(str1+n) == '\0') //判断是否到字符串的末尾
		        break;

		      *(str_p+n) = *(str1+n);
		      n++;
		    }

		    //3：str2 : world 循环赋值
		    int m=0;
		    while(1)
		    {
		      if(*(str2+m) == '\0')
		        break;
		      *(str_p+n+m) = *(str2+m);

		      m++;
		    }

		    //4：返回拼接成功的字符串的首地址
		    return str_p;
		  }

		  return 0;
		}


		int main()
		{
		  char str1[] = "\0";
		  char str2[] = "world";

		  char * str_p = My_Strcat(str1,str2);
		  if(str_p == NULL)
		  {
		    printf("拼接失败！\n");
		    return -1;
		  }
		  else
		  {
		    printf("拼接成功！，数据为%s\n",str_p);
		    free(str_p);
		  }

		  return 0;
		}



四：
  
  4.1 数组指针
  
		他是一个指针变量，存放的是数组的地址。
	
	数组指针变量的定义：
	
		int (*array_p)[]; 
		int (*array_p)[10];
		
	赋值：
			int array[10];
			array_p = array; //错的
			array_p = &array[0]// 错的 首元素的地址不能放在数组指针变量里面，
								只能放在int *里面
			array_p = &array;//数组array的地址（数组的地址） 对的
			
	int main()
	{
		int array[10] = {1,2,3,4,5};
		//定义一个数组指针变量
		int (*array_p)[] = &array;
		
		return 0;
	}

  练习：
		1 根据数组指针的解引用，遍历打印数组里面每一个元素。
		2：
			根据数组指针变量的用法，实现通过操作数组指针变量，让数组里面的
			元素从小到大排列。



		/*数组指针变量的定义和赋值以及解引用*/
		#include <stdio.h>

		int main()
		{
			int array[10] = {1,2,6,0,4};
			//定义一个数组指针变量
			int (*array_p)[] = &array;
			//3对应的元素的地址是下标为2的元素的地址
			
			printf("array[2] = %d\n",*((*array_p)+2));
			printf("array[2] = %d\n",(*array_p)[2]);
			//1：先获取到数组名字（先找数组的地址，然后在解引用）
			//2：获取首元素的地址
			//3:把首元素的地址+2.跳到3数据对应的地址(获取3对应的元素的地址)
			//4：根据3对应的元素的地址进行解引用
			
			/*
			1：array     是什么? 表示一个可以存放10个整形数据的数组，首元素的地址
			2: array+1   是什么？array的第二个元素的地址
			3：array_p   是什么？数组指针变量，存放的是array数组的地址
			4: *array_p	 是什么？array数组的地址解引用获取到的是数组array(数组名)
									也就是array数组的首元素地址
			4.1：&array_p是什么？数组指针变量的地址
				int a = 11;
				int *b = &a; 
				*(&(*b)) = 11;
				
			5：(*array_p)+1是什么？array数组的第二个元素地址
			6：*((*array_p)+1)是什么？array数组的第二个元素地址解引用，就是2
			
			*/
			return 0;
		}
  

		/*
		根据数组指针变量的用法，实现通过操作数组指针变量，让数组里面的
					元素从小到大排列。
		  
		*/

		#include <stdio.h>

		int main()
		{
			int array[10] = {1,2,6,0,4};
			//定义一个数组指针变量
			int (*array_p)[] = &array;
			
			int * new_p = *array_p; //把array的首元素的地址给new_p
			//3对应的元素的地址是下标为2的元素的地址
			int data;
			for(int n=0; n<5; n++)
				printf("%d ",(*array_p)[n]);
			printf("\n");
			for(int n=0; n<4; n++)
			{
				for(int m=n+1; m<5; m++)
				{
					if(new_p[n] > new_p[m])
					{
						data 		  = new_p[n]; //*(new_p+n)
						new_p[n] = new_p[m];
						new_p[m] = data;
					}
				}
			}
			
			for(int n=0; n<5; n++)
				printf("%d ",(*array_p)[n]);
			printf("\n");
			
			return 0;
		}


  
  指针数组
	int o()
	{	
		printf("---o---\n");
		return 0;
	}
	
	int main()
	{
		int (*q)() = &o;
		int (*(*p())) = &q;
		
		(**p)();
		return 0;
	}

  
  


		#include <stdio.h>
		#include <errno.h>
		#include <stdlib.h>
		#include <string.h>

		char * My_Strcat(char * str1,char *str2);
		char * My_Strcat(char * str1,char *str2)
		{
		  //考虑能不能拼接
		  /*1：*str2 == ‘\0’,str2没有字符串数据*/
		  /*2：str1对应的空间不够存放  主动申请，不需要判断*/
		  /*3：参数为NULL*/

		  if (str1 == NULL || str2 == NULL)
		  {
		    printf("字符串操作的地址为空！\n");
		    return NULL;
		  }
		  else if(*str2 == '\0')
		  {
		    printf("str2为空，不能拼接\n");
		    return NULL;
		  }
		  else
		  {
		    //在进行拼接  --- 遍历字符串，在赋值
		    //1：申请空间
		    char * str_p = (char *)malloc((strlen(str1)+strlen(str2))*sizeof(char));
		    if(str_p == NULL)
		    {
		      perror("malloc:");
		      return NULL;
		    }
		    //2：获取str1对应的字符串最后的地址 （循环遍历）  //str1 : hello   str1指向的地址  ，需要得到一个指向o后面的地址
		    //第一种方法，str1没有改变
		    int n=0;
		    while(1)
		    {
		      if(*(str1+n) == '\0') //判断是否到字符串的末尾
		        break;

		      *(str_p+n) = *(str1+n);
		      n++;
		    }

		    //3：str2 : world 循环赋值
		    int m=0;
		    while(1)
		    {
		      if(*(str2+m) == '\0')
		        break;
		      *(str_p+n+m) = *(str2+m);

		      m++;
		    }

		    //4：返回拼接成功的字符串的首地址
		    return str_p;
		  }

		  return 0;
		}


		int main()
		{
		  char str1[] = "\0";
		  char str2[] = "world";

		  char * str_p = My_Strcat(str1,str2);
		  if(str_p == NULL)
		  {
		    printf("拼接失败！\n");
		    return -1;
		  }
		  else
		  {
		    printf("拼接成功！，数据为%s\n",str_p);
		    free(str_p);
		  }

		  return 0;
		}

  
  
  
 作业：
	1：在系统里面找一下NULL的本身是什么。
	
	2：找一下具有不让编译器优化的关键词。


	1：在系统里面找一下NULL的本身是什么
	答：
	    在路径：/usr/src/linux-headers-4.15.0-45/include/linux/stddef.h
	    定义：#define NULL ((void *)0)

	2：找一下具有不让编译器优化的关键词。
	答：
	    volatile：特征修饰符，作用是使编译器不优化指定内容，并且可以直接读取内容，一般存放在寄存器中。
	    restrict：类型限定符，告诉编译器只能通过指针来改变指定内存的内容




	    1、在系统里面找一下NULL的本身是什么。
		答：
		NULL，零数据接收器，从/dev/null读取总是返回文件的结尾（例如read(2)返回0），而从/dev/zero读取总是返回包含0的字节(' \ 0 '字符)。

		2、找一下具有不让编译器优化的关键词。
		答：volatile
		一般说来，volatile用在如下的几个地方：
		①中断服务程序中修改的供其它程序检测的变量需要加volatile；
		②多任务环境下各任务间共享的标志应该加volatile；
		③存储器映射的硬件寄存器通常也要加volatile说明，因为每次对它的读写都可能由不同意义。
		另外，以上这几种情况经常还要同时考虑数据的完整性（相互关联的几个标志读了一半被打断了重写），在①中可以通过关中断来实现，②中可以禁止任务调度，③中则只能依靠硬件的良好设计了。


		NULL本质是一个宏定义 ，c++程序中是0 在c语言中是（（void *）0）

		2：找一下具有不让编译器优化的关键词。
		关键词 ：volatile
		用volatile关键词声明变量表示该变量随时都会发生不可预测的变化，编译器将不对这些变量进行编译优化。


		1：在系统里面找一下NULL的本身是什么。
		nicai@nicai:/mnt/hgfs/courseware/day01/memory$ find /usr -name stddef.h
		/usr/src/linux-headers-5.3.0-61/include/linux/stddef.h
		/usr/src/linux-headers-5.3.0-61/include/uapi/linux/stddef.h
		/usr/src/linux-headers-5.3.0-62/include/linux/stddef.h
		/usr/src/linux-headers-5.3.0-62/include/uapi/linux/stddef.h
		/usr/include/linux/stddef.h
		/usr/lib/gcc/x86_64-linux-gnu/7/include/stddef.h
		/usr/lib/llvm-6.0/lib/clang/6.0.0/include/stddef.h
		在/usr/src/linux-headers-5.3.0-61/include/linux/stddef.h路径下可以找到
		定义：
		#ifndef _LINUX_STDDEF_H
		#define _LINUX_STDDEF_H

		#include <uapi/linux/stddef.h>

		#undef NULL
		#define NULL ((void *)0)


		2：找一下具有不让编译器优化的关键词。
		Volatile关键词，volatile是一个类型修饰符（type specifier），它是被设计用来修饰被不同线程访问和修改的变量。作用是使编译器不优化指定内容，并且可以直接读取内容，一般存放在寄存器中。
		编译器在用到这个变量时必须每次都小心地重新读取这个变量的值，而不是使用保存在寄存器里的备份。
		对于volatile类型的变量，系统每次用到他的时候都是直接从对应的内存当中提取，而不会利用cache当中的原有数值，以适应它的未知何时会发生的变化，系统对这种变量的处理不会做优化——显然也是因为它的数值随时都可能变化的情况。


### 省钱利器：
![](/hexo-private-blog-website/images/淘宝客22.jpg)
![](/hexo-private-blog-website/images/淘宝客23.jpg)
![](/hexo-private-blog-website/images/淘宝客24.jpg)

### NULL:
![](/hexo-private-blog-website/images/NULL.png)
![](/hexo-private-blog-website/images/NULL1.png)
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