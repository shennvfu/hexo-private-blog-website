---
title: C_Language_Basics_10
date: 2020-07-30 11:11:18
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

	人生到处知何似，应似飞鸿踏雪泥。
	泥上偶然留指爪，鸿飞那复计东西。

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
一、联合体（共用体）
	
	特点：共用体里面的所有成员都是共用一块内存空间。
			减少内存消耗，但是访问数据的效率低。
	
	1.1定义类型
	
		union un_1
		{
			int  a;
			char b;
			long c;
		}变量名;
		
	1.2 定义变量
	
		union 类型名 变量名
		union un_1   A; 
		
	1.3 赋值
		A.a = 1;
		A.b = 's';
		A.c = 10000;
		
		printf("%d---%c---%ld\n",A.a,A.b,A.c);
		
	1.4 初始化
		union un_1   A = {1,'s',1000}；//第一次赋值是联合体变量的值
		union un_1   A = {
							.a = 1,
							.b = 's',
							.c = 1000}；//最后一次赋值是联合体变量的值
							
	练习：
			自己尝试着联合体的传参，联合体指针的运用。
			
			
二、枚举（整数常量集合）

	定义一个枚举类型：
				enum en_1
				{
				  mon,  
				  Thu,  
				  Fed,  
				  Stu,  
				  Sat,  
				  Sun  
				};
		
	定义变量：
			enum en_1 A = mon;
			
			
			printf("%d\n",mon);
			
	补充：
			1：枚举值是常量，不能修改
			2：枚举类型可以不需要定义变量，直接使用枚举值
			3：枚举类型大小4个字节



		#include <stdio.h>

		int main()
		{
			enum en_1
			{
			  mon,   
			  Thu,  
			  Fed,  
			  Stu,   
			  Sat,  
			  Sun   
			};
			enum en_1 A = mon;
					
			printf("%d\n",mon);
			
			
			
			return 0;
		}

			
三、const
	3.1
		用来修饰变量，被修饰的变量成为常变量。
	
		const int a= 10;
			a = 11;
			a = 12;
	
		修饰字符串变量表示的路径
	
	3.2 const与指针变量
		int b = 11;
		const int * a = &b;  //int const * a
		//修饰* ,*a指向的数据不能通过*a来改变
		b = 12;
		*a = 12;
		
		 int * const a = &b;
		 //修饰a,a里面的数据不能通过a来改变
		 int c;
		 a = &c;
		
		int * const a
		
	练习：
		


		#include <stdio.h>

		int main()
		{
			const int a;  //
			
			printf("%d\n",a);
			
			//a = 11;
			
			//printf("%d\n",a);
			
			return 0;
		}



		#include <stdio.h>

		int main()
		{
			int b = 11;
			/*
			int * a = &b;  //int const * a
			//修饰* ,*a指向的数据不能改变
			//b = 12;
			*a = 12;
			printf("%d---%d\n",b,*a);
			*/
			
			int * const a = &b;
			int c;
			a = &c;
			
			return 0;
		}



		#include <stdio.h>

		int main()
		{
			
			int a = 10;//举个例子：a为10 是正常情况，不是10时，需要提醒
			
			a = 8;
			if(a != 10)
			{
				#ifdef D_BUG
					printf("不正常！\n");
				#endif
			}
				
			return 0;
		}


四、宏定义
	
	用法：
		1：普通宏定义
		
		2：条件编译 （调试的）
		
		3: 宏定义函数
		
			单行定义：
					#define M  10
					#define MAX(a,b)  (a>b?a:b);
					#define P(a,b)    (a)*(b);
					
					
					
					int main()
					{
						++MAX(11,12)
						++a>b?a:b;
						
						P(1+2,12)
						(1+2)*(12)
						return 0;
					}
					
			多行定义：换行   \

			#define SWAP(a,b)   {
			\       
				a = a+b;\
				b = a-b;\
				a = a-b;\
			}
						
				int a = 11;
				int b = 12;
				a = a+b;
				b = a-b;
				a = a-b;
			注意：
				宏定义只帮你进行字符的替换
				注意宏定义最后面有没有;
				注意括号
			


		#include <stdio.h>

		int main()
		{
			
			int a = 10;//举个例子：a为10 是正常情况，不是10时，需要提醒
			
			a = 8;
			if(a != 10)
			{
				#ifdef D_BUG
					printf("不正常！\n");
				#endif
			}
				
			return 0;
		}



		#include <stdio.h>

		#define SWAP(a,b) \
		{\
		       a = a+b;\
			   b = a-b;\
			   a = a-b;\
		}

		int main()
		{
			int a = 11;
			int b = 12;

			printf("a:%d----b:%d\n",a,b);

			SWAP(a,b)

			printf("a:%d----b:%d\n",a,b);
			
			return 0;	
		}


			
