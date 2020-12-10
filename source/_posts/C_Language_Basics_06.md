---
title: C_Language_Basics_06
date: 2020-07-27 15:31:18
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

	山外青山楼外楼，西湖歌舞几时休。
	暖风熏得游人醉，直把杭州作汴州。

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
	 一、
	二维数组

	 	
	回顾：
				
	一维数组：
	int a[常量表达式];  
	int a[2*2];
				
	二维数组：int b[常量表达式][常量表达式];


	  可以把二维数组看做是一维数组 二维数组的行和列可以看做是一维数组的个数加上个数里面的元素个数	
	二维数组特点：

					
	int a[行][列];  列数一定要在定义的时候确定，行数可以不用
	 就是说一维数组里面的元素个数要确定				
	int a[][3];

		
	二维数组定义：

					
	int a[2][2];
					
	int a[][2];

		
	初始化：

					
	int a[2][2] = {12,13,14,15};

					
	int a[2][2] = {{12,13},{14,15}};

					
	int a[2][2] = {{12},{14,15}};
					
	int a[2] = {1};

		
	注意：数组有初始化，但是没有全部初始化的时候，没有初始化的元素都默认为0



	补充：

	2、二维数组的行地址与列地址
	1）行地址
	1、二维数组中，数组名a的值，是数组a首元素a[0][0]的地址，即&a[0][0]，第一行第一个元素的地址；

	2、二维数组中，数组名a+1是数组a的元素a[1][0]的地址，即&a[1][0]，第二行第一个元素的地址；

	2）列地址
	1、二维数组中，a[0]的值，即该数组的首元素a[0][0]的地址，即&a[0][0]；

	2、二维数组中，a[0]+1的值，是数组元素a[0][1]的值，即&a[0][1]；

	3）混合一下
	1、二维数组中，“a[0]+1”是指向数组元素a[0][1]的地址，“a[1]+2”是指向数组元素a[1][2]的地址；

	2、同样的，二维数组中，“*(a+1)+2”是指向数组元素a[1][2]的地址，与“a[1]+2”相等；

	注：

	*（a+1）表示第2行的行地址；

	*a+1表示第一行第二个元素的地址；

	3、二维数组中，*(*(a+1)+2))是数组元素a[1][2]的值！！！


	/*
	*copyright(c) 2018,HH
	*All rights reserved.
	*作 者：HH
	*完成日期：2018年7月25日
	*版本号：v1.0
	*
	*问题描:二维数组，元素地址的表示，行列地址的表示；
	*输入描述：；
	*程序输出：
	*/
	 
	#include <stdio.h>
	int main()
	{
	    int a[2][3]={{1,2,3},{4,5,6}};
	    int i,j;
	    int *p1,*p2,*p3,*p4,*p5,*p6;
	    printf("array a is :\n");
	    for(i=0;i<2;i++)
	    {
	        for(j=0;j<3;j++)
	        {
	            printf("%d ",a[i][j]);
	        }
	        printf("\n");
	    }
	    printf("array b is :\n");
	    p1=a;//二维数组的数组名，即首元素a[0][0]的首地址；
	    p2=a+1;//a+1是数组a的元素a[1][0]的地址，即&a[1][0]
	    p3=a[0];//同a[0][0]的地址，即第一行第一个元素的地址
	    p4=a[1];//同a[1][0]的地址，即第二行第一个元素的地址
	    printf("p1=a指向的值是：%d;地址是：%d\n",*p1,p1);
	    printf("p2=a+1指向的值是：%d;地址是：%d\n",*p2,p2);
	    printf("p3=a[0]指向的值是：%d;地址是：%d\n",*p3,p3);
	    printf("p4=a[1]指向的值是：%d;地址是：%d\n",*p4,p4);
	    p5=a[0]+1;//指向第一行，第二列元素的地址；
	    p6=a[1]+1;//指向第二行，第一列元素的地址；
	    printf("p5=a[0]+1指向的值是：%d;地址是：%d\n",*p5,p5);
	    printf("p6=a[1]+1指向的值是：%d;地址是：%d\n",*p6,p6);
	    printf("*(a+1)指向的地址是：%d\n",*(a+1));
	    printf("*(*(a+1))指向的值是：%d\n",*(*(a+1)));
	    printf("*(a+1)+2指向的地址是：%d\n",(*(a+1)+2));
	    printf("*(a+1)+2指向的值是：%d\n",*(*(a+1)+2));
	    printf("*a+2指向的值是：%d;地址是：%d\n",*(*a+2),*a+2);
	    return 0;
	}


	3）行地址，列地址的等价写法
	注：在二维数组a[i][j]中，a[i]是“行名”，等价于指针；

	a[0]等价于a，都表示指针；

	a[1]等价于a+1，都表示指针；

	a[1]+1等价于*(a+1)+1，都表示指针；

	注意防止越界！


