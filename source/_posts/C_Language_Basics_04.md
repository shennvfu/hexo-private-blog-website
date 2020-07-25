---
title: C_Language_Basics_04
date: 2020-07-27 14:32:18
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

# C语言基础
一、函数

  1.1 怎么打印函数的地址？

    1）printf("%p\n",函数名);
    2）printf("%p\n",&函数名);

  1.2 函数的声明、定义、封装、调用

    声明：
        调用函数，需要告诉编译器这个函数的函数原型在哪里？
          一般函数的声明是声明在头文件里面。
          如果是只有一个.c，习惯的吧函数的声明声明在函数的上面

    怎么声明？
      函数由什么组成？以main例子，定义函数的返回值类型，函数名字，参数类型
      long Fun();//声明了一个函数名字位Fun，返回值类型位long的函数

      int main()       int:函数的返回值类型        0：返回值  ():存放参数
      {

        return 0;
      }

      注：返回值类型要和返回值要一致
      ():存放参数（如果有些变量不是在本函数里面定义的，需要传递给本函数，就可以通过传参的形式来实现）


      定义：  写一个空的函数

        定义Fun函数：
        long  Fun()
        {


          return 10000;
        }

        封装：实现函数的功能
        long Fun()
        {

          printf("hello world!\n");

          return 10000;
        }


        调用:让程序运行你要的函数

          函数是在函数里面被调用的。

            调用者：  main()
            被调用者：Fun();

          因为main函数是C语言程序第一个执行的函数，所以只能用main调用Fun函数


    1.3 函数的返回值怎么使用？

        注意：函数的返回值是自己自定义的。（根据你自己要实现的程序功能来确定）

          一般情况下，返回0表示函数被正常调用

        返回值什么时候会返回？
          是在函数退出的时候就会返回值，当你调用某个函数的时候，当这个函数退出时就返回返回值
        如何获取返回值？

          int a();
          int a()
          {
            printf("1\n");
            return 10;
          }

          int main()
          {
            printf("a函数的返回值为：%d\n",a());//当你的函数退出时就会返回，那么意味着你要调用执行这个函数
            //当函数退出的时候，函数调用的表达式就是返回值
            return 0;
          }


      1.4 函数的传参

          函数与函数之间传递数据的一种方法：
                          发送：A发送11的数据（int a）,实参(原来数据的位置) int data
                          接收：B接收11的数据（int b）,形参 int b

          数据怎么传递？ A---> B

          要调用函数的时候才能传递数据。
          你想传给B函数，就需要调用B函数，在A里面调用B函数

      int A()
      {
        int data = 11;
        B(data);
        return 0;
      }

      int B(int b)  //b = 11
      {

        printf("%d\n",b);

        return 0;
      }



     函数代码：
     #include <stdio.h>

	/*函数的声明*/
	long Fun_1();
	long Fun_2();

	/*函数的定义+封装*/
	long Fun_1()
	{

	  printf("hello world!\n");
	  Fun_2();
	  return 1000;
	}



	long Fun_2()
	{

	  printf("你好！\n");

	  return 0;
	}

	int main()
	{

	  Fun_1();//调用执行fun函数

	  return 0;
	}

	/*运行的行数顺序
	17

	20

	8

	11


	13

	21

	22 return 0整个程序退出
	*/

	#include <stdio.h>

	/*函数的返回值*/
	int a();
	int a()
	{
	  printf("1\n");
	  return 10;
	}

	int main()
	{
	  /*第一种获取返回值的方法*/
	  printf("a函数的返回值为：%d\n",a());//当你的函数退出时就会返回，那么意味着你要调用执行这个函数
	  //当函数退出的时候，函数调用的表达式就是返回值

	  /*第二种*/
	  /*
	  int ret;//定义一个变量用来接收返回值
	  ret = a();
	  printf("%d\n",ret);
	  */

	  int ret = a();
	  printf("%d\n",ret);
	  return 0;
	}

	/*函数的传参*/
	#include <stdio.h>

	int A();
	int B(int b,char c,char d);

	int A()
	{
	  char char_data = 'O';
	  int data = 1100000;
	  B(data,'A',char_data);
	  return 0;
	}

	int B(int b,char c,char d)  //b = 11
	{

	  printf("%d---%c---%c\n",b,c,d);

	  return 0;
	}


	int main()
	{
	  A();

	  return 0;
	}

	/*
	  23
	  25
	  7
	  9
	  10
	  14
	  17
	  19
	  11
	  25
	  27
	*/




  二、C语言程序的工程模块化

    2.1特点：

          多个文件：.c文件,.h文件

          各个文件的作用：
                  .c：定义封装、调用函数
                  .h:声明函数、宏定义、头文件包含
          文件和API都是按照项目的功能来分类

          工程例子：

                智能家居
                        .c
                  安防：摄像头监控、门禁（rfid、指纹识别、人脸识别）
                  电器控制：窗帘、空调、灯....
                  娱乐：游戏（自己写的，游戏移植）、音视频（音乐、视频），图库（电子相册）

          文件按照功能分类：

                  摄像头 ：real_time.c
                  门禁   ：  rfid.c
                  电器控制：led.c
                  游戏   ： Game.c
                  电子相册：Pic.c
                  头文件  ：smart_home.h
                  main函数：main.c

          API按照功能分类：
                 摄像头总接口 ：int Real_Time();
                 门禁总接口   : int Rfid();
                 游戏         ：int Game();
                 电子相册     ：int Pic();