五、Linux下GDB调试工具
		
	1.1
		第一步：
				编译时：gcc xxx.c -o xxx -g
		第二步：
				gdb xxx
				
		第三步：star
		第四步：n 一行一行的执行
		
		在执行的过程中：输入 p 变量名 或者p &变量名
		
		q:退出工具
		
	1.2 让gdb生成存放堆栈内存调试信息的core文件
	
		1）
		查看默认的core文件大小（KB）
		ulimit -c
		如果是0KB，需要设置core文件的大小才能生存对应的core文件
		ulimit -c 4
		ulimit -c unlimited  (无限制)
		
		2）生成core文件
			gcc xxx.c -o xxx -g
		
			注意：运行段错误的程序，才会生成core
			./xxx
			      
		3）使用gdb解析core文件
		
			gdb -c core 二进制可执行程序
			gdb -c core xxx
			
六：
	递归函数：函数自己调用自己
		
		采用递归函数的场景：
						目录检索

		递归的两个必要条件
		1、存在限制条件，当满足这个条件时，递归便不再继续。
		2、每次递归调用之后越来越接近这个限制条件。
	递归函数算阶乘：
			 
			 5！ = 5X4X3X2X1;



补充：

		递归：函数调用自身的编程技巧
		人理解迭代，神理解递归。毋庸置疑地，递归确实是一个奇妙的思维方式。
		递归的两个必要条件
		1、存在限制条件，当满足这个条件时，递归便不再继续。
		2、每次递归调用之后越来越接近这个限制条件。

		1.递归和非递归分别实现求第n个斐波那契数。

		斐波纳契数列fibonacci,又称黄金分割数列，指的是这样一个数列：1、1、2、3、5、8、13、21、……
		在数学上，斐波纳契数列以如下被以递归的方法定义：
		//F0=0，F1=1，Fn=F(n-1)+F(n-2)（n>=2，n∈N*）。

		递归实现斐波那契数C程序

		#define _CRT_SECURE_NO_WARNINGS
		#include <stdio.h>
		#include <stdlib.h>
		int fibonacci(int n)
		{
			if(n <= 2)
			{
				return 1;
			}
			else
			{
			    return fibonacci(n - 1) + fibonacci(n - 2);
		    }
		}
		int main()
		{
			int n;
			printf("请输入你想输出第几项的斐波那契数：\n");
			scanf("%d", &n);
			printf("%d\n", fibonacci(n));
			system("pause");
			return 0;
		}



不用递归实现斐波那契数C程序

		#define _CRT_SECURE_NO_WARNINGS
		#include <stdio.h>
		#include <stdlib.h>

		int fibonacci(int n)
		{
			int result = 0;
			int a = 1;
			int b = 1;
			int i;
			if(n <= 2)
			{
				return 1;
			}
			for(i = 3; i <= n; i++)
			{
				result = a + b;
				a = b;
				b = result;
			}
			return result;
		}
		int main()
		{
			int n;
			printf("请输入你想输出第几项的斐波那契数：\n");
			scanf("%d", &n);
			printf("%d\n", fibonacci(n));
			system("pause");
			return 0;
		}



		2.编写一个函数实现n^k，使用递归实现

		//n^k相当于k次 n*n*...*n，函数传递两个参数，k用来判断返回，k自减！
		#define _CRT_SECURE_NO_WARNINGS
		#include <stdio.h>
		#include <stdlib.h>
		int power(int n, int k)
		{
			if(k == 0)
			{
				return 1;
			}
			if(k == 1)
			{
				return n;
			}
			return n * power(n, k - 1);
		}
		int main()
		{
			int n;
			int k;
			printf("请输入要打印的n的k次方：\n");
			scanf("%d %d", &n, &k);
			printf("%d\n", power(n, k));
			system("pause");
			return 0;
		}




		3. 写一个递归函数DigitSum(n)，输入一个非负整数，返回组成它的数字之和，
		例如，调用DigitSum(1729)，则应该返回1+7+2+9，它的和是19

		#include <stdio.h>
		#include <stdlib.h>
		int DigitSum(int n)
		{
			if(n == 0)
			{
				return 0;
			}
			return n % 10 + DigitSum(n/10);
		}
		int main()
		{
			printf("%d\n", DigitSum(1729));
			system("pause");
			return 0;
		}




		4.编写一个函数 reverse_string(char * string)（递归实现）
		实现：将参数字符串中的字符反向排列。
		要求：不能使用C函数库中的字符串操作函数。
		#include <stdio.h>
		#include <stdlib.h>
		void reverse_string(char * str)
		//*str传递的是字符串首字符的地址（指向首地址的指针）
		{
			if(*(++str) != '\0')
			{
				reverse_string(str);
			}
			printf("%c", *(str - 1));
		}
		int main()
		{
			char* ch = "abcdefg";
			printf("翻转前的字符串：%s\n", ch);
			printf("翻转后的字符串：");
			reverse_string(ch);
			printf("\n");
			system("pause");
			return 0;
		}



		5.递归和非递归分别实现strlen

		#include <stdio.h>
		#include <stdlib.h>
		int Strlen(char* str)
		{
			if(*str == '\0')
			{
				return 0;
			}
			return 1 + Strlen(str + 1);
		}
		int main()
		{
			char* ch = "abcdefg";
			int len = Strlen(ch);
			printf("%d\n", len);
			system("pause");
			return 0;
		}



		6.递归和非递归分别实现求n的阶乘

		#define _CRT_SECURE_NO_WARNINGS
		#include <stdio.h>
		#include <stdlib.h>
		int Factor(int n)
		{
			if( n <= 1)
			{
				return 1;
			}
			return n * Factor(n - 1);
		}
		int main()
		{
			int n;
			printf("请输入要计算的阶乘数：\n");
			scanf("%d", &n);
			printf("%d\n", Factor(n));
			system("pause");
			return 0;
		}




		7.递归方式实现打印一个整数的每一位

		#include <stdio.h>
		#include <stdlib.h>
		void Print(int a)
		{
			if(a > 9)
			{
				Print(a / 10);
			}
			printf("%d ", a % 10);
		}
		int main()
		{
			int a = 123456789;
			Print(a);
			printf("\n");
			system("pause");
			return 0;
		}



		/*递归函数*/  
		//递归的两个必要条件
		1、存在限制条件，当满足这个条件时，递归便不再继续。
		2、每次递归调用之后越来越接近这个限制条件。
		#include <stdio.h>
		/*
		int A();

		int A()
		{	
			static int n = 0;
			n++;
			printf("第%d次被调用！\n",n);
			
			if(n<5)
			{
				A();
			}
			
			return 0;
		}
		*/

		int Count_Data(int n);
		int Count_Data(int n)
		{
			if(n == 0)
				return 1;
			n *= Count_Data(n-1); // 函数调用自身  自己调用自己 
			
			return n;
		}

		int main()
		{
			
			printf("n! = %d\n",Count_Data(3));
			
			return 0;
		}


		//23  12  14  9  17 14  17 25
			 
