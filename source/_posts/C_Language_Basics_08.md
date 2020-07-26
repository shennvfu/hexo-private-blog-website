---
title: C_Language_Basics_08
date: 2020-07-28 16:21:18
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).


这是一个链接 [菜鸟教程][Runoob]

[Runoob]: http://www.runoob.com/


# C语言基础
	一、总结
	已学的：
		一维数组（定义、赋值、初始化、传参，数组里面每一个元素的控制）
		二维数组 
		存储类  
		指针函数：他是一个函数，函数的返回值类型的指针类型
		函数指针：他是一个指针，存放函数的地址（通过函数地址解应用（调用函数），回调函数）
		数组指针：
	
	未学的：
		函数指针和数组指针的传参
		指针数组
		函数：宏定义函数 、递归函数、内联函数、变参函数
		C语言程序运行时的内存分布（变量的，代码段）
		结构体：结构体、结构体指针、结构体数组
		枚举
		联合体（共用体）
		
二、函数指针的传参

	//定义一个可以接受函数指针变量的API
	int Fun_B(int (*f_p)());
	int Fun_A();
	
	int Fun_A()
	{
		printf("我是Fun_A\n");
		return 0;
	}
	
	int Fun_B(int (*f_p)())
	{
		(*f_p)();
		return 0;
	}
	
	int main()
	{
		//定义一个函数指针变量存放函数的地址
		int (*f_p)() = 函数的地址；
		Fun_B(f_p);
		
		return 0;
	}
	


			/*函数指针变量的练习*/

			#include <stdio.h>
			char Get_Max(char c_a,char c_b);
			int Working(char (*f_p)(char,char));

			int Working(char (*f_p)(char,char))
			{
				
				char ret = (*f_p)('o','o');
				if(ret == '\0')
				{
					printf("两个字符相等！\n");
				}
				else
				{
					printf("大的字符数据是%c\n",ret);
				}
				
				return 0;
			}

			char Get_Max(char c_a,char c_b)
			{
				if(c_a == c_b)
				{
					return 0;
				}
				else
				{
					return (c_a>c_b?c_a:c_b);
				}
				
				
			}


			int main()
			{
				
				char (*f_p)(char,char) = &Get_Max;
				
				Working(f_p);
				
				return 0;
			}


	
	练习：
			封装一个函数，实现判断两个字符变量的大小，把大的字符变量返回出来。
			在定义一个函数指针变量存放上面的函数的地址，在通过函数指针变量
			解引用调用上面的函数。
			



			#include <stdio.h>

			int Fun_B(int (*f_p)());
			int Fun_A();

			int Fun_A()
			{
				printf("我是Fun_A\n");
				return 0;
			}

			int Fun_B(int (*f_p)())
			{
				(*f_p)();
				return 0;
			}

			int main()
			{
				//定义一个函数指针变量存放函数的地址
				int (*f_p)() = &Fun_A;
				//(*f_p)();
				Fun_B(f_p);
				
				return 0;
			}



			/*指针数组的定义。赋值，初始化*/

			#include <stdio.h>
			#include <stdlib.h>

			int main()
			{
				int a =10;
				
				//int * array[10] = {&a,&a}; 初始化
				int * array[10];
				//malloc循环赋值
				
				for(int n=0; n<10; n++)
					array[n] = (int *)malloc(sizeof(int));
				
				//*(array[n]) = 5;
				for(int n=0; n<10; n++)//循环给指针数组里面每个指针变量赋值数据域
					**(array+n) = n;//*(array[n]) = n;
					
				
				
				for(int n=0; n<10; n++)//循环打印指针数组中变量指向的数据域
					printf("array[%d] = %d\n",n,**(array+n));
				
				
				for(int n=0; n<10; n++)//循环释放内存空间
					free(*(array+n));
				
				
				
				
				return 0;
			}



