---
title: C_Language_Basics_09
date: 2020-07-29 09:21:18
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

桃李春风一杯酒，江湖夜雨十年灯。
想得读书头已白，隔溪猿哭瘴溪藤。

这是一个链接 [菜鸟教程][Runoob]

[Runoob]: http://www.runoob.com/

# C语言基础
一、结构体类型

	1.1 特点：
	
		和数组一样，可以存放多个成员。
	
		结构体变量里面的成员的类型可以不一致。
		
	1.2 作用：
			经常用他来封装分类数据。
	
	1.3
	
		结构体变量的定义：int a;
		
		结构体类型
		
			结构体1             结构体2
					int                 char arrya[10];
					long                short b;
	
			
		理解一下一般类型的使用：
						直接定义变量   int a;
						再给变量赋值   a = 11;
		结构体：
				先定义结构体的类型   
					struct A { int a; long b;};
					
					struct B
					{
						char arrya[10];
						short b;
					};
				
				
				在根据结构体类型对应变量
				
					struct A 
					{ 
						int a;
						long b;
					}变量名，.....; 
					
					struct  
					{ 
						int a;
						long b;
					}变量名，.....;  //在定义类型的时候同时定义固定的变量，后面不能在定义结构体变量
					
					struct A st;
					
					
				在给各个变量赋值,要先用.来引用结构体里面的成员才能赋值
				
						st.a = 11; //st的a
						st.b = 100;
						
						
				初始化结构体变量：
				
					struct A 
					{ 
						int a;
						long b;
						char c[12];
						char d;
					}B,C,D,E....; 
					
					
					struct A XXX = {
										1,
										2,
										"dfsadfs",
										's'
									};
									
					struct A YYY = {
						.a = 1,
						.b = 2
	
					};
					printf("%d---%ld\n",YYY.a,YYY.b);
						
				
		结构体标识符：struct
	


		#include <stdio.h>
		#include <string.h>


		int main()
		{
			//定义结构体类型
			struct 
			{
				
				int a;
				char b[11];
				short c;
				
			}B,C,D;
			
			
			//struct A XXX;//先定义结构体变量
			
			//根据.来引用成员并且赋值
			B.a = 10;
			char c_data[] = "helloworld";
			strcpy(B.b,c_data);
			B.c = 11;
			
			printf("%d---%s---%hd\n",B.a,B.b,B.c);
			
			
			return 0;
		}



		#include <stdio.h>

		struct A 
		{ 
			int a;
			long b;
		}B = {1,2}; 

		int main()
		{
			printf("%d---%ld\n",B.a,B.b);
			
								
			struct A XXX = {
								1,
								2
			
							};
			printf("%d---%ld\n",XXX.a,XXX.b);
			
			struct A YYY = {
								.a = 1,
								.b = 2
			
							};
			printf("%d---%ld\n",YYY.a,YYY.b);
							
			
			return 0;
		}



		/*结构体变量当函数的参数*/

		#include <stdio.h>
		#include <string.h>

		struct ST_1
		{
			char         name[13];
			unsigned int age;
		};

		struct ST_1 Fun(struct ST_1 xxx);
		struct ST_1 Fun(struct ST_1 xxx)
		{	
			//这里有一个结构体变量
			
			printf("结构体XXX的地址:%p\n成员name的地址：%p\n成员age的地址:%p\n",
					&xxx,&(xxx.name),&(xxx.age));
					
			return xxx;
		}

		int main()
		{

			struct ST_1 p_1,p_2; //p_1是实参
			
			char * p = "小明";
			strcpy(p_1.name,p);
			//p_1.name = "小明"; 错误
			p_1.age = 8;
			
			
			//printf("name:%s---age:%d\n",p_2.name,p_2.age);
			
			p_2 = Fun(p_1);
			

			return 0;
		}


		#include <stdio.h>

		typedef struct student_inf
		{
			int a;
			
		}s_i;


		int main()
		{
			struct student_inf xm;
			xm.a = 11;
			
			s_i xh;
			xh.a = 12;
			
			printf("%d---%d\n",xm.a,xh.a);
			
			return 0;
		}


	1.4 结构体变量当参数、返回值
	
		int Fun();
		int Fun(struct ST_1 xxx)
		{	
			printf("name:%s---age:%d\n",xxx.name,xxx.age);
			
			return 0;
		}
		
		int main()
		{
			struct ST_1
			{
				char         name[13];
				unsigned int age;
			};
		
			struct ST_1 p_1; //p_1是实参
			char * p = "小明";
			strcpy(p_1.name,p);
			p_1.age = 8;
			
			Fun(p_1);
			
		
			return 0;
		}
		
	补充：结构体变量和int变量一样，可以直接赋值数据 结构体1 = 结构体2；
	
	练习：
	
			1：定义一个结构体类型
			2：在main里面定义结构体的变量，并且赋值
			3：封装一个函数接受结构体变量，
			打印结构体的地址，以及各个成员的地址。总结有什么特点
	

		/*结构体变量当函数的参数*/

		#include <stdio.h>
		#include <string.h>

		struct ST_1
		{
			char         name[13];
			unsigned int age;
		};

		struct ST_1 Fun(struct ST_1 xxx);
		struct ST_1 Fun(struct ST_1 xxx)
		{	
			//这里有一个结构体变量
			
			printf("结构体XXX的地址:%p\n成员name的地址：%p\n成员age的地址:%p\n",
					&xxx,&(xxx.name),&(xxx.age));
					
			return xxx;
		}

		int main()
		{

			struct ST_1 p_1,p_2; //p_1是实参
			
			char * p = "小明";
			strcpy(p_1.name,p);
			//p_1.name = "小明"; 错误
			p_1.age = 8;
			
			
			//printf("name:%s---age:%d\n",p_2.name,p_2.age);
			
			p_2 = Fun(p_1);
			

			return 0;
		}


	1.5 结构体指针变量
	
			struct A 
			{ 
				int a;
				long b;
			};
			
			struct A  B;
									
		结构体指针变量的定义： struct A * st_p = &B;
		
		(*st_p).a = 10;
		B.a = 10;
		
		printf("%d\n",(*st_p).a);
	

		#include <stdio.h>
		#include <stdlib.h>
		#include <errno.h>

		struct A 
		{ 
			int a;
			long b;
		};


		int main()
		{
			struct A data_array[2] = {{101,109},{111,112}};//先定义一个结构体数组
			struct A (*st_p)[2] = &data_array;//结构体数组指针
			/*打印每个结构体里面的数据*/
			for(int n=0; n<2; n++)
				//printf("a:%d----b:%ld ",((*st_p)[n]).a,((*st_p)[n]).b);
					printf("a:%d----b:%ld ",(*st_p)->a,(*st_p)->b);
						
			printf("\n");
			
			return 0;
		}


	
	1.6 计算结构体大小
	
		struct A 
			{ 
				int a;
				long b;
			};
		int a;
		printf("%ld\n",sizeof(struct A));
		
		
	1.7 typedef:给数据类型取新的别名
	
	跟普通类型：	typedef int i_t;
	跟结构体类型：
					typedef struct studnt_inf
					{
						int a;
					
					}s_i;
					
					struct studnt_inf  xm;
					s_i xh;
					把struct studnt_inf取成新的名字位s_i



		#include <stdio.h>

		typedef struct student_inf
		{
			int a;
			
		}s_i;


		int main()
		{
			struct student_inf xm;
			xm.a = 11;
			
			s_i xh;
			xh.a = 12;
			
			printf("%d---%d\n",xm.a,xh.a);
			
			return 0;
		}
					
	1.8 结构体与malloc
	
		1）给结构体指针变量申请空间
		
		typedef struct studnt_inf
		{
			int a;
					
		}s_i,*p_s_i;
		struct studnt_inf   == s_i
		struct studnt_inf * == p_s_i
		
		struct studnt_inf * p_s = (struct studnt_inf *)malloc(sizeof(struct studnt_inf));
		s_i * p_s = (s_i *)malloc(sizeof(s_i));
		p_s_i p_s = (p_s_i)malloc(sizeof(s_i));
		
		
		
	1.9 结构体数组
				
		
	练习：
		
		1:
		使用typedef定义顺便给结构体类型取新别名，
		给结构体指针变量申请空间，
		把结构体指针变量当初参数传给一个函数，
		在函数里面打印结构体的成员数据。
		
	
		1:结构体数组
		struct A 
		{ 
			int a;
			long b;
		}p_s[3] = {{1,2},{3,4},{5,6}};
		
		定义结构体数组变量：
		int       array[10];
		struct A  p_s[10];
		
		结构体数组变量赋值：
		array[0] = 11;
		*array   = 11;
 		
		ps[0].a = 11;
		
		(*ps).a = 11;
		ps->a = 11;
		
		
		初始化：
				struct A 
				{ 
				int a;
				long b;
				}p_s[3] = {{1,2},{3,4},{5,6}};
				
				struct A  p_s[10] = {{.a=1,.b=2},
									 {.a=3,.b=4},
									 {.a=5,.b=6}
									 };
		



		#include <stdio.h>

		struct A 
		{ 
			int a;
			long b;
		}/*p_s[3] = {{1,2},{3,4},{5,6}}*/;

		int main()
		{

				
				//定义结构体数组变量：
				//int       array[10];
				struct A  p_s[10] = {{.a=1,.b=2},{.a=3,.b=4},{.a=5,.b=6}};
				
				//结构体数组变量赋值：
				//array[0] = 11;
				//*array   = 11;
		 		
				/*
				p_s[0].a = 11;
				(*p_s).a = 12;
				p_s->a = 13;
				*/
				
				printf("%d\n",p_s->a);
			
			
			return 0;
		}



		#include <stdio.h>

		int main()
		{
			struct A 
			{ 
				int  a;
				long b;
			};
			struct A B;
			printf("%ld --- %ld\n",sizeof(struct A),sizeof(B));
			printf("%ld \n",sizeof(long));

			return 0;
		}


	2:结构体数组指针
	
		定义+赋值：
			
				struct A (*pe)[10] = NULL;
				struct A  p_s[10];
				pe = &p_s;
				
			
			初始化：
			
				struct A  p_s[10];
				struct A (*pe)[10] = &p_s;
				
				
				int (*array_p)[10] = (int (*)[])malloc(10*sizeof(int));
				struct A (*pe)[10] = (struct A (*)[])malloc(10*sizeof(struct A));
				if(pe == NULL)
				{
					perror("malloc");
					return -1;
				}
	
	

		#include <stdio.h>
		#include <stdlib.h>
		#include <errno.h>

		struct A 
		{ 
			int a;
			long b;
		};


		int main()
		{
			struct A data_array[2] = {{101,109},{111,112}};//先定义一个结构体数组
			struct A (*st_p)[2] = &data_array;//结构体数组指针
			/*打印每个结构体里面的数据*/
			for(int n=0; n<2; n++)
				//printf("a:%d----b:%ld ",((*st_p)[n]).a,((*st_p)[n]).b);
					printf("a:%d----b:%ld ",(*st_p)->a,(*st_p)->b);
						
			printf("\n");
			
			return 0;
		}



		#include <stdio.h>
		#include <string.h>

		struct A 
		{ 
			int a;
			long b;
			char buf[10];
		};


		int main()
		{
			struct A  B;
			//结构体指针变量的定义： 
			struct A * st_p = &B;
			(*st_p).a = 10;
			st_p->b = 11;
			strcpy((*st_p).buf,"abc");
			//B.a = 10;
				
			printf("%d --- %s --- %ld\n",(*st_p).a,(*st_p).buf,st_p->b);
			printf("%d\n",(&B)->a);
			
			return 0;

		}



		#include <stdio.h>
		#include <errno.h>
		#include <stdlib.h>
		#include <string.h>

		typedef struct student_inf
		{
			
			char         name[10] ;
			unsigned int age;
			
		}s_i,*p_si;

		p_si Fun(p_si xm);
		p_si Fun(p_si xm)
		{
			printf("name:%s----age:%d\n",xm->name,xm->age);
			printf("请输入要更改的姓名：");
			scanf("%s",xm->name);
			
			return xm;
		}


		int main()
		{
			p_si xm = (p_si)malloc(sizeof(s_i));
			if(xm == NULL)
			{
				perror("malloc");
				return -1;
			}
			
			strcpy(xm->name,"xm");
			xm->age= 11;
			
			xm = Fun(xm);
			printf("name:%s----age:%d\n",xm->name,xm->age);
			free(xm);
			
			return 0;
		}


	3:结构体指针数组
		他是一个数组，存放多个结构体的地址
			
		struct A 
		{ 
			int a;
			long b;
		};
			
			
		定义：struct 类型名 *变量名[2];
			  struct A * array[2];
		
		
		赋值：
		定义一个结构体数组存放两个结构体，然后把这两个结构体的地址存放在上面的结构体指针数组里面
		struct A 
		{ 
			int a;
			long b;
		}a[2] = {{11,12},{13,14}};
		
		array[0] = &(a[0]);
		array[1] = &(a[1]);
			
		printf("%d\n",(*(array[0])).a);
		printf("%d\n",(array[0])->a);
	

	
		#include <stdio.h>


		int main()
		{
			struct A 
			{ 
				int a;
				long b;
			}a[2] = {{11,12},{13,14}};
			
			struct A * array[2];  //定义一个结构体指针数组
			array[0] = &(a[0]); //分别把结构体数组的结构体地址赋值给结构体指针数组
			array[1] = &(a[1]);
				
			printf("%d\n",(*(array[0])).a);
			printf("%d\n",(array[0])->a);
			
			
			return 0;
		}


	CPU字长:
		
		32位：4个字节
		64位：8个字节
	
		普通变量的存放：
				字节对齐：
	
	练习：
		写一份最少代码的C语言程序。
		
		test.c
		
		void main()
		{
		
		}
				
	

		#include <stdio.h>
		#include <errno.h>
		#include <stdlib.h>
		#include <string.h>

		typedef struct student_inf
		{
			
			char         name[10] ;
			unsigned int age;
			
		}s_i,*p_si;

		p_si Fun(p_si xm);
		p_si Fun(p_si xm)
		{
			printf("name:%s----age:%d\n",xm->name,xm->age);
			printf("请输入要更改的姓名：");
			scanf("%s",xm->name);
			
			return xm;
		}


		int main()
		{
			p_si xm = (p_si)malloc(sizeof(s_i));
			if(xm == NULL)
			{
				perror("malloc");
				return -1;
			}
			
			strcpy(xm->name,"xm");
			xm->age= 11;
			
			xm = Fun(xm);
			printf("name:%s----age:%d\n",xm->name,xm->age);
			free(xm);
			
			return 0;
		}

		#include <stdio.h>

		typedef struct student_inf
		{
			int a;
			
		}s_i;


		int main()
		{
			struct student_inf xm;
			xm.a = 11;
			
			s_i xh;
			xh.a = 12;
			
			printf("%d---%d\n",xm.a,xh.a);
			
			return 0;
		}



		#include <stdio.h>
		#include <string.h>

		struct A 
		{ 
			int a;
			long b;
			char buf[10];
		};


		int main()
		{
			struct A  B;
			//结构体指针变量的定义： 
			struct A * st_p = &B;
			(*st_p).a = 10;
			st_p->b = 11;
			strcpy((*st_p).buf,"abc");
			//B.a = 10;
				
			printf("%d --- %s --- %ld\n",(*st_p).a,(*st_p).buf,st_p->b);
			printf("%d\n",(&B)->a);
			
			return 0;

		}


		#include <stdio.h>

		struct A 
		{ 
			int a;
			long b;
		}/*p_s[3] = {{1,2},{3,4},{5,6}}*/;

		int main()
		{

				
				//定义结构体数组变量：
				//int       array[10];
				struct A  p_s[10] = {{.a=1,.b=2},{.a=3,.b=4},{.a=5,.b=6}};
				
				//结构体数组变量赋值：
				//array[0] = 11;
				//*array   = 11;
		 		
				/*
				p_s[0].a = 11;
				(*p_s).a = 12;
				p_s->a = 13;
				*/
				
				printf("%d\n",p_s->a);
			
			
			return 0;
		}



		#include <stdio.h>


		int main()
		{
			struct A 
			{ 
				int a;
				long b;
			}a[2] = {{11,12},{13,14}};
			
			struct A * array[2];  //定义一个结构体指针数组
			array[0] = &(a[0]); //分别把结构体数组的结构体地址赋值给结构体指针数组
			array[1] = &(a[1]);
				
			printf("%d\n",(*(array[0])).a);
			printf("%d\n",(array[0])->a);
			
			
			return 0;
		}




		#include <stdio.h>
		#include <stdlib.h>
		#include <errno.h>

		struct A 
		{ 
			int a;
			long b;
		};


		int main()
		{
			struct A data_array[2] = {{101,109},{111,112}};//先定义一个结构体数组
			struct A (*st_p)[2] = &data_array;//结构体数组指针
			/*打印每个结构体里面的数据*/
			for(int n=0; n<2; n++)
				//printf("a:%d----b:%ld ",((*st_p)[n]).a,((*st_p)[n]).b);
					printf("a:%d----b:%ld ",(*st_p)->a,(*st_p)->b);
						
			printf("\n");
			
			return 0;
		}



	作业：
		设置m值的方法
		设置结构体成员的位域
		实现结构体数组指针的传参和返回值---(必作，这个指针变量用malloc)


		1、设置m值的方法
		答：通过定义结构体成员的最大的数据类型长度来改变m值，超过cpu字长则按cpu字长计算，
		所以设置m值方法是改变内存变量的存储地址和变量数据类型长度内取一定长度位数。

		2、设置结构体成员的位域
		答： 位结构定义的一般形式为:

		　　　　struct 位结构名{
		　　　　　　　　　　　　 数据类型 变量名: 位数（整型常数）;
		　　　　　　　　　　　　 数据类型 变量名: 位数（整型常数）;
		　　　　　　　　　　 } 位结构变量;　  //位数大小不能超过所定义类型包含的总bit数
		　   可以设置
		        数据类型：0          //从从下一单元起存放该位域
		        数据类型：整数常量   //无位域名，只用来作填充或调整位置


		3、实现结构体数组指针的传参和返回值---(必作，这个指针变量用malloc)
		答：
		#include <stdio.h>
		#include <stdlib.h>
		#include <errno.h>

		typedef struct A
		{
		    int a;
		    int b;
		} stA;

		stA *func(stA (*arr_p)[2]);

		stA *func(stA (*arr_p)[2])
		{
		    for(int i = 0; i < 2; i++)
		    {
		        ((*arr_p)[i]).a += 1 ;
		        ((*arr_p)[i]).b += 1;
		    }

		    return 0;
		}

		int main()
		{
		    stA array[2] = {{1,3},{5,7}};
		    stA (*arr_p)[2] = (stA (*)[])malloc(2*sizeof(stA));
		    stA *tem_p = arr_p;              //开创空间地址保存变量

		    if(arr_p == NULL)     //
		    {
		        perror("malloc");
		        return -1;
		    }

		    arr_p = &array;                  //取数组地址
		    func(arr_p);

		    for(int i = 0; i < 2; i++)
		    {
		        printf("%d---%d\n", ((*arr_p)[i]).a, ((*arr_p)[i]).b);
		    }

		    free(tem_p);

		    return 0;
		}



		1.设置m值的方法
		答：
		typedef struct L1{	
		char type;	
		string name;	
		L1 *next1;
		}sdt;
		sdt s; s.name="K1";
		或者bai#include<stdlib.h>//在开头处包括头文件
		sdt *s1 = (sdt*)malloc(sizeof(sdt));
		s1->name = "K1";

		2.设置结构体成员的位域
		答：
		位域的定义和位域变量的说明位域定义与结构定义相仿，其形式为： 
		struct 位域结构名 
		{

		 位域列表

		};

		其中位域列表的形式为： 类型说明符 位域名：位域长度

		位域变量的说明与结构变量说明的方式相同。 可采用先定义后说明，同时定义说明或者直接说明这三种方式。例如：
		struct bs
		{
		　　int a:8;
		　　int b:2;
		　　int c:6;
		}data; 

		说明data为bs变量，共占两个字节。其中位域a占8位，位域b占2位，位域c占6位。

		3.实现结构体数组指针的传参和返回值---(必作，这个指针变量用malloc)
		答：
		#include <stdio.h>

		#include <stdlib.h>

		#include <errno.h>



		struct A

		{

		  int a;

		  long b;

		}a[2] = {{1,2},{3,4}};



		struct A * Fun (struct A (*p_s)[2]);//p_s是形参



		struct A * Fun (struct A (*p_s)[2])

		{

		  for(int n=0;n<2;n++)

		    printf("%d---%ld\n",((*p_s)[n]).a,((*p_s)[n]).b);



		  return *p_s;

		}



		int main()

		{

		  struct A (*pe)[2] = (struct A (*)[])malloc(2*sizeof(struct A));

		  if(pe == NULL)

		  {

		    perror("malloc");

		    return -1;

		  }

		  struct A *p = *pe;//备份pe的地址

		  pe = &a;

		  struct A *p_ret = Fun(pe);

		  for(int n=0;n<2;n++)

		    printf("%d---%ld\n",((p_ret)+n)->a,((p_ret)+n)->b);

		  free(p);



		  return 0;

		}	


		1、设置m值的方法
		答：首先确定编译器的默认对齐方式或者#pragma pack（n）中n的值，然后遵守以下两条法则计算结构体分配的内存大小：结构体中的变量item在结构体中相对于首地址的偏移量应该是 X的倍数，X 由如下式子确定：X=min(n,sizeof(item))，结构体的大小必须为其最大的那个X的整数倍。

		   
		2、设置结构体成员的位域
		答：第一个数据成员放在offset为0的地方，以后每个数据成员存储的起始位置要从该成员大小的整数倍开始（比如int在32位机为4字节，则要从4的整数倍地址开始存储）。结构体作为成员：如果一个结构里有某些结构体成员，则结构体成员要从其内部最大元素大小的整数倍地址开始存储。（struct a里存有struct b，b里有char，int，double等元素，那b应该从8的整数倍开始存储。


		3、实现结构体数组指针的传参和返回值（必做，这个指针变量用malloc）
		答：代码如下：
		    //实现结构体数组指针的传参和返回值（这个指针变量用malloc）

		#include <stdio.h>
		#include <stdlib.h>
		#include <errno.h>

		struct X
		{
		      int a;
		      long b;
		}buf[2] = {{11,12},{13,14}};

		struct X *fun_1(struct X (*p_2)[2]);

		struct X *fun_1(struct X (*p_2)[2])
		{
		       for(int n=0; n<2; n++)
		        printf("a is：%d\nb is：%ld\n",((*p_2)[n]).a,((*p_2)[n]).b);
		        return *p_2;
		}

		int main()
		{
		      struct X (*p_1)[2] = (struct X (*)[])malloc(2*sizeof(struct X));
		      if(p_1 == NULL)//判断申请空间是否失败
		      {
		             perror("malloc");
		             return -1;
		      }
		      p_1 = &buf;
		      fun_1(p_1);
		      return 0;
		}



			设置m值的方法
			设置结构体成员的位域
			实现结构体数组指针的传参和返回值---(必作，这个指针变量用malloc)
		①m的值的是根据结构体变量的类型来确定的，因此我们可以通过定义结构体成员变量来设置m的值，如果超过cpu字长则按cpu字长计算。其结构体大小结果要为成员中最大字节的整数倍，m为其中的最大字节数。
		②位域的定义和位域变量的说明位域定义与结构定义相仿，其形式为： 

		struct 位域结构名 
		{

		 位域列表

		};
		其中位域列表的形式为：

		类型说明符 位域名：位域长度
		位域变量的说明与结构变量说明的方式相同。
		可采用先定义后说明，同时定义说明或者直接说明这三种方式。
		例如：
		struct bit_field
		{
		　　int a:8;
		　　int b:2;
		　　int c:6;
		}data; 
		说明data为field变量，共占两个字节。其中位域a占8位，位域b占2位，位域c占6位。
		③
		#include "stdio.h"
		#include "stdlib.h"
		#include "errno.h"
		#define M 2

		typedef struct student
		{
			char name[20];
			int age;
			float sorce;
		}s_d;

		s_d *fun_array_pointer(s_d (*str_arr_pointer_par)[M]); //函数的声明

		s_d *fun_array_pointer(s_d (*str_arr_pointer_par)[M])
		{
			for(int n=0; n < M; n++)
			{
				printf("姓名:%s\n年龄:%d\n成绩:%.2f\n", ((*str_arr_pointer_par)[n]).name,((*str_arr_pointer_par)[n]).age,((*str_arr_pointer_par)[n]).sorce);
			}

			printf("请输入要修改的姓名:");

			for(int n=0; n < M; n++)
			{
				scanf("%s",(*str_arr_pointer_par+n)->name);
			}
			
			return *str_arr_pointer_par;

			
		}

		int main(int argc, char const *argv[])
		{
			struct student str_array[M] = {{"灰原哀",21,86.5},{"柯南",19,89.6}}; //定义一个结构体数组

			struct student *str_arr_pointer_ret;

			s_d (*str_arr_pointer)[M] = (s_d (*)[])malloc(M*sizeof(s_d));

			struct student (*pointer)[] = str_arr_pointer;

			if(str_arr_pointer == NULL)
			{
				perror("malloc");
				return -1;
			}

			str_arr_pointer = &str_array;

			str_arr_pointer_ret = fun_array_pointer(str_arr_pointer);

			for(int n=0; n< M; n++)
			{
				printf("姓名:%s\n年龄:%d\n成绩:%.2f\n", str_arr_pointer_ret[n].name,str_arr_pointer_ret[n].age,str_arr_pointer_ret[n].sorce);
			}

			free(pointer);

			return 0;
		}