七：
	内联函数：
	inline:修饰函数
	
	inline int A();
	int A()
	{
		printf("hello world!\n");
		
		return 0;
	}
	
	int main()
	{
		int A()
		{
			printf("hello world!\n");
		
			return 0;
		}
		return 0;
	}
	


		#include <stdio.h>

		inline int A();
		int A()
		{
			printf("HELLO WOLRD! \n");
			
			return 0;
		}

		int main()
		{
			
			printf("------\n");
			A();
			
			return 0;
		}


		内联函数：汇编文件(.s文件)
		.file	"inline.c"
		.section	.rodata
	.LC0:
		.string	"HELLO WOLRD! "
		.text
		.globl	A
		.type	A, @function
	A:
	.LFB0:
		.cfi_startproc
		pushq	%rbp
		.cfi_def_cfa_offset 16
		.cfi_offset 6, -16
		movq	%rsp, %rbp
		.cfi_def_cfa_register 6
		movl	$.LC0, %edi
		call	puts
		movl	$0, %eax
		popq	%rbp
		.cfi_def_cfa 7, 8
		ret
		.cfi_endproc
	.LFE0:
		.size	A, .-A
		.section	.rodata
	.LC1:
		.string	"------"
		.text
		.globl	main
		.type	main, @function
	main:
	.LFB1:
		.cfi_startproc
		pushq	%rbp
		.cfi_def_cfa_offset 16
		.cfi_offset 6, -16
		movq	%rsp, %rbp
		.cfi_def_cfa_register 6
		movl	$.LC1, %edi
		call	puts
		movl	$0, %eax
		call	A
		movl	$0, %eax
		popq	%rbp
		.cfi_def_cfa 7, 8
		ret
		.cfi_endproc
	.LFE1:
		.size	main, .-main
		.ident	"GCC: (Ubuntu 5.4.0-6ubuntu1~16.04.12) 5.4.0 20160609"
		.section	.note.GNU-stack,"",@progbits
		
	补充：	
			调用函数，需要动态开辟栈空间
	好处：
			在编译的时候，在函数的调用位置
			加入对应的函数代码段，
			速度比普通的函数调用快
			
	验证是否内联函数进行了在编译的过程加入代码段：
	
		gcc - E
		gcc - S
		
	补充：
		一般情况下，用inline修饰函数时，只是建议编译器采用内联的操作