三、数组指针的传参

	练习：
		定义一个整形数组，初始化。
		把数组的地址存放在一个数组指针变量里面。
		通过函数的传参，把数组指针变量传给另外一个函数，在这个函数里面对数组指针变量解引用，
		打印出数组里面的每一个元素数据。
		
	#include <stdio.h>

	int Fun(int (*a_p)[]);
	int Fun(int (*a_p)[])
	{
		for(int n=0; n<4; n++)
			printf("%d ",*((*a_p)+n));
		printf("\n");
		
		for(int n=0; n<4; n++)
			printf("%d ",(*a_p)[n]);
		printf("\n");
	
		return 0;
	}
	
	
	int main()
	{
		int array[]  = {90,98,78,76};
		int (*a_p)[] = &array; 
		
		Fun(a_p); //Fun(&array);
		
		return 0;
	}
	


		/*数组指针传参*/

		#include <stdio.h>

		int Fun(int (*a_p)[]);
		int Fun(int (*a_p)[])
		{
			for(int n=0; n<4; n++)
				printf("%d ",*((*a_p)+n));
			printf("\n");
			
			for(int n=0; n<4; n++)
				printf("%d ",(*a_p)[n]);
			printf("\n");

			return 0;
		}


		int main()
		{
			int array[]  = {90,98,78,76};
			int (*a_p)[] = &array; 
			
			Fun(a_p); //Fun(&array);
			
			return 0;
		}


补充：
		给数组指针malloc
		
	例子：
		#define M 10
		int (*a_p)[M] = (int(*)[])malloc(M*sizeof(int));
		if(a_p == NULL)
		{
			perror("malloc");
			return -1;
		}
			
	检测：
			数组首元素地址+1，跳多少字节？
			数组首地址+1，跳多少字节？
			
			

			#include <stdio.h>
			#include <stdlib.h>
			#define M 10

			int main()
			{
				
				int (*a_p)[M] = (int(*)[])malloc(M*sizeof(int));
				if(a_p == NULL)
				{
					perror("malloc");
					return -1;
				}
				
				(*a_p)[2] = 12;
				
				printf("%d\n",*((*a_p)+2));
				//printf("%d\n",*((*a_p)+3));
				//printf("%d\n",*((*a_p)+15));
				free(a_p);
				
				return 0;
			}