----------------------------------------------------下午-----------------------------------
## 数组
二、数组（数据类型中的一种）

    一维数组：
          整形数组、字符数组、结构体数组

    数组的特点：
                1：可以存放多个数据 （在数组里面称为元素）
                2：里面存放的元素的类型是要一致
                3: 通过数组的下标来访问数组里面每一个元素，从0开始
                4：数组下标的范围从0~数组的长度-1
                5：数组里面的每一个元素都是按顺序存放的（他们的地址是连在一起的）
                6: 数组名字是数组首元素的地址（数组的地址和数组首元素的地址是一样的）
                  int_array 就是int_array[0]的地址
                  数组的地址：&int_array
                  &int_array = int_array = &int_array[0]



    数组如何使用：

        数组的定义：

                定义一个整型数组：int int_array[数组的长度];

                                 int int_array[10];
                访问第一个元素:int_array[0]; 数组里面的第一个元素

        数组的赋值：(注意：如果定义的时候没有初始化，那么再赋值的时候只能一个一个赋值,打印数据的时候也需要一个一个打印)
                1：定义之后再赋值
                    int_array[0] = 99;

                    for(int n=0; n<10; n++)
                      int_array[n] = n;

                  2:初始化

                    1）
                    int int_array[10] = {0,1,2,3,4,5,6};
                    //元素的数据可以重复，但元素的个数不可以超过数组的长度，可以小于数组的长度


                    2）
                    int int_array[] = {0,1,2,3};

                    3)吧数组里面每一个元素都清成0
                      int int_array[10] = {0};


          例子：
              自己定义一个整形数组，自己初始化一个整形数组，然后把每一个元素打印出来。


			#include <stdio.h>

			/*数组的定义  初始化*/

			int main()
			{
			  /*
			  //定义一个能够存放10个整形数据的整形数组变量
			  int int_array[10];
			  for(int n=0; n<10; n++)
			    int_array[n] = n;

			  for(int n=0; n<10; n++)
			      printf("int_array[%d] = %d\n",n,int_array[n]);


			  //初始化数组，每一个元素数据都是0

			  int int_array[10] = {0};

			  for(int n=0; n<10; n++)
			      printf("int_array[%d] = %d\n",n,int_array[n]);

			  */

			  int int_array[10] = {1,2,3,4,5,6,7,8,9};

			  for(int n=0; n<10; n++)
			      printf("int_array[%d] = %d\n",n,int_array[n]);

			  return 0;
			}


			/*打印数组每一个元素的地址*/
			#include <stdio.h>


			int main()
			{
			  int int_array[10];
			  for(int n=0; n<10; n++)
			      printf("int_array[%d]的地址为：%p\n",n,&int_array[n]);

			  printf("数组的地址:%p\n首元素的地址：%p\n,int_array=%p\n",&int_array,&int_array[0],int_array);


			  return 0;
			}


  三、数组当作函数的参数

    int Fun(int array[5]);//定义一个整形数组类型的变量来接受实参

    int Fun(int array[5])
    {

      for(int n=0; n<5; n++)
        printf("array[%d] = %d\n",n,array[n]);

      return 0;
    }


    int main()
    {
      int array[5] = {1,2,3,4,5};
      Fun(array);//数组的名字就是表示整个数组

      return 0;
    }



		/*数组传参*/

		#include <stdio.h>
		int Fun(int array[5],int data); //定义一个整形数组类型的变量来接受实参


		int Fun(int array[5],int data)
		{

		  printf("%d\n",data);
		  for(int n=0; n<5; n++)
		    printf("array[%d] = %d\n",n,array[n]);

		  return 0;
		}


		int main()
		{

		    int array[5] = {1,2,3,4,5};
		    Fun(array,array[2]);//数组的名字就是表示整个数组   ---- 传整个数组
		    
		    return 0;
		}