八：内存分布
	
	一个地址一个字节。
	
	32位系统有多少个寻找空间（多少个地址） 2的32次方个地址
	（2的32次方个字节）
	
	 k  m  g
	
	32位系统：4G
		



		#include <stdio.h>


		int main()
		{
			int *p;  // 野指针  段错误  访问的内存超出了系统所给这个程序的内存空间
			*p =11; // 个人理解：定义的指针变量 就是关于地址的操作系统会自定义的给定一个初始值 如果超出了或者改变了系统给定的这个初始值就会出现段错误


			return 0;
		}




		/*变量在内存的分布*/
		#include <stdio.h>

		//全局变量，存放在数据段（静态数据），程序退出才释放
		int A;
		static int B;


		int Fun(int a); /*a和b属于局部变量，函数退出后被释放*/

		int Fun(int a)
		{
			int b;
			
			static int c;//属于静态局部变量，存放在数据段,程序退出后才释放
			
			int * p = (int *)malloc(sizeof(int ));
			//p属于局部变量，但是他指向的空间是malloc出来的，
			//所以在堆空间里面，如果不free,程序退出后才释放
			
			int *q = &b;
			
			//q属于局部变量,函数退出后被释放*/
			
			return 0;
		}


		int main()
		{
			Fun(11);	
			
			return 0;
		}




		#include <stdio.h>


		union un_1
		{
			int  a;
			char b;
			long c;
		};

		int main()
		{
			union un_1 A;
			
			A.a = 10;
			A.b = 's';
			A.c = 1000;
			
			union un_1   B = {1,'s',1000};//第一次赋值是联合体变量的值
			union un_1   C = {
								.a = 1,
								.b = 's',
								.c = 1000};
				//最后一次赋值是联合体变量的值
			
			
			
			printf("%ld---%c---%d\n",A.c,A.b,A.a);
			printf("%ld---%c---%d\n",B.c,B.b,B.a);
			printf("%ld---%c---%d\n",C.c,C.b,C.a);
			
			
			return 0;
		}



		