### 课堂笔记
		联合体(共用体)
		共用体里面的成员公用一块内存空间，可以减少内存消耗，但是访问数据的效率低
		共用体会造成数据间差
		C89 C99 C11 ANSI  C语言标准
		枚举值是常量不能修改
		枚举类型可以不需要定义变量 直接使用枚举值
		枚举类型大小为4个字节
		常变量 程序退出时才释放
		常变量 是只读的  变成静态的数据  不能再去修改
		使用const修改常变量 一定要初始化赋值
		const int * a 修饰* *a指向的数据不能改变
		内联函数 栈比较大

		生成原生的汇编文件  
		验证是否内联函数进行了在编译的过程加入代码段
		gcc -E
		gcc -S
		这个步骤不能看出
		一般情况下，用inline修饰函数时，只是建议编译器采用内联的操作
		内存管理  内存分布
		为什么32位系统最大只能用4G内存
		一个地址一个字节
		寻址空间
		32位系统有多少个寻址空间(多少个地址) 2的32次方个地址 2的32次方个字节 也就是4个G
		静态数据指的是所有的全局变量，以及static型局部变量
		数据段中 .bss段 专门用来存放未初始化的静态数据 .data段 专门存放已经初始化的静态数据 .rodata段 用来存放只读数据 即常量 比如进程中所有的字符串 字符常量 等
		代码段中 .text段 用来存放用户程序代码(即用户代码) 也就是包括main函数在内的所有用户自定义函数 .init段 用来存储系统给每一个可执行程序自动添加的初始化代码 这部分代码功能包括环境变量的准备、命令行参数的组织和传递等 并且这部分数据被放在了栈底
		.init段 系统初始化代码



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