### 二维数组:
![](/hexo-private-blog-website/images/二维数组.png)
![](/hexo-private-blog-website/images/二维数组1.png)


### 支付宝付款:
![](/hexo-private-blog-website/images/alipay.jpg)

		
	练习：
				
	自己初始化一个二维数组，标准输出（按矩形形状输出）

				
	定义一个二维数组，求该二维数组対角线合

				
	打印二维数组中数组的首地址，首元素的地址，数组对应的数据值（%p）

				
	打印二维数组中每一个一维数组的地址


		#include <stdio.h>


		int main()
		{
			int array[2][2] = {{2,3},4};

			for(int x=0; x<2; x++)
				for(int y=0; y<2; y++)
					printf("array[%d][%d] = %d\n",x,y,array[x][y]);
			

			return 0;
		}


		#include <stdio.h>


		int main()
		{
			int array[3][3] = {1,2,3,4,5,6,7,8,9};

			for(int x=0; x<3; x++)
			{
				for(int y=0; y<3; y++)
				{
					printf("%5d ",array[x][y]);
				}
				printf("\n");
			}
			return 0;	
		}


		#include <stdio.h>


		int main()
		{
			int array[2][2];

			for(int n=0; n<2; n++)
				printf("array[%d]的地址为%p\n",n,array[n]);

			printf("%p--%p\n",&array[0],&array[1]);

			printf("%p--%p\n",&array[0],(array+1));//array是数组的名字，也就是首元素的地址

			int a[2];

			printf("%p--%p\n",&a[0],&a[1]);
			
			printf("%p--%p\n",a,a+1);
			printf("%p--%p\n",a,++a);//a a++是错的，a是常量，不能自身加减
			

			return 0;
		}


		#include <stdio.h>
		#include <errno.h>
		#include <stdlib.h>

		/*指针变量写出数组的形式*/
		int main()
		{
		  int * p = (int *)malloc(4*sizeof(int));//1
		  if(p == NULL)
		  {
		    perror("malloc ");
		    return -1;
		  }

		  p[0] = 4;
		  p[1] = 5;

		  for(int n=0; n<4; n++)
		    printf("%d\n",p[n]);

		  free(p);

		  return 0;
		}


		#include <stdio.h>
		#include <stdlib.h>
		#include <errno.h>

		int main()
		{
		  char * c = (char *)malloc(20*sizeof(char));
		  if(c == NULL)
		  {
		    perror("malloc");
		    return -1;
		  }

		  *c = 'A';
		  *(c+1) = 'B';

		  printf("%c--%c\n",*c,*(c+1));
		  printf("%c\n",(*c)++);
		  printf("%c\n",*c);
		  /*c存放的地址没变，变的是对应的数据A+1 = B*/

		  free(c);
		  return 0;
		}


		
	int A(int a)//形参
		
	{
			
	a+=1;
			
	return 0;
		
	}

		
	int B()
		
	{
			
	int a = 10;//实参
			
	A(a);
			
	return 0;
		
	}


		
	------------------------------------------------
		
	int A(int array_1[])//形参
		
	{
			
	array_1[0]+=1;
			
	return 0;
		
	}

		
	int B()
		
	{
			
	int array_2[1] = 2; //实参
			
	A(array_2);
			
	return 0;
		
	}

		
	array_1和array2的变量名字不同，但是都是存放&array_2[0]的地址也是&array_2的值

		
	普通的数据值和地址值有区别的



	---------------------------------------------------------------------------------------------------------


	二、指针类型


		
	编码中的指针：变量的地址
		
	编码中：叫指针变量

		
	变量的作用：存放数据的(指针变量用来存放变量的地址的)

		
	int a = 10; a

		
	基本指针变量的分类：
				
	整形：int
				
	整形指针类型： 
	int  *
				
	字符指针类型： 
	char *

		
	定义指针变量

				
	整形指针变量：int * a;
				
	字符指针变量：char *a;

		
	初始化与赋值指针变量：

			
	初始化：

				
	int b;

				
	int * a = &b; a

				
	char c;

				
	char *d = &c; d

				
	printf("指针变量a存放的地址是%p---b的地址%p\n",a,&b);

				
	printf("指针变量d存放的地址是%p---c的地址%p\n",d,&c);

		
	赋值：

				
	int *a;//野指针（没有初始化的指针变量，里面存放的是随机值）

				
	int b;

				
	把b的地址存放到指针变量a里面

				
	*a = &b;错的
				
	*a = b;错的
				 
	a = &b;
				
	printf("%d\n",*a);//使用a里面的随机地址，非法使用（段错误）

				
	printf("%p\n",a);

				
	补充：避免野指针 int * a = NULL;
				
	printf("%d\n",*a);//肯定会段错误

				
	总结：
						
	整形变量b的地址存放指针变量a里面，但是指针变量a有自己的地址


		#include <stdio.h>

		int main()
		{
		/*	
			int b;
			int * a = &b; 

			char c;

			char *d = &c; 

			printf("指针变量a存放的地址是%p---b的地址%p\n",a,&b);

			printf("指针变量d存放的地址是%p---c的地址%p\n",d,&c);

			printf("指针变量a的地址为%p\n",&a);
		*/

			int *a;//野指针（没有初始化的指针变量，里面存放的是随机值）

			printf("%d\n",*a);//使用a里面的随机地址，非法使用（段错误）

			return 0;
		}


		#include <stdio.h>



		int A(int * xxx)
		{
			printf("%p\n",xxx);

			return 0;	
		}


		int main()
		{
			int b;

			printf("%p\n",&b);
			A(&b);

			return 0;	
		}


		#include <stdio.h>


		int main()
		{
		  int a = 10;
		  int *b = &a;

		  printf("*b = %d---a = %d\n",*b,a);

		  /*更改b里面的地址对应的数据，看看变化*/
		  *b += 11;

		  printf("*b = %d---a = %d\n",*b,a);


		  return 0;
		}



		#include <stdio.h>

		/*传指针和传普通数据的不同*/


		int A(int * a);
		/* 两边数据不会互相影响
		int A(int a)
		{
		  a+=10;

		  printf("%d\n",a);  //21

		  return 0;
		}
		*/

		int A(int * a)
		{
		  *a+=10;

		  printf("A函数里面的*a = %d\n",*a);

		  return 0;
		}

		int main()
		{
		  int a = 11;
		  int b = 12;
		  int *c = &b;
		  A(&a);

		  printf("a = %d\n",a);// 21

		  int d = (*c)++;  //先解应用获取数据12，在输出，最后在+1，本质*c对应的数据值为13

		  printf("%p\n",c);
		  int e = *(c++);  //先地址完后移动一，在解引用
		  printf("&e = %p---c = %p\n",&e,c);
		  int g = *(++c);

		  printf("&g = %p\n",&g);

		  printf("%d --- %d --- %d\n",d,e,g);


		  /*
		    *c++   先解引用 在对应的数据+1
		    *(c++) 先地址加一，在解引用
		    (*c)++ 先解引用 在对应的数据+1

		    char A[] = "yuoi";
		    char *b = &A;

		    *(b++) = 'u'  *b++ =  'z'


		  */

		  return 0;
		}



		
	指针变量的作用：
					
	1：存放变量的地址（指针变量的类型要和存放地址对应的变量的类型一致）
					
	2：根据指针变量存放的地址找到地址上面存放的数据（解引用，需要用到*）

	  
	*什么时候需要写？
	      
	定义指针变量(初始化)的时候：
	int *a;  //*这时候只起到标志的作用
	      
	解引用：
	            
	int a = 10;
	            
	int *b = &a; //把a变量的首地址存放在指针变量b里面。

	            
	printf("%d---%d\n",*b,a); //*b:根据b变量里面存放的地址获取对应数据10

	        
	如果根据b指针变量里面的地址，改变其地址上面的数据值，那么有什么变化？
	        
	a对应的数据值也会跟着改变，因为b里面存放的地址就是a的首地址。
	    
	补充：关于地址与++ --

	      
	*c++   先解引用 在对应的数据+1 //有待商榷
	      
	*(c++) 先地址加一，在解引用
	      
	(*c)++ 先解引用 在对应的数据+1

	      
	char A[] = "yuoi";
	      
	char *b = &A;

	      
	*(b++) = 'u'  *b++ =  'z'


	              
	简单的指针变量传参：

	                	
	int A(int * xxx)
	                	
	{
	                		
	printf("%p\n",xxx);

	                		
	return 0;
	                	
	}


	                	
	int main()
	                	
	{
	                			
	int b;
	                			
	int *a = &b;

	                			
	printf("%p---%p\n",a,&b);
	                			
	A(a);

	                			
	return 0;
	                	
	}


			
	练习：

					
	1 定一个整形数组，把数组里面元素的地址传给自己定义一个函数，在这个函数里面打印数组元素的地址.



	        
	2 封装一个函数，进行指针变量传参（两个形参），在这个函数里面实现数据值互换

	          
	int A(int * data_1,int * data_2)
	          
	{
	            
	/*把data_1对应的数据值和data_2对应的数据值互换*/
	            
	return 0;
	          
	}

	          
	第一种：

	              
	int Fun(int * data_1,int * data_2);
	              
	int Fun(int * data_1,int * data_2)
	              
	{
	                
	printf("改变之前：%d---%d\n",*data_1,*data_2);
	                
	int *data = data_1;
	                
	data_1 = data_2;
	                
	data_2 = data;
	                
	printf("改变之后：%d---%d\n",*data_1,*data_2);

	                
	return 0;
	              
	}


	     
	         
	int main()
	              
	{
	                
	int data_1 = 11;
	                
	int data_2 = 12;

	                
	Fun(&data_1,&data_2);

	                
	return 0;
	              
	}


		code：


		#include <stdio.h>
		#include <stdlib.h>
		#include <errno.h>

		int main()
		{
		  char * c = (char *)malloc(20*sizeof(char));
		  if(c == NULL)
		  {
		    perror("malloc");
		    return -1;
		  }

		  *c = 'A';
		  *(c+1) = 'B';

		  printf("%c--%c\n",*c,*(c+1));
		  printf("%c\n",(*c)++);
		  printf("%c\n",*c);
		  /*c存放的地址没变，变的是对应的数据A+1 = B*/

		  free(c);
		  return 0;
		}


		#include <stdio.h>
		#include <errno.h>
		#include <stdlib.h>

		int main()
		{
		  int * p = (int *)malloc(4*sizeof(int));//1
		  if(p == NULL)
		  {
		    perror("malloc ");
		    return -1;
		  }
		  int *bf = p;
		  /*
		  int * p = NULL;
		  p = (int *)malloc(4);//2
		  */

		  *p = 4;
		  *(p+1) = 5;
		//  (*p)+1 = 5;错误

		  printf("%p---%p\n",p,p+1);

		  printf("%d--%d\n",*p,*(p+1));
		  printf("%p\n",*p++);

		  free(bf);

		  return 0;
		}



		#include <stdio.h>
		/*2 封装一个函数，进行指针变量传参（两个形参），在这个函数里面实现数据值互换*/
		int Fun(int * data_1,int * data_2);
		int Fun(int * data_1,int * data_2)
		{
		/*鸡肋 只互换形参data_1和data_2*/
		  int *data = data_1;
		  data_1 = data_2;
		  data_2 = data;


		  return 0;
		}

		int Fun_2(int * data_1,int * data_2);
		int Fun_2(int * data_1,int * data_2)
		{
		  //要根据地址更改对应的数据才会让原始的变量对应的数据发生改变
		  int data;
		  data    = *data_1;
		  *data_1 = *data_2;
		  *data_2 = data;

		  return 0;
		}


		int main()
		{
		  int data_1 = 11;
		  int data_2 = 12;

		  printf("改变之前：%d---%d\n",data_1,data_2);

		  Fun_2(&data_1,&data_2);

		  printf("改变之后：%d---%d\n",data_1,data_2);

		  return 0;
		}



	-------------------------------------------------------------------------------------------------------------------------------------------------

	三、自己手动给指针变量申请内存空间

	  
	自动申请：
	        
	int a = 10;
	        
	int *b = &a;

	  
	手动申请：

	      
	申请内存空间的API，（专门给指针变量申请）：
	            
	申请：malloc,calloc
	            
	释放：free


	      
	man -f malloc

	      
	头文件：#include <stdlib.h>
	      
	函数原型：

	          
	void *malloc(size_t size);

	          
	返回值类型：void * （无类型指针---万能指针）
	          
	返回值：判断函数的调用情况

	            
	申请内存空间成功：
	                          
	返回申请出来的内存空间的首地址（一个字节的地址）
	            
	申请内存空间失败：
	                          
	NULL

	              
	形参一：
	                    
	size：设置申请空间的大小（字节为单位）

	        
	例子1：自己定义一个整形指针变量，使用malloc给这个指针变量申请4个字节的内存空间

	        
	#include <stdio.h>
	        
	#include <stdlib.h>

	        
	int main()
	        
	{
	          
	int * p = (int *)malloc(4);//1

	          
	/*
	          
	int * p = NULL;
	          
	p = (int *)malloc(4);//2
	          
	*/

	          
	*p = 4;

	          
	printf("%d--%p\n",*p---p);
	          
	return 0;
	        
	}


	        
	例子2：
	自己定义一个字符指针变量，使用malloc给这个指针变量申请20个字节的内存空间

	          
	char char_data = 'A';
	          
	char * c = &char_data;

	          
	char *p = "helloworld";  
	//char p[] = "helloworld";

	          
	//p存放的是字符h的地址

	          
	malloc---char:

	            
	char * c = (char *)malloc(20*sizeof(char));
	            
	if(c == NULL)
	            
	{
	              
	perror("malloc");
	              
	return -1;
	            
	}

	            
	*c = 'A';
	            
	*(c+1) = 'B';

	            
	printf("%c--%c\n",*c,*(c+1));

	  
	一维数组补充：

	        
	数组是一个特殊的指针 （数组的名字是数组首元素的地址）

	        
	int array[10] = {1,2,4};

	        
	array[0];//第一个元素   数据：1
	   array=&array[0]=*array     
	*(array+1): 数据：2
	        
	array:就是array[0]的地址
	        
	array+1:就是array[1]的地址
	        
	*(array+1)://第二个元素array[1]

	        
	for(int n=0; n<10; n++)
	          
	printf("%d\n",array[n]);

	          
	for(int n=0; n<10; n++)
	            
	printf("%d\n",*(array+n));



	补充：指针变量当中函数的返回值：

	    
	int * Fun(int * p)
	    
	{
	      
	(*p)++;

	      
	return p;
	    
	}

	    
	int * Fun_2()
	    
	{
	      
	int *q = 11;
	      
	return q;
	    
	}

	    
	int main()
	    
	{
	      
	int p = 10;
	      
	int *q = Fun_2();
	      
	printf("%d\n",*q); // 11
	  
	//p的地址与q存放的地址是否一样？ 一样的
	      
	printf("%p---%p\n",&p,q);
	      
	return 0;
	    
	}


	    
	//局部变量： 对应函数退出时，才会释放对应的内存空间 （定义在函数里面）
	      
	全局变量：程序退出时，才会释放对应的内存空间       （定义在函数外面）

	    
	局部变量：存放在内存中的栈空间
	    
	手动申请的：malloc,calloc(在堆空间里面的)



	    
	练习：使用指针变量，封装一个模仿strcat函数。


	  
	作业：
	      
	自己在系统的头文件里面找size_t究竟本身是什么类型？
	      
	使用指针变量，封装一个模仿strcpy与strlen的函数。



	自己在系统的头文件里面找size_t究竟本身是什么类型？
	在/usr/lib/gcc/x86_64-linux-gnu/7/include下可以找到
	定义：
	#define __SIZE_TYPE__ long unsigned int
	typedef __SIZE_TYPE__ size_t;
	size_t 类型定义在cstddef头文件中，该文件是C标准库的头文件stddef.h的C++版。它是一个与机器相关的unsigned类型，其大小足以保证存储内存中对象的大小。   
	使用指针变量，封装一个模仿strcpy与strlen的函数。
	①模仿strcpy函数
	#include "stdio.h"
	#include "stdlib.h"

	char *strcpy_pointer(char *dest,char *src); //函数的声明

	char *strcpy_pointer(char *dest,char *src)
	{
		int n=0;
		dest = (char *)malloc(sizeof(src)*sizeof(char));
		while(*src != '\0') //判断字符串是否为空
		{
			*(dest + n) = *src;
			src++;
			n++;
		}
		return dest;
	}

	int main(int argc, char const *argv[])
	{
		char *str_1 = "hello";
		char *str_2 = "world";

		char *strcp_ret = strcpy_pointer(str_1,str_2);

		printf("%s\n", strcp_ret);

		return 0;
	}



	②模仿strlen函数
	#include "stdio.h"

	// 函数原型: size_t strlen(const char *s);
	int strlen_pointer(char *p); //函数的声明

	int strlen_pointer(char *p)
	{
		int len = 0;
		while(*p != '\0') //判断字符串是否为空
		{
			len++;
			p++;
		}
		return len;

	}

	int main(int argc, char const *argv[])
	{
		char *str_1 = "helloworld";

		int ret = strlen_pointer(str_1);

		printf("%d\n", ret);

		return 0;
	}



		1、学习realloc函数
		答：函数原型：void *realloc(void *ptr, size_t size)
		    函数作用：更改已经配置的内存空间，即更改由malloc()函数分配的内存空间的大小。
		    函数类型：void
		    返回值：调用成功则会返回一个指针，不成功则返回NULL
		    参数1：是一个指针类型，表示需要改变已存在的指针
		    参数2：改变空间的大小，单位是字节（byte）

		2、在系统的头文件里面找size_t究竟本身是什么类型？
		答：在路径：/usr/lib/gcc/x86_64-linux-gnu/5/include/stddef.h
		    定义：typedef __SIZE_TYPE__ size_t;
		         #define __SIZE_TYPE__ long unsigned int
		    根据头文件的定义，size_t是无符号长整型，有8个字节



		3、使用指针变量，封装一个模仿strcpy与strlen的函数。
		答：1）模仿strcpy函数：
		    #include <stdio.h>
		    #include <stdlib.h>

		    char *strcpy_func(char *dest, char *src);

		    char *strcpy_func(char *dest, char *src)
		    {
		        int j = 0;
		        dest = (char *)malloc(sizeof(src)*sizeof(char));
		        while(*src != '\0')  //传递str2给str1
		    	{
		    		*(dest + j) = * src ;
		    		src++;
		    		j++;
		    	}

		        return dest;
		    }

		    int main()
		    {
		        char *str1 = "abc";
		        char *str2 = "df";

		        char *strcp = strcpy_func(str1, str2);

		        printf("%s\n", strcp);

		        return 0;
		    }

		2)模拟strlen函数
		答：
		    #include <stdio.h>

		    int strlen_func(char *src);

		    int strlen_func(char *src)
		    {
		        int len = 0;

		        while(*src != '\0') //计算非空字符
		        {
		                len++;
		                src++;
		        }

		        return len;
		    }

		    int main()
		    {
		        char *str = "abcd";

		        printf("%d\n", strlen_func(str));
		    }


	  
	malloc:

	      
	注意返回的指针变量，不要改变自身的数据值，因为后面free的时候需要的用到原始指针变量（首地址）

	      
	关于字符串拼接：

	              
	注意了，每一个字符串都有自己的的首地址，要记住每部分代码段中首地址的位置

	      
	返回值：指针类型，注意是不是返回-1



		#include <stdio.h>
		#include <stdlib.h>
		#include <errno.h>

		int main()
		{
		  char * c = (char *)malloc(20*sizeof(char));
		  if(c == NULL)
		  {
		    perror("malloc");
		    return -1;
		  }

		  *c = 'A';
		  *(c+1) = 'B';

		  printf("%c--%c\n",*c,*(c+1));
		  printf("%c\n",(*c)++);
		  printf("%c\n",*c);
		  /*c存放的地址没变，变的是对应的数据A+1 = B*/

		  free(c);
		  return 0;
		}



		#include <stdio.h>
		#include <errno.h>
		#include <stdlib.h>

		int main()
		{
		  int * p = (int *)malloc(4*sizeof(int));//1
		  if(p == NULL)
		  {
		    perror("malloc ");
		    return -1;
		  }
		  int *bf = p;
		  /*
		  int * p = NULL;
		  p = (int *)malloc(4);//2
		  */

		  *p = 4;
		  *(p+1) = 5;
		//  (*p)+1 = 5;错误

		  printf("%p---%p\n",p,p+1);

		  printf("%d--%d\n",*p,*(p+1));
		  printf("%p\n",*p++);

		  free(bf);

		  return 0;
		}




		#include <stdio.h>
		#include <stdlib.h>
		#include <errno.h>
		#include <string.h>

		char * My_strcat(char * str_1,char * str_2);

		char * My_strcat(char * str_1,char * str_2)
		{
		  char * new_p = NULL;
		  if(*str_2 == '\0')
		  {
		    printf("没有数据拼接！\n");
		    return NULL;
		  }
		  else
		  {
		    new_p = (char *)malloc((strlen(str_1)+strlen(str_2))*sizeof(char));
		    if(new_p == NULL)
		    {
		      perror("malloc");
		      return NULL;  //return (char *)-1;
		    }
		    else
		    {
		      printf("%ld\n",(strlen(str_1)+strlen(str_2))*sizeof(char));
		    }
		    strcpy(new_p,str_1);
		    new_p[strlen(new_p)] = '\0';
		  }

		  int n=0;
		  while(1)//循环让str_1指向对应字符串的尾部的地址
		  {
		    if((*(new_p+n)) == '\0')  break;
		    new_p+n;
		    n++;
		  }

		  printf("----\n");
		  int m=0;
		  while(1)//开始拼接
		  {
		    if(*(str_2+m) == '\0')break;
		    *(new_p+n) = *(str_2+m);

		    n++;
		    m++;

		  }

		  return new_p;
		}



		int main()
		{
		  char * str_1 = "hello";
		  char * str_2 = "world";

		  char * new_p = My_strcat(str_1,str_2);
		  printf("%s\n",new_p);

		  free(new_p);
		  return 0;
		}



	  
	剩下：
	      
	指针变量的大小：
	      
	指针变量与void*的相互转换
	      
	callocAPI
	      
	数组指针
	      
	指针数组
	      
	函数指针
	      
	结构体指针





		#include <stdio.h>
		#include <stdlib.h>
		#include <errno.h>
		#include <string.h>

		char * My_strcat(char * str_1,char * str_2);

		char * My_strcat(char * str_1,char * str_2)
		{
		  char * new_p = NULL;
		  if(*str_2 == '\0')
		  {
		    printf("没有数据拼接！\n");
		    return NULL;
		  }
		  else
		  {
		    new_p = (char *)malloc((strlen(str_1)+strlen(str_2))*sizeof(char));
		    if(new_p == NULL)
		    {
		      perror("malloc");
		      return NULL;  //return (char *)-1;
		    }
		    else
		    {
		      printf("%ld\n",(strlen(str_1)+strlen(str_2))*sizeof(char));
		    }
		    strcpy(new_p,str_1);
		    new_p[strlen(new_p)] = '\0';
		  }

		  int n=0;
		  while(1)//循环让str_1指向对应字符串的尾部的地址
		  {
		    if((*(new_p+n)) == '\0')  break;
		    new_p+n;
		    n++;
		  }

		  printf("----\n");
		  int m=0;
		  while(1)//开始拼接
		  {
		    if(*(str_2+m) == '\0')break;
		    *(new_p+n) = *(str_2+m);

		    n++;
		    m++;

		  }

		  return new_p;
		}



		int main()
		{
		  char * str_1 = "hello";
		  char * str_2 = "world";

		  char * new_p = My_strcat(str_1,str_2);
		  printf("%s\n",new_p);

		  free(new_p);
		  return 0;
		}



		1、自己在系统的头文件里面找size_t究竟本身是什么类型？
		答：size_t的全称是size type，是一种用来记录大小的数据类型。size_t是标准C库中定义的，在64位系统中为long long unsigned int，非64位系统中为long unsigned int。一个基本的无符号整数的C / C + +类型，它是sizeof操作符返回的结果类型，该类型的大小可以选择。
		      
		2、使用指针变量，封装一个模仿strcpy与strlen的函数。
		答：
		代码如下：
		//利用指针变量，封装一个模仿strlen和strcpy的函数

		#include <stdio.h>

		int My_strlen(char *str_1);
		char *My_strcpy(char *str_1,char *str_2);

		int n=0;

		int My_strlen(char *str_1)
		{
		    while(*(str_1+n) != '\0')
		    {
		        n++;
		    }
		     return n;
		}

		char *My_strcpy(char *str_1,char*str_2)
		{
		    char *temp = str_1;
		    while((*str_1++ = *str_2++) != '\0');
		    return temp;

		}

		int main()
		{
		    char *str_1="helloworld";
		    char *str_2="hello";

		    My_strlen(str_1);
		    printf("字符串的长度为：%d\n",n);
		    printf("复制前的字符串为：%s\n",str_1);
		    My_strcpy(str_1,str_2);
		    printf("复制后的字符串为：%s\n",str_1);
		    return 0;
		}

		运行时出现段错误，但找不到错误点。



		1.自己在系统的头文件里面找size_t究竟本身是什么类型？
		答：size_t是标准C库中定义的，此类型位于头文件stddef.h中，在64位系统中为long long unsigned int，非64位系统中为long unsigned int。


		2.使用指针变量，封装一个模仿strcpy与strlen的函数。
		答：
		#include <stdio.h>
		#include <stdlib.h>

		int My_strlen(char *str);
		char *My_strcpy(char *str_1,char *str_2);

		int My_strlen(char *str)
		{
			int n = 0;
			int sum = 0;
			while(1)
			{
				if(*(str+n) != '\0')//计算字符个数
				{
					n++;
					sum++;
				}
				else
					break;
			}
			return sum;	
		}

		char *My_strcpy(char *str_1,char *str_2)
		{
			int m = 0;
			
			while(1)
			{
				if((*(str_1+m) = *(str_2+m)) != '\0')
				{
					m++;	
				}
				else
				{
					*(str_1+m) = '\0';	
		break;
				}
			}
			
			return str_1;	
		}

		int main()
		{
			char *str = "helloworld";	
			printf("字符串的长度为：%d\n",My_strlen(str));
			
			char str_1[] = "hello";
			char str_2[] = "world";
			printf("复制前的字符串：%s\n",str_1);
			My_strcpy(str_1,str_2);
			printf("复制后的字符串：%s\n",str_1);
			
			return 0;	
		}




		1、自己在系统的头文件里面找size_t究竟本身是什么类型？
		答：size_t的全称是size type，是一种用来记录大小的数据类型。size_t是标准C库中定义的，在64位系统中为long long unsigned int，非64位系统中为long unsigned int。一个基本的无符号整数的C / C + +类型，它是sizeof操作符返回的结果类型，该类型的大小可以选择。
		      
		2、使用指针变量，封装一个模仿strcpy与strlen的函数。
		答：
		//利用指针变量，封装一个模仿strlen的函数

		#include <stdio.h>
		#include <stdlib.h>

		int n=0;
		int My_strlen(char *str_1);
		char *My_strcpy(char *str_1,char *str_2);

		int My_strlen(char *str_1)
		{
		    
		    while(*(str_1+n) != '\0')
		    {
		        n++;
		    }
		     return n;
		}

		char *My_strcpy(char *str_1,char *str_2)
		{
		    while((*str_1++ = *str_2++) != '\0');
		    return str_1;

		}

		int main()
		{
		    char str_1[]="hello";
		    char str_2[]="helloworld";
		    My_strlen(str_1);
		    printf("字符串的长度为：%d\n",n);
		    printf("复制前的字符串为：%s\n",str_1);
		    My_strcpy(str_1,str_2);
		    printf("复制后的字符串为：%s\n",str_1);
		    return 0;
		}




### 课堂笔记

		栈空间里面的变量地址是动态申请的
		定义的变量一般都是自动变量
		局部变量没有初始化的时候，会默认赋值随机值。
		全局变量没有初始化的时候，系统它会自动清零。
		数值名  数组首元素地址只能放在整型变量里面
		数组指针变量存放的是数组的地址
		数组的地址解引用获取到的是数组名 也就是数组的首元素的地址
		数组指针 只有在解引用之后才是数组的名字也就是数组的首地址
		int * 指向数组首元素的指针

		函数指针变量存放的是函数的地址
		函数的地址解引用获取到的是函数名


### 省钱利器：
![](/hexo-private-blog-website/images/淘宝客20.jpg)
![](/hexo-private-blog-website/images/淘宝客21.jpg)
![](/hexo-private-blog-website/images/淘宝客22.jpg)

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