作业：


	1:找一下系统里面使用enum类型的系统文件
	
		文件类型：
					普通   0   reg...
					目录   1
					套接字 7
		
				enum
				{
					DT_UNKNOWN = 0,
				# define DT_UNKNOWN	DT_UNKNOWN
					DT_FIFO = 1,
				# define DT_FIFO	DT_FIFO
					DT_CHR = 2,
				# define DT_CHR		DT_CHR
					DT_DIR = 4,
				# define DT_DIR		DT_DIR
					DT_BLK = 6,
				# define DT_BLK		DT_BLK
					DT_REG = 8,
				# define DT_REG		DT_REG
					DT_LNK = 10,
				# define DT_LNK		DT_LNK
					DT_SOCK = 12,
				# define DT_SOCK	DT_SOCK
					DT_WHT = 14
				# define DT_WHT		DT_WHT
				  };
				  
	2:找生成程序的汇编文件的方法去验证是否加入代码段
	
	3:找让编译器执行内联操作的方法
	
	4:阶段总结+做习题


	2:找生成程序的汇编文件的方法去验证inline函数是否加入代码段
	答：验证的方法是开启编译优化，gcc编译器开启编译优化的选项是 -O（大写）。
	    -O选项后面可以加数字，表示优化的级别。没有数字默认是1，最大可以加到3。
	    优化级别越高，产生的代码的执行效率就越高，但是编译的过程花费的时间就会越长。
	    例子：inline.c文件
	            #include <stdio.h>
	            inline int A();
	            int A()
	            {
	            	printf("HELLO WOLRD! \n");
	            	return 0;
	            }

	            int main()
	            {
	            	printf("------\n");
	            	A();
	            	return 0;
	            }

	    通过预处理后在执行命令：gcc -O inline.i -o inline.s -S
	    生成inline.s文件（其中一部分汇编程序）
	            ......
	                A:
	            .LFB0:
	            	.cfi_startproc
	            	subq	$8, %rsp
	            	.cfi_def_cfa_offset 16
	            	movl	$.LC0, %edi
	            	call	puts
	            	movl	$0, %eax
	            ......
	            main:
	            .LFB1:
	            	.cfi_startproc
	            	subq	$8, %rsp
	            	.cfi_def_cfa_offset 16
	            	movl	$.LC1, %edi
	            	call	puts
	            	movl	$.LC0, %edi
	            	call	puts
	            	movl	$0, %eax
	            ......
	    其中原有的 call A 程序已被 A 内容代替。


	3:找让编译器执行内联操作的方法
	答：1）内联函数代码简短；
	    2）内联函数被频繁调用
	    3）执行-O选项开启编译期间优化。


	4:阶段总结
	答：这周学习的知识点有：
	    1）二维数组
	        eg：int array[常量表达式][常量表达式] //第一个为行，第二个为列
	        注意点： 列数一定要在定义的时候确定，行数可以不用
	                部分初始化赋值时，没有赋值的元素系统自动填写0或'\0'
	                array既可以代表数组，也可以表示数组的地址或首元素地址
	                array表示地址时是常量，不能array++或++array
	                array表示地址时，array+1表示二维数组的第二行第一个元素地址

    2）普通指针
        eg： int *p
        注意点： 赋值方式有两种，int *p = &a；或int *p； p = &a;
                输出地址有 "%p"
                int *a;//野指针，尽量避免，可定义为 int * a = NULL //空指针
                指针变量的类型要和存放地址对应的变量的类型一致
                *p++:先引用，再地址+1，数据不变
                *(p++):先引用，再地址+1，数据不变
                (*p)++:整型指针中，先引用，再对应数据+1；但再字符型指针中不能用，原因是指向的数据是常量

    3)指针传参
        格式：int func(int * argument){}
                    int main() {...int *p = &a;func(p);....}

    4)手动申请空间
        函数：void *malloc(size_t size);
             void *calloc(size_t nmemb, size_t size);
            申请成功返回内存空间首地址，否则返回NULL
            申请后需要手动free(*p);

    5)指针函数
        格式：int *func(int * argument){}
                    int main() {...int *p = &a;func(p);....}
            可以返回一个指针值（地址）

    6）函数指针变量：存放函数的地址（函数也是一种变量）
        定义格式：返回值类型 (*函数变量名) (形参类型);
        初始化：int (*fun_p)() = &a; //  函数a()
        解引用就是调用函数(回调函数)：(*fun_p)();//调用了a函数
        函数返回指针：指针函数（他是一个函数，返回值为指针）

    7)存储类
        auto:局部变量使用
        register:获取数据效率比内存快
        static:内存数据不释放
        extern:一般在定义全局变量的时候使用,在不同.c文件中引用相同变量值

    8）数组指针：是一个指针变量，存放的是数组的地址
        定义：int (*array_p)[]
        赋值：array_p = &array //array是数组地址
        传参：int func(int (*p)[]){}  //传参函数
             int main(){...int (*array_p)[3] = &array;func(array_p);...}//主函数
        注意：申请空间int (*a_p)[M] = (int(*)[])malloc(M*sizeof(int));

    9)指针数组：是一个数组，存放多个指针变量
        定义：int *array[]
        赋值：int a = 1;  array[0] = &a;//循环赋值
        传参：int Fun(char * c_array[]){}//传参函数
            int main()  {char * c_array[10];Fun(c_array);}//主函数
        注意：array： 是一个指针数组
            array+1:  指针数组中第二个元素的地址
        	*array:  第一个元素存放的数据
        	*(array+1):第二个元素存放的数据
        	**(array+1):根据第二个元素存放的数据（是一个变量的地址）来解引用

    10)结构体
        定义：struct ST_1 { int a; long b;};
            struct ST_1 A;
        赋值：A.a = 1;或*p = &A；p->a = 1;
        传参：int Fun(struct ST_1 xxx){}//传参函数
            int main(){...Fun(A);...}
        结构体返回值：struct ST_1 fun(struct ST_1 xxx){...return xxx}//传参函数
                    int main(){...struct ST_1 B=Fun(A);...}//主函数

    11)结构体指针与结构体数组
        类似普通指针和数组一样结合结构体使用

    12）结构体数组指针与结构体指针数组
        类似数组指针和指针数组结合结构体使用

    13）联合体、枚举与const
        联合体内变量共用一个内存空间，空间大小为成员中类型占用字节最大的；
        union un_1   A = {1,,1000}；//第一次赋值是联合体变量的值
        union un_1   A = {.a = 1,
                          .c = 1000}；//最后一次赋值是联合体变量的值
		枚举值是常量，不能修改
		枚举类型可以不需要定义变量，直接使用枚举值
		枚举类型大小4个字节
        const int * a和int const * a//*a指向的数据不能通过*a来改变，但是指向数据可以通过其他方式改变
        int * const a//a里面的数据不能通过a来改变

    14)递归函数、内联函数
        递归函数经常在索引或逐级处理数据等情况使用节省代码量
        内联函数要求代码简短、被调用频繁时使用，编译时可以直接嵌入到程序，节省调用时间

    15）学习过程中的问题处理注意点
        数组没有返回值，不能array++或++array
        数组名str可以（*str）++，但是字符指针str不能(*p)++
        printf输出是从右往左运算，整型是++a能影响缓存输出，但是输出数组++a[0]和指针++(*p)没有影响
        函数传参的形参和实参类型要一致，返回值和函数定义类型有关



		一、二维数组
		定义 int arr[n] [m];其中n控制一维数组的个数，m控制每一个一维数组里元素的个数，列数一定要在定义的时候确定。
		初始化： int arr[2][2] = {{1,2},{3,4}};
		Int arr[2][2] = {1,2,3,4};
		初始化的时候假如没有全部初始化，没有初始化的元素为0
		二、申请内存空间
		在定义局部变量的时候，系统会自动为变量申请空间，（如int a = 10;）内存将在相应函数退出后自动释放。用户也可以手动申请在用完不需要用的时候手动释放掉。
		申请内存函数
		Void * malloc(size_t size);
		Void * calloc(size_t nmemb, size_t size);
		(void *)为无类型指针--万能指针，可以给调用者使用任意一种指针类型
		例
		Void * A(void * data)
		{
		Int * new_p = (int *)data;
		Printf(“%d\n”,*new_p);
		Return NULL;
		}
		Int main()
		{
		Int a =11;
		A((void *)&a);
		Return 0;
		}
		Size的单位一般为字节
		返回值：返回成功申请到的内存空间的首地址
		释放
		Free();
		释放内存时，传出free函数的地址一定时malloc/calloc申请到的空间的首地址，否则会释放失败
		例
		1.Int * p = (int *)malloc(sizeof(int));
		2.int * p = NULL; p =(*)malloc(4);
		补充
		通过malloc/calloc手动申请的存放在堆空间里，随着整个程序的退出内存才会释放掉，所以不再需要用到申请的空间时需要手动free()释放内存。
		Malloc/calloc的区别在于malloc只申请空间而不会对内存里面的数据进行清空初始化，calloc会清空为0；
		例子
		Malloc(4*sizeof(int));
		Calloc(4,sizeof(int));

		三、指针类型
		指针：用来存放变量的地址，指针变量和存放的地址对应的变量类型一致。
		定义 int * a;char *a ;
		初始化与赋值：
		野指针：int * p; printf(“%d\n”,*p);
		非法，会发生段错误
		指针作用：1.存放变量地址
		2.根据指针变量存放的地址找到地址上面存放的数据（解引用）
		例 整形指针int *
		Int a = 10;
		Int * b = &a;
		Printf(“%d---%d”,*b,a);//根据b里面的地址获取对应数据a//
		结果 10---10
		字符指针char *
		例
		1.  	  
		2.char p * = “hello”;//此时p存放的是首元素’h’的地址
		补充：
		数组可以看成是一个特殊的一维指针
		Int a[10] = {1,2,3};
		Array[0] : 表示数组的首元素
		Array：数组名 也是数组首元素的地址----->&arrary[0]
		Array+1:数组第二个元素的地址----->&array[1]
		*(array+1):数组的第二个元素----->array[1]
		/*/

		该程序中 二维数组a看成有两个元素a[0] a[1]，每个元素为一个一维数组
		所以对于 pa = a而不是pa = &a的理解为：
		*pa为一个数组指针，指向二维数组里的一维数组
		Pa：里面存放的是a[0]的地址 a[0] = {1,2,3}
		a:   指向a的首元素即a[0]的地址
		类似的：

		P = c -------->p指向c[0]的地址，
		*(p[0]+2) ------>*(*(p)+2)--->c[0]地址加2对应的元素---->c[0][2]
		P+1     ------>c[0]地址+1----->c[1]的地址
		*(p+3)  ------->c[0]地址+3对应的元素-------->c[3][0]地址
		*(p+1)+3-------->c[0]的地址+1对应的元素+3--------->c[1][3]的地址
		*(*(p+1)+3)------>c[1][3]    c[1]地址+3对应元素
		*((*p)+6)---------->c[1][3]	    c[0]地址+6对应元素
		*(*(p+5))---------->c[5][0]    c[5][0]         
		                                      
		四、函数指针变量：存放函数的地址
		返回值类型(* 函数名)(形参)
		函数指针变量的定义 赋值 解引用
		例			int (*p)(int *,char *);
		 			P = &A;
		(*p)(NULL, NULL);
		函数指针的传参
		//例： 封装一个函数，比较两个字符大小，输出大的，再封装一个函数，利用函数指针调用它
		Char sel(char a ,char b)  			//实现比较字符大小，接收两个字符变量，返回最									//大的字符
		{
		Char max ;
		max = (a>b)?a:b;
		Return max;
		}
		Int work(char (*p)(char ,char))		//实现调用sel函数，接受函数指针，成功返回0	
		{
		Char a = ‘a’;
		Charb = ‘b’;
		Char ret = (*p)(a,b);	//函数指针解引用
		Printf(“%c\n”,ret);
		Return 0;
		}
		Int main()						
		{
		Char (*p)(char , char) = &sel;//定义并初始化一个字符型函数指针，指向								 //sel函数
		Work(p);					 //调用调用函数work来调用sel
		Return 0;
		}

		五、数组指针变量：存放数组的地址
		定义： int (*array_p)[];
		 Int (*array_p)[10];
		赋值： array_p = &array;
		解引用：
		例  int main()
		{
		Int data[5];
		Int (*arr_p)[5] = &data;
		For(int i = 0;i<5;i++)
		{
		Printf(“%d\n”,*((*arr_p)+i））
		//等价于(*arr_p[i])//
		}
		Return 0;
		}
		/*/data：表示整个数组或首元素地址  add_p:数组指针	
		Data+1:首元素地址加1	
		*add_p:对数组指针的解引用，获得数组(数组名)
		(*add_p)+1:第二个元素的地址，等价&data[1]
		*((*add_p)+1):第二个元素，等价data[1]
		数组指针传参
		/*/例 封装一个接受形参类型为数组指针的函数，实现打印一串字符串功能
		Int tran(char(*p)[ ])				//接受数组指针类型的形参，实现打印字符串功能，									//成功返回0
		{
		For(int i = 0;i<5;i++)
		{
		Printf(“%s”,(*p)[i]);		//数组指针的解引用，得到数组(数组名)
		Return 0;
		}
		}
		Int main()
		{
		Char chr[5] = “hello”;
		Char(*chr_p)[5] = &chr;			//定义数组指针，指向数组chr
		Tran(chr_p);					//传参
		Return 0;
		}
		六、指针数组  是一个数组，用来存放多个指针变量
		以整形指针数组为例
		定义 ：int * array[10];
		赋值 ： 
		1.直接赋值    array[i] = &p;
		2.通过malloc给数组申请空间
		- for(int i = 0;i<10;i++)
		Array[i] = (int *)malloc(sizeof(int));
		- for(int i = 10;i<10;i++)
		*(array[i]) = i;
		- for(int i = 0;i<1;i++)
		Printf(“%d”,*(array[i]));
		- for(int i =0;i<10;i++)
		Free(array[i]);
		/*/array:是一个数组，也是数组首元素地址
		array+i：是第i个元素的地址
		Array[0]:数组首元素   
		*array：首元素里面存放的数据(这里是一个地址)
		*(array+i):是第i个元素里面存放的数据(是一个地址)
		**(array+i)：根据第i个元素存放的数据指向的变量

		七、存储类关键词
		1.自动变量 ：  auto 只能给局部使用 
		auto int a ; 
		2.寄存器存储： register 
		Register int a ;获取数据效率快，大小为寄存器大小，在内存里面没有地址
		所以：
		Register int a = 3;
		Printf(“%p”,a);运行会报错
		3.静态变量 static ：在函数里面定义的，用于设置变量存储域，只初始化一次：
		//定义一个函数A
		  Int A()
		{	
		Int a = 11;
		a++;
		Printf(“%d\n”,a); 
		}
		Int main()
		{
		A();
		A();
		}
		程序运行结果为12和13
		4.extern 外部变量（一般定义全局变量的时候使用）
		声明：     extern int a ;
		赋值：	  a = 12;
		5.const 被修饰的变量成为常变量，即初始化后能再赋值修改
		i.Const int a = 11;
		- a =12;     //报错，常变量不能再赋值修改
		ii. Int b = 11；
		Const int * a = &b;即表示*a指向的数据不能改
		- a = 12;   ×
		  b = 12    √  //因为是*a不能变，b本身是局部变量可变
		补充：全局和局部变量同名时，全局变量优先于局部变量
		全局变量没有初始化的时候清零
		八、结构体类型 ：可以存放多个成员
		特点：结构体里面的成员类型可以不一致
		作用：封装分类数据
		标识符：struct(是标识符，不是类型)
		结构体定义  struct 类型名{成员}变量名；
		1.struct A{
		 		Int a ;
		Char b;
		}data,....;	
		2.struct {
		      Int a;
		       Char b;
		}data,*p....;   //这种和第一种的区别在于，这种没有类型名，定义固定的变量，后面不//能再定义
		赋值：
		例
		Struct A{
		 Int a ;
		 Int b;
		};
		1.Struct A aaa = {
						.a = 1,
						.b = 2};
		2.struct A bbb = {
						3,
						4};
		Printf(“%d---%d---%d---%d”,aaa.a,aaa.b,bbb.a,bbb.b);			
		结构体变量可以直接赋值数据
		Str_1 = str_2
		/*/
		例
		Struct ST {
			Char name[10];
			Int age;
			};
		Int FUN(struct ST someone)
		{
			Printf(“%s---%d”,someone.name,someone.age0;
			Return 0;
		}
		Int main()
		{
			Struct ST xm;
			Char * p = “xm”;
			Strcpy(xm.name,p);//这里不能直接定义了之后又赋值即
								  Xm.name = “xm”;除非初始化的时候就确定
								  Struct ST xm = {.name = “xm”,.age = 8};
								或struct ST xm = {“xm” , 8};

			Xm.age = 8;
			FUN(xm);
			Return 0;
		}
		九、结构体指针
		定义 struct A * st_p = &B  
		1.(*st_p).a = xxx;//等价B.a = xxx;
		2.St_p -> a = xxx;
		3.(&B) -> a = xxx;
		给结构体指针申请空间
		Typedef struct student_inf
		{
		Int a ;
		Int b;
		}s_i , *p_si;
		Int main()
		{
		P_si xm = (p_si)malloc(sizeof(s_i));
		If(xm == NULL)
		{
		          Perror(“malloc error”);
		          Return 0;
		}
		(*xm).a = 1;
		(*xm).b = 2;
		Return 0;
		}
		补充
		1.计算结构体大小：
		字节对齐
		CPU 字节：32位系统-->4字节
		64位系统-->8字节
		先计算结构体里面最大的m值，结构体大小为m的整数倍
		2.typedef的使用 ：给数据类型起别名 提高可读性，提高编码效率
		Typedef struct A (类型名){...};new（新的名字）
		例
		Typedef struct student_message{
				Int num;
				Char name;
				}msg;
		Int main()
		{
		Struct student_message XM;
		XM.a = 10;
		Msg XH;             //两种定义等价
		XH.a = 11;
		Return 0;
		}
		十、结构体数组
		定义 struct 类型名 array[n] ;

		Struct A {
			Int a ;
			Int b;
			} ;
		     struct A arr[3];
		     Arr[0].a = 11;
		     (*arr).b = 12;     
		赋值 ps[0].a = 11;
		   或（*ps）.a = 11;
		   或  ps->a = 11;
		十一、结构体数组指针
		定义 
		struct A (*arr)[10] = (struct A(*)[10])malloc(sizeof(struct A))
		十二、联合体（共同体）union
		特点：共同体里面的所有成员都是共用一块内存空间，减少内存消耗但是访问数据的效率低
		定义      union 类型名{
			  Int a ;
			  Char b ;
				Long c;}变量名；
		任意时刻只有一个成员有效，数据会覆盖
		例1
		Union un_1{ 
		     Int a ;
		     Int b ;
		}data;
		Union un_1 A = {1,2};
		//实际会报警告Printf(“%d---%d”,A.a,A.b);
		结果输出  1---1   第一次赋值的值是union的值

		例2
		Union un_2 B = {
				.a = 3,
				.b = 4};
		Printf(“%d---%d”,B.a ,B.b);
		输出结果   4---4   最后一次赋值的值是union的值
		/*/共用体虽然和结构体相似，但共用体的大小计算和结构体不同，共用体的大小为最大成员的大小
		十三、枚举 enum
		是整数常量集合，并呈加一递增，由于是常量，再初始化后是不能再重新赋值修改的
		定义：
		 Enum en_1{
		 Mon,//不初始化的话默认为0
		 Tue,
		 Wed,
		 Thur = 5，
		 Fri,
		 Sat = -1，
		 Sun   }
		  Enum en_1 A = mon;
		Printf(“%d”,A);
		结果为0
		//对应0，1，2，5，6，-1，0  加1递增//
		十四、宏定义macro
		1.普通宏定义  #define M 10 
		2.条件调试 gcc xxx.c -o xxx -D 条件编译



		3.宏定义函数
			单行定义：#define MAX(a,b) a>b?a:b
			多行定义：
		#define SWAG(a,b){\
			\a = a+b;\
			\b = a-b;\
			\a = a-b;\
			}                     //’\’是换行符
		十五、Linux下的GDB调试
		      gcc xxx.c -o xxx -g  
		      Gdb xxx
		      Start
		      N：一行行执行
		      Q:退出
		      P：在执行过程中，输入p 变量名或 p &变量名 可以查看当前某变量的值
		      B：设置断点(breakpoint) b 行数
		      D：删除断点             d 断点编号
		/**********让gdb生成存放堆栈内存调试信息的core文件*******/
		1.查看默认core文件的大小    Ulimit -c
		如果结果为零，先设置core大小
		Ulimit -c unlimited
		Ulimit -c 1024//设置成1024字节
		2.生成core文件   gcc xxx.c -o xxx -g
		   ./xxx             
		//要先运行错误的程序才会生成core(可以ls命令看一下有没有生成core)
		4.使用gdb解析core
		      Gdb -c core xxx(二进制可执行程序)
		十六、递归函数
		/*/例 求某数阶乘
		Int cnt(int n)
		{
		If(n == 0)
		 Return 1;
		N *= cnt(n-1);
		Return n;
		}
		Int main()
		{
		Printf(“n! = %d”,cnt(n));
		Return 0;
		}
		十七、内联函数 inline
		速度的比普通调用函数快，调用的函数需要动态开辟栈空间，用空间代价换取时间效率
		Static Inline int A();
		Static Inline int A(){
		        ...........
		        Return 0;
			}
		定义声明时最好在前面加上static 
		另外，inline只是一个建议，提醒编译器这是一个内联函数，但编译器不一定会采取建议实现内联，如果一定要内联则可以
		Static inline __attribute__((always_inline)) int A();
		可以利用objdump命令查看反汇编文件来确定有没有实现内联，如果有调用函数的过程说明没有真正实现内联
		栈：存放局部变量，系统自动申请，内存系统自动释放；环境变量和命令行参数放在栈底
		堆：用户自定义的，用户自己释放（malloc calloc）
		数据段：    bss：普通全局变量，静态全局变量，静态局部变量
			 未初始化的静态数据会初始化为0
		            Data:已初始化的数据
		            Rodata:常量
		代码段：    .test段：用户自定义的函数
		            .init段：系统自动添加的初始化代码，
				实现环境变量的准备和命令行参数


### 省钱利器：
![](/hexo-private-blog-website/images/淘宝客30.jpg)
![](/hexo-private-blog-website/images/淘宝客31.jpg)
![](/hexo-private-blog-website/images/淘宝客32.jpg)

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