--------------------------------------------------------------------------------------------
一、指针数组
	
	指针数组：他是一个数组，用来存放多个指针变量。
	
	整形指针数组，只能存放整形变量的地址。
	
	整形指针数组的定义：
					
					int *array[10];
					
	整形指针数组的赋值：		
					
					int a = 10,....;
					第一个元素 = 整形变量的地址 //给数组中第一个元素赋值一个整形变量的地址
					array[0] = &a;
	malloc给指针数组申请空间：
					for(int n=0; n<10; n++)
						array[n] = (int *)malloc(sizeof(int));
						
					....
					for(int n=0; n<10; n++)
						free(array[n]);
						
	array：    是一个指针数组
	array+1:   指针数组中第二个元素的地址
	*array:    第一个元素存放的数据
	*(array+1):第二个元素存放的数据
	**(array+1):根据第二个元素存放的数据（是一个变量的地址）来解引用
	


			/*指针数组的定义。赋值，初始化*/

			#include <stdio.h>
			#include <stdlib.h>

			int main()
			{
				int a =10;
				
				//int * array[10] = {&a,&a}; 初始化
				int * array[10];
				//malloc循环赋值
				
				for(int n=0; n<10; n++)
					array[n] = (int *)malloc(sizeof(int));
				
				//*(array[n]) = 5;
				for(int n=0; n<10; n++)//循环给指针数组里面每个指针变量赋值数据域
					**(array+n) = n;//*(array[n]) = n;
					
				
				
				for(int n=0; n<10; n++)//循环打印指针数组中变量指向的数据域
					printf("array[%d] = %d\n",n,**(array+n));
				
				
				for(int n=0; n<10; n++)//循环释放内存空间
					free(*(array+n));
				
				
				
				
				return 0;
			}

	

			/*指针数组传参*/
			#include <stdio.h>

			int Fun(char * c_array[]);
			int Fun(char * c_array[])
			{
				for(int n=0; n<10; n++)
					printf("%c\n",*(c_array[n]));
				
				return 0;
			}

			int main()
			{
				char a = 'o';
				char * c_array[10] = {&a};
				
				Fun(c_array);

				return 0;
			}



			#include <stdio.h>
			#include <stdlib.h>
			/*使用普通的整形二级指针当作函数的形参，*/

			int A(int ** p_1,int ** p_2);
			int A(int ** p_1,int ** p_2)
			{
				//对应指针变量数据域互换
				
				
				int data;
				data  = **p_1;
				**p_1 = **p_2;
				**p_2 = data;
				
				return 0;
			}


			int main()
			{
				
				int ** p_1 = (int **)malloc(sizeof(int *));
				int ** p_2 = (int **)malloc(sizeof(int *));
				int a=1,b=2;
				int *p_3 = &a;
				int *p_4 = &b;
				p_1 = &p_3;
				p_2 = &p_4;
				
				printf("%d---%d\n",**p_1,**p_2);
				A(p_1,p_2);
				printf("%d---%d\n",**p_1,**p_2);
				
				
				return 0;
			}



		/*字符指针数组*/
		#include <stdio.h>
		#include <stdlib.h>
		#include <string.h>

		int main()
		{
			char * c_array[2];
			for(int n=0; n<2; n++)
				c_array[n] = (char *)malloc(5*sizeof(char));
			
			/*
			*(c_array[0]) = 'a';
			*(c_array[0]+1) = 'b';
			*(c_array[0]+2) = 'c';
			*/
			strcpy(c_array[0],"hello");
			strcpy(c_array[1],"world");

			printf("%s\n",c_array[0]);
			printf("%s\n",c_array[1]);
			
			/*
			for(int n=0; n<10; n++)
				**(c_array+n) = 'o';
				
			for(int n=0; n<10; n++)
				printf("%c\n",**(c_array+n));
			
			for(int n=0; n<10; n++)
				printf("%p----%p\n",c_array+n,*(c_array+n));
							   //元素本身的地址      元素存放的地址（你malloc出来的地址）
			
			*/
			for(int n=0; n<2; n++)
				free(*(c_array+n));

			return 0;
		}