四、字符数组

    注：字符串最后面有一个结束标识符\0
    字符数组的定义：
            char buf[10];
    初始化：
           1）char buf_2[12] = "helloworld"   buf_2[11] = '\0';
           2）char buf_2[] = "hello world";
           3) char buf_2[] = {'h','e','l'};
           4) char buf_2[10] = '\0';

    memset()//清空数组

    void *memset(void *s, int c, size_t n);
    s:要清除的空间的首地址
    c:要填充到空间里面的数据  0
    n：要清除的空间的大小


    编码对字符串操作时，要注意有自己手动给字符串最后面补个\0.
    定义数组空间，特意多一个字节空间。


    字符数组的赋值：

            1：循环赋值
            2：使用字符串操作函数进行赋值：
                strcpy:拷贝直到遇到\0才停止拷贝
                strncpy:可以设置拷贝字符的个数

          man 3 strcpy

          头文件：#include <string.h>
          函数原型：

                char *strcpy(char *dest, const char *src);

                目标地址：你要把字符串复制到的空间的首地址   buf
                src:被拷贝的字符串的首地址        "hello world"
                strcpy(buf,"helloworld");
                strcpy(buf,buf_2);

                #include <stdio.h>
				#include <string.h>


				int main()
				{
				  //定义一个字符数组
				  char buf_1[11];

				  strcpy(buf_1,"helloworld");
				  printf("%s\n",buf_1);//一次性打印
				  memset(buf_1,0,strlen(buf_1));
				  printf("%s\n",buf_1);//一次性打印
				/*
				  for(int n=0; n<10; n++)//循环打印
				  {
				    printf("%c",buf_1[n]);
				  }

				  printf("\n");

				  printf("%s\n",buf_1);//一次性打印
				*/

				  return 0;
				}


    练习：

      编写一份C语言程序，实现生成一个随机的整形数组，然后实现从大到小排列里面的数组元素。

        1：封装一个可以生成随机数组的函数
        2：封装实现从大到小排列的函数
        3：封装一个按顺序打印数组里面每一个元素的数组




        #include <stdio.h>
		#include <stdlib.h>
		#include <time.h>
		#define M 10

		int Get_Num(int array[M]);//rand（）生成随机数 ,每次只能生成1个随机数
		int Show_Date(int array[M]);
		int Max_Min(int array[M]);

		int Get_Num(int array[M])
		{
		  srand(time(0));//设置更新随机值的间隔时间  time(0):1970到现在的秒数

		  /*
		    rand函数是根据种子值来更新随机数的，如果种子不改变，那么每次获取到的随机数都是一样的。
		    所以需要用srand函数来设置种子，srand(time(0)).time(0)就是种子值
		  */
		  for(int n=0; n<M; n++)
		    array[n] = rand()%100;//生成的随机数变成rand的返回值返回出来

		  return 0;
		}

		int Show_Date(int array[M])
		{
		    for(int n=0; n<M; n++)
		      printf("%d ",array[n]);

		    printf("\n");

		    return 0;
		}

		int Max_Min(int array[M])
		{
		   int data;
		   for(int n=0; n<M-1; n++)
		   {
		     for(int m=n+1; m<M; m++)
		     {
		       if(array[n] < array[m])
		       {
		         //互换
		         data     = array[n];
		         array[n] = array[m];
		         array[m] = data;
		       }
		       else
		       {
		         continue;
		       }
		     }
		   }
		  /*
		   n  m
		   8   9
		   8   7
		   8   11
		   8...12

		   9   7   2
		   9   11
		   9...12
		   */
		  return 0;
		}

		int main()
		{
		  //定义一个数组，用来存放随机数
		  int array[M];

		  //获取随机数
		  Get_Num(array);

		  //打印数组的元素
		  printf("改变之前：");
		  Show_Date(array);

		  Max_Min(array);

		  printf("改变之后：");
		  Show_Date(array);

		  return 0;
		}



		1：有一个数组：int array[20] = {1,0,1,0,0,1,0,1,1,1,
		                            0,1,0,1,1,0,1,0,1,0};
		数组中的元素都是由0和1组成，试着写一份程序，计算数组中0，1的个数。
		要求：不得使用任何形式的判断语句
		#include “stdio.h”

		int main()
		{
		 int num_1,num_2 = 0;
		 int array[20] = {1,0,1,0,0,1,0,1,1,1,0,1,0,1,1,0,1,0,1,0};
		 for(int n=0; n < 20; n++)
		{
		 num_1 += array[n];
		 num_2 =20- num_1;
		}
		printf(“数组中0的个数为:%d个,数组中1的个数为:%d个\n”,num1,num2);

		return 0;
		}


		2：生成一个随机数组，找出数组里面最大的数。
		#include “stdio.h”
		int Rand_Array(int array[]) //函数的声明
		int Printf_Data(int array[])
		Int Maximum(int array[])
		int main()
		{
		 int array[5];
		 Rand_Array(array);
		 Printf_Data(array);
		 Maximun(array);

		 printf("最大值为：%d\n",array[0])
		return 0;
		}
		 

		int Rand_Array(int array[])
		{ 
		  srand(time(0));
		 for(int n=0; n < 5; n++)
		     array[n] = rand()%10;
		 
		return 0;
		}

		Int Printf_Data(int array[])
		{
		 prinft(“生成的随机数组:”);
		 for(int n=0; n < 5; n++)
		  printf(“%d “,array[n]);

		printf(“\n”);

		return 0;
		}

		int Maximum(int array[])
		{
		  int num;
		for( int n=0; n < 4 ; n++)
		{
		  for(int m=n+1; n < 5; m++)
		  {
		   if(array[n] < array[m])
		   {
		    num   = array[n];
		    array[n] = array[m];
		    array[m] = num;
		   }
		   else
		{
		 continue;
		}
		 }
		}
		return 0;

		}

		3：自己查找一个memset函数的第二个形参int c，是不是可以填充任意的数据
		有些数据可以填充，有些数据不可以填充 
		memset函数的第二个形参可以按ascii表中的数据进行填充
		但是填充不同的数据会有不同的填充效果







		2：  生成一个随机数组，找出数组里面最大的数。
		程序：
		#include <stdio.h>
		#include <stdlib.h>
		#include <time.h>

		int get(int data[]);//生成随机数
		int max(int data[]);//求出最大值 返回所求最大值

		int get(int data[])
		{
			srand(time(0));
			for (int i = 0; i < 10; i++)
			{	
				data[i] = rand()%100 ;
			}	
			return 0;
		}

		int max(int data[])
		{	

			int max = data[0];

			for (int i = 0; i <10; i++)
			{
				if (data[i]>max)
				{
					max = data[i];		
				}
			}

			return max;
		}


		int main(int argc, char const *argv[])
		{
			
			int num[10] ;

			get(num);
			int M = max(num);

			printf("生成一个随机数据为：\n");
			for (int i = 0; i < 10; i++)
			{
				printf("%d ",num[i] );
				
			}
				printf("\n");
				printf("其中最大值为： %d\n", M);
			return 0;
		}




### 支付宝付款:
![](/hexo-private-blog-website/images/alipay.jpg)