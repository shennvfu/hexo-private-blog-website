---
title: C_Language_Basics_05
date: 2020-07-27 14:32:18
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).


这是一个链接 [菜鸟教程][Runoob]

[Runoob]: http://www.runoob.com/

# C语言基础
		一、数组作用

				
		1：微型处理器开发（首选数据）内存大小固定的，类型也是固定的

				
		2：数组越界编译不会出错，运行有可能出错,不能越界。
					
					
		int array[3] = {1,2,3};


		二、显示时间的程序，看show_time.c



		三、标准输入输出函数


			
		输出：
					
		printf

					
		1）puts :输出打印一个字符串
						
						
		char buf[] = "helloworld"

						
		int ret = puts(buf);
						
		补充：puts的返回值是有效的输出字符数，会自动换行，属于行缓存

					
		2）fputs：往文件流指针指定的文件输入一个字符串
						
		char buf[] = "你好"
						
		fputs(buf,stdout);
						
					
		3)putchar:输出打印一个字符,不会自动换行
						
		char data='s';
						
		putchar(data);

					
		4)fputc:往文件流指针指定的输入文件一个字符
						
		char data = 's';
						
		fputc(data,stdout);

					
		5)putc:输出打印一个字符

						
		char data='s';
						
		putc(data);	
					
			
		输入：
					
		scanf

					
		gets:键盘接受一个字符串，直到遇到\n符号才停止接受,把字符串存放指定的变量里面qq
					
					
		char buf[10];
					
		gets(buf);//键盘接受字符串
					
		puts(buf);


					
		fgets：控制接受的字符的个数,会补'\0'

					
		getchar:键盘接受一个字符，并函数的返回值返回的字符存放到一个字符变量里面，这个字符变量需要自己定义

						
		char data;
						
		data = getchar();
						
		putchar(data);


					
		fgetc: 
						
		从文件指针指定的文件里面读取一个字符的数据，并把数据存放那个到一个字符变量里面。

						
		char data;
						
		data = fgetc(stdin);



		#include <stdio.h>

		int main()
		{
				
			char data;
			data = getchar();
			putchar(data);
			
			return 0;
		}

		#include <stdio.h>
		/*puts,fputc*/
		int main()
		{
			char buf[] = "你好！";
			fputs(buf,stdout);
			char data = '\n';
			fputc(data,stdout);
			return 0;	
		}

		#include <stdio.h>

		int main()
		{
			char data;
			data = fgetc(stdin);
			
			fputc(data,stdout);
			
			return 0;	
		}

		#include <stdio.h>

		int main()
		{
			
			char buf[10];
			gets(buf);//键盘接收字符串
			puts(buf);

			fgets(buf,10,stdin);
			puts(buf);

			return 0;
		}


		#include <stdio.h>
		/*puts putchar的例子*/
		int main()
		{
			
			char buf[] = "hiiiii";
			int ret = puts(buf);
		//	printf("%d\n",ret);
			
			char data_1 = 'x';
			//char data_2 = '\n';
			putchar(data_1);
		//	putchar('\n');
		//	putchar(10);

			return 0;	
		}

					

		---------------------------------------------------下午------------------------------------

		一、字符数组 (在使用一个没有初始刷的数组时，要有先清空数组在使用的习惯)

			
		1.1 字符数组的定义：
						
		char buf[10] = '\0';//定义一个空字符 !!!!错的定义！！！
						
		char buf[10]; 
						
		char buf[10] = {'\0'};
						
		char buf[10] = "\0";//定义一个空的数组

			
		1.2 字符数组的赋值
						
						
		buf = "helloworld"//！！！！错误！！！！

						
		for(int n=0; n<10)
							
		buf[n] = 'o';

			
		1.3 字符数组的初始化

						
		char buf[] = "helloworld";
						
		char buf[10] = "hello";
						
		char buf[10] = {'a','b'};

			
		1.4 字符数组的打印：
						
		int m = strlen(buf);
						
		for(int n=0; n<m; n++)
							
		printf("%c\n",buf[n]);

						
		printf("%s\n",buf);
						
						
		strcpy(buf,"hellw\0orld");//strcpy从h的地址开始一个一个字符的拷贝直到遇到\0
			
		习惯：定义数组的时候，多一个字节的空间，自己手动给字符串最后面补上\0


		#include <stdio.h>
		#include <string.h>

		/*字符数组的打印*/

		int main()
		{
			char buf[] = "helloworld";

			int m = strlen(buf);
			for(int n=0; n<m; n++)
				printf("%c\n",buf[n]);

			printf("%s\n",buf);


			char buff[] = "中O国";
			printf("%c\n",buff[3]);//O
			printf("%c%c%c\n",buff[4],buff[5],buff[6]);//国
			char array[3] = "\\n";
			printf("%ld\n",strlen(array));
			printf("%s\n",array);
			return 0;
		}


		#include <stdio.h>

		int main()
		{
			
		   int array[20] = {1,0,1,0,0,1,0,1,1,1,
		                    0,1,0,1,1,0,1,0,1,0};

			int mask[2] = {0,0};//存放计算0和1的个数

			for(int n=0; n<20; n++)
				mask[array[n]]++;
			
			printf("数组中0的个数为：%d,1的个数为%d\n",mask[0],mask[1]);
			return 0;	
		}



			
		练习：
					
		假设有个字符数组的数据是"中O国"
					
		让你写一份程序，打印出数组里面的0;

					
		char buf[] = "中O国";
					
		printf("%c\n",buf[3]);//O
					
		printf("%c%c%c\n",buf[4],buf[5],buf[6]);//国


			
		1.5 字符串数组传参

				
		int char_array_fun(char new_buf[]);
				
		int char_array_fun(char new_buf[])
				
		{
					
		for(int n=0; n< strlen(new_buf); n++)
					
		{
						
		if(new_buf[n] == 'w')
							
		printf("%c\n",new_buf[n]);
						
		else
							
		continue;
					
		}

					
		return 0;
				
		}


				
		int main()
				
		{
					
		char buf[] = "helloworld";

					
		char_array_fun(buf);

					
		return 0;
				
		}

			
		注意：
					
		\\n:2个字节长度，第一个\起到了取消转义，不计算在字符串的长度中。
					
		strlen只计算\0前面的字符长度
					
		helle\\nw:8个字符长度
			
			
		练习：
					
		自己在main函数里面定义一个字符数组，然后自己键盘输入数据字符给这个数组，
					
		封装一个函数来统计这个数组里面大于h的字母有那一些。
			
			
		1.6 字符串操作函数

				
		strlen:计算字符串的长度，不计\0
			
		strcpy:字符串拷贝函数
				
		strcmp：字符串判断函数（判断两个字符串相不相等）
						
		strcmp(s1,s2);
						
						
		相等：0
						
		s1>s2:大于0
						
		s1<s2:小于0

						
				
		strcat:字符串拼接函数
						
		strcat(dest,src);//吧src对应的字符串拼接到dest的后面

						
		char dest[30] = "中";
						
		char src[10] = "国";
						
		strcat(dest,src);
						
		puts(dest);
						
		puts(strcat(dest,src));

				
		练习：
						
		自己封装一个函数，模仿strcpyAPI的功能,不能调用其他字符串的操作函数。

						
		char buf_1[20]; //存放拷贝的数据
						
		char buf_2[20];//存放要拷贝的数据


		#include <stdio.h>


		/*
		 *返回值：
		 			-2：buf_2是空的
					-1：buf_1的空间不够
		 *			0 ：拷贝成功
		 * */

		int My_Strcpy(char buf_1[],char buf_2[]);
		int My_Strlen(char buf[]);

		int My_Strlen(char buf[])
		{
			for(int num=0; buf[num]!='\0'; num++)

			return num;	
		}


		int My_Strcpy(char buf_1[],char buf_2[])
		{
			//判断buf_2是不是空的
			if(buf_2[0] == '\0') 
				return -2;
			else if(My_Strlen(buf_1) < My_Strlen(buf_2))
				return -1;
			else
			{
				int num=0;
				while(1)
				{
					buf_1[num] = buf_2[num];

					if(buf_2[num] == '\0') break;

					num++;
				}
			}

			return 0;
		}


		int main()
		{
			char buf_1[20] = "hello";
			char buf_2[20] = "world"; 
			
			puts(buf_1);

			My_Strcpy(buf_1,buf_2);

			puts(buf_1);

			return 0;	
		}



			#include <stdio.h>
			#include <unistd.h>
			#include <stdlib.h>

			int Count_Sec(int sec);
			int Count_Min(int min);
			int Count_Hour(int hour);

			int Count_Sec(int sec)
			{
				if(++sec == 60)
					sec = 0;
				
				return sec;	
			}

			int Count_Min(int min)
			{

				if(++min == 60)
						min = 0;
				
				return min;
			}

			int Count_Hour(int hour)
			{
				if(++hour == 24)
					hour = 0;

				return hour;	
			}

			int main()
			{
				system("clear");//专门执行脚本命令
				int hour,min,sec;
				printf("请输入初始化时间：");
				scanf("%d%d%d",&hour,&min,&sec);
				
				while(1)
				{
					
					sleep(1);
					sec = Count_Sec(sec);
					if(sec == 0)
					{
						min = Count_Min(min);
						if(min == 0)
						{
							hour = Count_Hour(hour);	
						}
					}
				
					system("clear");//专门执行脚本命令
					printf("当前时间为：%d--%d--%d\n",hour,min,sec);

				}
				
				return 0;	
			}


			#include <stdio.h>
			#include <string.h>

			int main()
			{
				
				char dest[30] = "中";
				char src[10] = "国";
				strcat(dest,src);
				puts(dest);
				puts(strcat(dest,src));

				printf("%p---%p\n",dest,strcat(dest,src));


				return 0;	
			}



			#include <stdio.h>
			#include <string.h>

			int main()
			{
				char buf_1[] = "hello";
				char buf_2[] = "heggo";

				int ret = strcmp(buf_1,buf_2);
				if(ret == 0)
				{
					printf("两个字符串相等！\n");	
				}
				else if(ret > 0)
				{
					printf("buf_1>buf_2\n");	
				}
				else
				{
					printf("buf_1<buf_2\n");	
				}
				
				printf("%d\n",ret);
				return 0;	
			}



			#include <stdio.h>
			#include <string.h>


			int main()
			{
				char buf[20];

				strcpy(buf,"hello\0world");

				puts(buf);
				
				return 0;	
			}



			/*字符数组传参的练习*
			 *
			 */
			#include <stdio.h>
			#include <string.h>
			#define M 20
			int Check_Char_Data(char char_array[]);
			int Check_Char_Data(char char_array[])
			{
				if(char_array[0] != '\0')//·判断数组是否为空
				{
					int num=0;

					printf("该字符串数组中大于h的有：");
					do
					{
						if(char_array[num] > 'h')  //这里不能填num++,先输出num再++
							printf("%c ",char_array[num]); //导致了这里打印的是num+1对应的元素
						num++;
					}
					while(char_array[num] !='\0');

					printf("\n");
				}
				else //数组为空
				{
					printf("数组为空！！！\n");
					return -1;
				}

				return 0;
			}


			int main()
			{
				char array[M];
				printf("请输入字符数据：");
				scanf("%s",array);
				Check_Char_Data(array);

				return 0;	
			}


		总结一周的知识点：

			
		技术背景（嵌入式+C语言<面向过程>）
			
		相对路径和绝对路径（系统位数+系统版本，/目录下每个目录的作用）
			
		数据类型：
					
		存放形式
					
		取值范围
					
		各个类型的输出格式（%p打印地址）
					
		类型转换：
							
		强制转换：
										
		int a  = 10;
										
		printf("%d\n",(float)a);
										
		float b = (float)a;
					
		ascii:小写-大写 = 32
							
		man ascii
					
		转义字符：
			
		运算符：
					
		优先级
					
		三木运算符： ？ ： ：
			
								
		int data = a>b?a:b;
								
		a>b?printf("a>b"):printf(a<b);
			
		控制流语句：

					
		死循环：
							
		while(1) 
		{
		}

		   
		while(-1);  
		for(; ; )  
		{
		}
		for( ; ; );
																			
					
		switch里面有没有break.

					
					
		for()
					
		{
						
		for()	
					
		}

					
		小的for循环完，大的for才循环1次
			
			
		函数：
					
		自己编码函数的流程：
										
		声明    定义   封装  调用

										
		返回值、行参的类型是在声明和定义时候才写的

					
		函数名就是函数的地址，函数也是变量


					
		函数能不能嵌套调用：能

			数组：

					
		数组首地址和数组名字和数组首元素的地址是一样的
					
		数组不能越界，编译不会报错，运行有可能出错。
					
		数组是不能当初函数的返回值返回（函数不能返回一个数组类型）
					
		字符数组和整形数组各个定义初始化的格式。

					
		灵活使用控制数组里面的每一个元素。

					
		多学习系统提供的API，看这些API的源码。


		课堂笔记：
		MAC地址都是唯一的
		指针就是地址
		栈是先进后出的
		先编译的低地址
		后定义的是高地址
		一般情况下栈顶是高地址
		一般情况下栈底是低地址
		不同编译器是不一样的
		局部变量放在栈空间里面
		段错误  地址非法使用  内存地址错误  空间不够
		变量才有合法地址
		如果一开始不知道给指针变量赋什么值 那可以给它赋值为空NULL
		malloc中的局部变量是存放到堆空间里面的 它是等到程序退出时才将它进行释放  
		就是等main函数退出时它才进行释放



		作业
			自学ungetc函数的用法，写一份对应的例子模板

			1.自学ungetc函数的用法，写一份对应的例子模板
			#include "stdio.h"

			int main(int argc, char const *argv[])
			{
				// 函数原型: int ungetc(int c, FILE *stream); //该函数API不会进行换行操作

				//int data = 'A';

				int ret = ungetc('A',stdin);

				//ungetc(data,stdin);

				putchar(ret);

				printf("\n");

				return 0;
			}
			
		待会飞秋共享一本c语言api手册，这周末学3个函数。

				
		写出该函数的对应的例子。

			
		自己写总结。


	一、

	自学ungetc函数的用法，写一份对应的例子模板




	答：函数形式：int ungetc(int c, FILE *stream)   头文件包含：#include <stdio.h>
		函数作用：把一个字符退回到stream代表的文件流中,以便它是下一个被读取到的字符。
		参数1：int c 用整星形式表示字符
		参数2：文件流指针，必须是输入流不能是输出流
		返回值：成功---->字符c
			失败---->EOF
		eg:   
			#include <stdio.h>



			int main()

			{
		
			    char array[] = "ab";


		
			    printf("操作之前：");
	//验证之前数据	
			    puts(array);


		
			    array[0] = ungetc('d', stdin);
	 //函数操作，没有手动输出，自动获取
			    array[1] = getchar();


		
			    printf("操作之后：");
	 //验证之后数据对比	
			    puts(array);

			}



	二、看c语言api手册，这周末学3个函数。写出该函数的对应的例子

	答：
		1)函数：int isalnum(int c)
	
		作用：检查 c 字符是否为数字或者英文字符
		头文件：#include<ctype.h>
		形参c：是一个整型，可以表示一个字符
		返回值：若是数字或英文，则返回true（非0），否则返回0.
	eg：
		#include <stdio.h>
		#include <ctype.h>

		int main()
		{
		    char check_vale[4] = "a1&";

		    for(int i = 0; i < 3; i++)
		    {
		        printf("%d\n", isalnum(check_vale[i]));
		    }
		}
		
		输出结果：8
			  8
			  0
		（经测试，字母大小写输出一样）


	2）函数：int isalpha(int c)
	
		作用：检查 c 字符是否为英文字符
		头文件：#include<ctype.h>
		形参c：是一个整型，可以表示一个字符
		返回值：若是英文，则返回true（非0），否则返回0.
	eg：
		#include <stdio.h>
		#include <ctype.h>

		int main()
		{
		    char check_vale[4] = "a1&";

		    for(int i = 0; i < 3; i++)
		    {
		        printf("%d\n", isalpha(check_vale[i]));
		    }
		}
		
		输出结果：1024
			  0
			  0
		（经测试，字母大小写输出都一样）

	
	3）int isascii(int c)
	
		作用：检查 c 字符是否对应有ASCII码字符
		头文件：#include<ctype.h>
		形参c：是一个整型，可以表示一个字符
		返回值：若是对应有ASCII码字符，则返回true（非0），否则返回0.
		补充：ascii码对应的整型数为0~127
	eg：
		#include <stdio.h>
		#include <ctype.h>

		int main()
		{
		    char check_vale[4] = ｛'d', 137, 23};

		    for(int i = 0; i < 3; i++)
		    {
		        printf("%d\n", isascii(check_vale[i]));
		    }
		}
		
		输出结果：1
			  0
			  1


		1.自学ungetc函数的用法，写一份对应的例子模板
		答：
		#include <stdio.h>


		int main()
		{
			char ch = 'a';
			
			int ret = ungetc(ch,stdin);

			printf("%c\n%d\n",ch,ret);	

			return 0;	
		}


		自学ungetc函数的用法，写一份对应的例子模板
		程序：
		/*
		 函数原形：
				int ungetc(int c, FILE *stream)；
		 函数作用：
		  		  将指定字符c写回文件流，返回该写回字符
		 参数  ：
		 				int c ：需要写回的指定字符c ； 
				FILE * 	stream：接收写回字符文件流
		 返回值：
		 		成功：返回该写回字符c
		 */	
		#include <stdio.h>
		int main(int argc, char const *argv[])
		{

			char ret;
			ret = ungetc(84, stdin);
			putchar(ret);
			printf("\n");
			return 0;

		}


		1、查看C语言API手册，这周末学3个函数。写出该函数的对应的例子。
		答：
		①rindex
		 	函数头文件：#include <string.h>
		函数原型：char *rindex(const char *s, int c);
		*s：被查找的字符串
		c：被查找的字符
		返回值：如果找到指定的字符则返回该字符所在的地址，否则返回0。
		功能：查找字符串s中最后一个出现的指定字符c的地址，然后将该字符出现的地址返回。字符串结束字符(NULL)也视为字符串一部分。
		程序：
		#include <stdio.h>
		#include <string.h>

		int main()
		{
			char buf[] = "438978584418656";
			char *p = rindex(buf,'8');
			printf("%s\n",p);
			
			return 0;	
		}


		②fprintf
		函数头文件：#include <stdio.h>
		函数原型：int fprintf(FILE *stream, const char *format, ...); 
		FILE *stream：文件流指针，将结果输出到指定文件中直到出现“\0”为止。
		*format：要被写入到流 stream 中的文本
		返回值：如果找到指定的字符则返回该字符所在的地址，否则返回0。
		功能：格式化输出数据至文件
		程序：
		#include <stdio.h>

		int main()
		{
			int a = 100;
			int b = -89;
			double c = 52.857488888888;

			fprintf(stdout,"%d,%f,%o\n",b,c,a);
			fprintf(stdout,"%d,%*d\n",a,1,a);
			
			return 0;	
		}


		③strtol
		函数头文件：#include <stlib.h>
		函数原型：long int strtol(const char *nptr, char **endptr, int base); 
		*nptr：要转换的字符串
		**endptr：NULL符合条件。
		base：要采用的进制方式，如为10则采用十进制。范围从2至36，或0.
		返回值：返回转换后的长整型数，否则返回ERANGE并将错误代码存入errno中。 
		功能：将字符串转换成长整型数
		程序：
		/*将下列字符串分别转化为十进制、十六进制的整数*/

		#include <stdio.h>
		#include <stdlib.h>

		int main()
		{
			char a[] = "88888";
			char b[] = "face";

			printf("a = %ld\n",strtol(a,NULL,10));
			printf("b = %ld\n",strtol(b,NULL,16));
			
			return 0;	
		}


### 支付宝付款:
![](/hexo-private-blog-website/images/alipay.jpg)