补充：字符指针数组与二维数组类似


	练习：
	
		定义一个字符指针数组，然后用malloc给其赋值。
		把helloworld放在malloc申请的来的地址上面的数据域。
		
		#include <stdio.h>
		#include <stdlib.h>
		
		int main()
		{
			char * c_array[10];
			for(int n=0; n<10; n++)
				array[n] = (char *)malloc(sizeof(char));
				
			for(int n=0; n<10; n++)
				**(array+n) = 'o';
				
			for(int n=0; n<10; n++)
				printf("%c\n",**(array+n));
			
			for(int n=0; n<10; n++)
				free(*(array+n));
		
			return 0;
		}
	
	注意：
		打印指针数组里面每一个元素的地址，和他们各自存放的地址。
		
	例子：
			根据刚刚申请九宫格的malloc，自己申请一个5*2的结构，第一列存放hellio
			第二列存放world
			
	指针数组传参
	
			#include <stdio.h>
			
			int Fun(char * c_array[]);
			int Fun(char * c_array[])
			{
				printf("%ld\n",sizeof(c_array));
				
				return 0;
			}
			
			int main()
			{
				char * c_array[10];
				Fun(c_array);
			
				return 0;
			}
	


			/*指针数组传参*/
			#include <stdio.h>

			int Fun(char * c_array[]);
			int Fun(char * c_array[])
			{
				for(int n=0; n<10; n++)
					printf("%c\n",*(c_array[n]));
				
				return 0;
			}

			int main()
			{
				char a = 'o';
				char * c_array[10] = {&a};
				
				Fun(c_array);

				return 0;
			}


	void*的使用：
				人性化的考虑，给调用者使用任意一种任意类型
				
				void * A(void * data);
				void * A(void * data)
				{
					int * new_p = (int *)data;
					printf("%d\n",*new_p);
				
					return NULL;
				}
				
				int main()
				{
					int a = 11;
					
					A((void *)&a);
					
					return 0;
				}



			#include <stdio.h>

			void * A(void * data);
			void * A(void * data)
			{
				int * new_p = (int *)data;
				printf("%d\n",*new_p);
				//printf("%d\n",*((int *)data));
				//printf("%d\n",*data);

				return (void *)new_p;
			}

			int main()
			{
				int a = 11;
				
				
				int * p = (int *)A((void *)&a);
				printf("%d\n",*p);
				
				
				return 0;
			}


	
	普通的二级指针：
				int a=0;
				int *b = &a;  printf("%d\n",*b);
				int **c = &b; printf("%d\n",**c);




			#include <stdio.h>
			#include <stdlib.h>
			/*使用普通的整形二级指针当作函数的形参，*/

			int A(int ** p_1,int ** p_2);
			int A(int ** p_1,int ** p_2)
			{
				//对应指针变量数据域互换
				
				
				int data;
				data  = **p_1;
				**p_1 = **p_2;
				**p_2 = data;
				
				return 0;
			}


			int main()
			{
				
				int ** p_1 = (int **)malloc(sizeof(int *));
				int ** p_2 = (int **)malloc(sizeof(int *));
				int a=1,b=2;
				int *p_3 = &a;
				int *p_4 = &b;
				p_1 = &p_3;
				p_2 = &p_4;
				
				printf("%d---%d\n",**p_1,**p_2);
				A(p_1,p_2);
				printf("%d---%d\n",**p_1,**p_2);
				
				
				return 0;
			}
				



		/*函数指针变量的练习*/

		#include <stdio.h>
		char Get_Max(char c_a,char c_b);
		int Working(char (*f_p)(char,char));

		int Working(char (*f_p)(char,char))
		{
			
			char ret = (*f_p)('o','o');
			if(ret == '\0')
			{
				printf("两个字符相等！\n");
			}
			else
			{
				printf("大的字符数据是%c\n",ret);
			}
			
			return 0;
		}

		char Get_Max(char c_a,char c_b)
		{
			if(c_a == c_b)
			{
				return 0;
			}
			else
			{
				return (c_a>c_b?c_a:c_b);
			}
			
			
		}


		int main()
		{
			
			char (*f_p)(char,char) = &Get_Max;
			
			Working(f_p);
			
			return 0;
		}


		/*字符指针数组*/
		#include <stdio.h>
		#include <stdlib.h>
		#include <string.h>

		int main()
		{
			char * c_array[2];
			for(int n=0; n<2; n++)
				c_array[n] = (char *)malloc(5*sizeof(char));
			
			/*
			*(c_array[0]) = 'a';   // 字符指针数组与二维数组类似
			*(c_array[0]+1) = 'b';
			*(c_array[0]+2) = 'c';
			*/
			strcpy(c_array[0],"hello");
			strcpy(c_array[1],"world");

			printf("%s\n",c_array[0]);
			printf("%s\n",c_array[1]);
			
			/*
			for(int n=0; n<10; n++)
				**(c_array+n) = 'o';
				
			for(int n=0; n<10; n++)
				printf("%c\n",**(c_array+n));
			
			for(int n=0; n<10; n++)
				printf("%p----%p\n",c_array+n,*(c_array+n));
							   //元素本身的地址      元素存放的地址（你malloc出来的地址）
			
			*/
			for(int n=0; n<2; n++)
				free(*(c_array+n));

			return 0;
		}



			/*指针数组传参*/
			#include <stdio.h>

			int Fun(char * c_array[]);
			int Fun(char * c_array[])
			{
				for(int n=0; n<10; n++)
					printf("%c\n",*(c_array[n]));
				
				return 0;
			}

			int main()
			{
				char a = 'o';
				char * c_array[10] = {&a};
				
				Fun(c_array);

				return 0;
			}



			#include <stdio.h>

			void * A(void * data);
			void * A(void * data)
			{
				int * new_p = (int *)data;
				printf("%d\n",*new_p);
				//printf("%d\n",*((int *)data));
				//printf("%d\n",*data);

				return (void *)new_p;
			}

			int main()
			{
				int a = 11;
				
				
				int * p = (int *)A((void *)&a);
				printf("%d\n",*p);
				
				
				return 0;
			}


作业：
	根据最后一个练习，把malloc申请成一级指针的类型空间，来实现原有的程序功能
	
				

		1.根据最后一个练习，把malloc申请成一级指针的类型空间，来实现原有的程序功能。
		答：
		#include <stdio.h>
		#include <stdlib.h>

		int Fun(int **a_2,int **b_2);

		int Fun(int **a_2,int **b_2)
		{
			int data;
			data = **a_2;
			**a_2 = **b_2;
			**b_2 = data;
			
			return 0;
		}

		int main()
		{
			int a = 11,b = 12;
			
			int *a_1 = (int *)malloc(sizeof(int));
			int *b_1 = (int *)malloc(sizeof(int));

			a_1 = &a;
			b_1 = &b;

			int **a_2 = &a_1;
			int **b_2 = &b_1;

			printf("互换前：%d---%d\n",**a_2,**b_2);
			Fun(a_2,b_2);
			printf("互换后：%d---%d\n",**a_2,**b_2);
			
			return 0;	
		}



		1、根据最后一个练习，把malloc申请成一级指针的类型空间，来实现原有的程序功能
		答：
		#include <stdio.h>
		#include <stdlib.h>

		int swp_func(int **p_1, int **p_2);

		int swp_func(int **p_1, int **p_2)  //交换数据
		{
		    int tem_data;
		    tem_data  = **p_1;
		    **p_1 = **p_2;
		    **p_2 = tem_data;

		    return 0;
		}

		int main()
		{
		    int a = 1, b = 2;
		    int *temp_p1 = (int *)malloc(sizeof(int));  //一级指针创建4个字节空间
		    int *temp_p2 = (int *)malloc(sizeof(int));
		    int **p_1, **p_2;
		    int *protect_p1, *protect_p2;

		    protect_p1 = temp_p1;  //保护指针变量首地址
		    protect_p2 = temp_p2;

		    temp_p1 = &a;    //一级指针到二级指针的赋值
		    temp_p2 = &b;
		    p_1 = &temp_p1;
		    p_2 = &temp_p2;

		    printf("交换之前：%d---%d\n",**p_1,**p_2);

		    swp_func(p_1, p_2);  //交换数据

		    printf("交换之后：%d---%d\n",**p_1,**p_2);

		    free(protect_p1);  //释放空间
		    free(protect_p2);

		    return 0;
		}



		根据最后一个练习，把malloc申请成一级指针的类型空间，来实现原有的程序功能
		#include "stdio.h"
		#include "stdlib.h"

		int fun_pointer(int **second_pointer_parameter_1,int **second_pointer_parameter_2); //函数的声明

		int fun_pointer(int **second_pointer_parameter_1,int **second_pointer_parameter_2)
		{
			int num;
			num = **second_pointer_parameter_1;
			**second_pointer_parameter_1 = **second_pointer_parameter_2;
			**second_pointer_parameter_2 = num;

			//printf("%d-------%d\n", **second_pointer_parameter_1,**second_pointer_parameter_2);

			return 0;
		}

		int main(int argc, char const *argv[])
		{
			int *pointer_1 = (int *)malloc(sizeof(int));
			int *pointer_2 = (int *)malloc(sizeof(int));

			int *pointer_3 = pointer_1;  // 备份首地址
			int *pointer_4 = pointer_2;

			int a=12,b=13;
			pointer_1 = &a; // pointer_1 首地址已经改变
			pointer_2 = &b;

			int **second_pointer_1 = &pointer_1;
			int **second_pointer_2 = &pointer_2;

			printf("未交换之前的数据:%d%5d\n", **second_pointer_1,**second_pointer_2);

			fun_pointer(second_pointer_1,second_pointer_2);
			//fun_pointer(&pointer_1,&pointer_2);

			printf("交换之后的数据:%d%5d\n", **second_pointer_1,**second_pointer_2);

			free(pointer_3); 
			free(pointer_4);

			return 0;
		}


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
