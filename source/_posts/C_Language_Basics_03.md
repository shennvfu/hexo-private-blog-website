---
title: C_Language_Basics_03
date: 2020-07-26 11:53:18
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

# C语言基础
  1.1 刷新输入输出缓存区的API
      fflush:
          头文件：#include <stdio.h>
          原型：
          int fflush(FILE *stream);

          int:
              返回值0成功
          stream：
                文件流指针，存指定的文件指针就可刷新对应的缓存区


        #include <stdio.h>


		int main()
		{
		  //定义一个字符变量
		  char data;
		  while(1)
		  {
		      printf("请输入一个字符");
		      scanf("%c",&data);
		      while(getchar()!= '\n');//清除缓存区

		      if(data>='A' && data<='Z') //判断是不是大写的
		      {
		          data += 32;
		          printf("输入对应的字母的小写为：%c\n",data);
		      }
		      else if(data>='a' && data<='z')
		      {
		        data -= 32;
		        printf("输入对应的字母的大写为：%c\n",data);
		      }
		      else
		      {
		        printf("输入的数据不符合，在重新输入！\n");
		        continue;//结束本次循环，进入下一次循环，一定要和循环结构搭配
		   }
		  }

		  return 0;
		}


	#include <stdio.h>
	#include <unistd.h>

	int main()
	{
  		int n=0;

  		while (1)
  	{
	    sleep(1);
	    printf("hello world!");
	    fflush(stdout);//刷新缓存区
	  }


	  return 0;
	}



    1.2 设置输入输出缓存区的大小

      1）setbuf

      头文件：#include <stdio.h>
      原型：void setbuf(FILE *stream, char *buf);

      stream：
            文件流指针，存指定的文件指针就可设置对应的缓存区

      buf：自己自定义的缓存区的首地址，根据系统（C语言标准），缓存区的大小不能小于BUFZIS(宏定义)  8192字节


      例子：自己定义一个缓存区，大小位8192，把helloworld输出到里面打印



      2)
      int setvbuf(FILE *stream, char *buf, int mode, size_t size);


      返回值：

      形参一：
          stream：文件流指针，存指定的文件指针就可设置对应的缓存区
          buf：自己自定义的缓存区的首地址，如果设置为NULL，系统自己申请缓存区
          mode：
              缓存机制：

                    行缓存：printf scanf...  （_IOLBF line buffered）
                          遇到/n符，才会对缓存区里面的数据进行输入输出操作。

                    无缓存：（  _IONBF unbuffered）标准出错perror、文件IO
                          无条件的之间对数据进行输入输出操作。

                    全缓存：（_IOFBF fully buffered）

                        等到缓存区满，才对缓存区里面的数据进行输入输出操作


    3)查找系统中BUFSIZ的大小
        第一步：
              从stdio.h里面找：
                    gedit /usr/include/stdio.h

                  找到：# define BUFSIZ         _IO_BUFSIZ
                        #define M                   10

        第二步：从#include <libio.h>里面找_IO_BUFSIZ
              libio.h：在_IO_BUFSIZ在宏定义的上面
              gedit /usr/include/libio.h
              #define _IO_BUFSIZ _G_BUFSIZ

        第三步：
            从#include <_G_config.h>里面找_G_BUFSIZ
            gedit /usr/include/_G_config.h
              #define _G_BUFSIZ 8192



          例子：
              1使用setvbuf设置标准输出缓存区为行缓存，大小为8192


              #include <stdio.h>
              #include <unistd.h>

              int main()
              {
                char buf[BUFSIZ];//定义一个数组，用来做标准输出的缓存区，大小为8192字节
                setbuf(stdout,buf); // buf 自己自定义一个缓存区的首地址


                while(1)
                {
                  sleep(1);
                  printf("hello wrld");
                }


                return 0;
              }

              #include <stdio.h>
              #include <unistd.h>


              int main()
              {
                char buf[BUFSIZ];

                setvbuf(stdout,buf,_IOLBF,50);

                fflush(stdout);
                while(1)
                {
                  sleep(1);
                  printf("hello world");
                }

                return 0;
              }


二、

  数据类型：
        1：类型与变量使用的概念：

            以整形类型为例子：
            第一种：
                  先定义变量   int data;
                  给变量赋值   data =1120;
                  使用变量     printf("%d\n",data);
            第二种：
                  初始化变量（  先定义变量，给变量赋值）    int a = 1020;
                  使用变量                                printf("%d\n",data);
            注意：定义变量的时候，最要要有给变量赋值初始值的习惯。

          2：
            数据类型转换：

              自动转换（隐式转换）：
                    1）低类型 --> 高类型

                    char  --->  short  --->int --->long --->folat --->double
                    int a = 10;
                    printf("%f\n",a);

                    2）进行混合运算时（+，-，）

                    int a = 11;
                    float b = 10.1;
                    printf("%f\n",a+b);

                    3）进行赋值运算时

                      等号的右边的数据自动转成等于号左边的类型

                      int a = 10;
                      float b = a;
                      printf("%f\n",b);

                    4）
                        函数传参的时候，实参的类型会自动转成形参的类型，（会报警高）
                        函数返回返回值的时候，返回值的类型会自动转成要求的返回值的类型，（会报警高）

                强制转换：

                  高精度--> 低精度（数据会丢失）

                  int a = 10;
                  printf("%f\n",(flaot)a);  //为了避免报警告，吧a的类型转成float的类型
======================================================================================================
一、运算符

  注：运算符的优先级（找一下对应图片）


  1.1 算数运算符   + - * / % ++ --

    例子：

        1:键盘输入一个数（不是个位数），把这个数的每一位单独打印出来 189
          9 8 1

        2:
          补充：
              字符型数据有对应的ascii表  （中文有中文编码：
                                                            windows:GB2312   一个中文2个字节
                                                            Linux ： utf-8   一个中文3个字节

                                         英文有英文编码   编码是用来设置存放对应的数据的格式大小）


            键盘输入一个字母，大写变小写，小写变大写。


  1.2 赋值运算符 （双目）  =  %= += -= /=

    例子：
              输入一个数，计算这个数+到10的总和。

  1.3 关系运算符  == != > < >= <=


  1.4 逻辑运算符 （真和假）

    条件判断1  &&   条件判断2

    if(1 && 1)
    {
      printf("打印hello\n");
    }
    if(100)
    {
      printf("打印hello\n");
    }

    int a = 1;
    if(!a)
    {
      printf("把helloworld\n");
    }

    条件判断1 ||  条件判断2
    if(1 || 0) 真
    if(0 || 0) 假
    if(printf("hello") || printf("world")); //判断的是函数的返回值

      !:逻辑非 （取反）

  1.5 位运算符 <<  >>  ^  ~ & |

    int 4;
    二进制：100 << 2   ----》  10000   //左移是扩大2的n次方倍
           100 >> 2   ---->   001      //右移是缩小2的n次方倍

    异或运算符^:
        int a = 8;   1000
        int b = 3;   0011
        int c = b^a; 1011 ---> 11

    ~:二进制补码运算符

      int a = 7;
      printf("%d\n",(~a));

      0.... 0111
      1.... 1000  -8

      ^=   a^=b;  --->  a = a^b;
      ~=
      &=
      |=

  1.6 杂项运算符

      sizeof、*、&、 ？  ：（三目运算符）

      sizeof()
      关于系统版本：

            12.04： int  printf("%d");
            16.06、18.04： long unsigned int   printf("%ld\n")

      查看编译器gcc :gcc -v

      *:解引用  （指针变量）  地址是连续存储的 每个连续的地址之间相差1

      &：当你./test运行程序时,代码里面的所有变量以及代码段都会加载到系统的运行内存里面。


      条件语句？执行1  ：执行2   （条件真执行1   条件假执行2）

      int a = 2;
      int b = 3;

      int d = a>b?a:b; //d等于3
      a>b?printf("a>b"):printf("a<b");


      #include <stdio.h>

      int main()
      {
        int num;
        int new_data;//存放取出来的单个数据
        printf("请输入一个数字：");
        scanf("%d",&num);

        //拆分
        //判断键盘输入的数是否是个位数

        //198
        while(1)
        {
          if(num / 10 != 0)
          {
              new_data = num%10;
              num /= 10;
              printf("%d\n",new_data);
          }
          else
          {
            printf("%d\n",num);
            break;
          }

        }


      //字符和整形数据互换

        int a = 65;
        char b = 'R';
        printf("%c\n",a);
        printf("%d\n",b);  //

        return 0;
      }


      /*数据类型自动转换 ---  低-->高*/

      #include <stdio.h>

      int main()
      {
        /*int a = 10;
        printf("%f\n",a);
      */

      /*混合运算的自动转换*/
        int a = 11;
        float b = 10.1;
        printf("%f\n",a+b);
        return 0;
      }


      #include <stdio.h>

      int main()
      {
        int a = 10;
        float b = (float)a;
        printf("%f\n",(float)a);  //为了避免报警告，把a的类型转成float的类型
        return 0;
      }


      /*赋值运算符 +=*/

      #include <stdio.h>
      #define M 10

      int main()
      {

        int data;
        printf("请输入一个数：");
        scanf("%d",&data);
        int sum=data;

        if(data<10)
        {
            for(;data<M ;data++)
            {
              //循环要执行的语句
              sum += (data+1);//sun = sum+(data+1)
            }
            printf("这个叠加到10的总和是%d\n",sum);
            /*
            for(1;2;3)
            {
              4
            }
            第一次循环 1 2(成立) 4 3
            第二次循环   2（成立）4 3
            */
        }
        else
        {
          printf("输入有误！退出程序！\n");
        }


        return 0;
      }

      #include <stdio.h>
      int main()
      {
        /*
        int a= 4;
        int b = a<<2;
        int c = a>>2;
        printf("%d-----%d\n",b,c);
        */

      /*
        int a = 8;   //1000
        int b = 3;   //0011
        int c = b^a; //1011 ---> 11

        printf("%d\n",c);
        */

        int a = 7;
        printf("%d\n",(~a));



        return 0;
      }


二、控制流语句

  九个：
      if

      for

      while()

      do
      {

      }while(1) //至少会执行一次

      例子：写一份do whille的示例代码

      switch(数)
      {
        case 1 :执行语句
        case 2 :
        。。。。
        default :

      }

      break:一定要和循环体结合在一起，退出当前的循环体

        while(1)
        {
          for(; ; )
          {
            break;  //至跳出for
          }
          break;
        }

      continue:取消本次循环，进入下一次循环

      while(1)
      {
        continue;
        printf("hello world!\n");
      }

      return  :无条件退出当前的函数

      int main()
      {

        return 0;
        printf("hello world!\n");
      }

      goto:
          要有条件的执行goto ()
          要退出goto的时候

      用法：
        int a = 4;

      
        loop:

            printf("hello world\n");

        goto loop;
        

        goto用法:

        #include <stdio.h>

        int main()
        {
          int a = 10;

          if(a != 10)
          {
            mask:
              printf("%d\n",a);
              return 0;
          }

          loop:
            a++;
            if(a == 20)
              goto mask;
          goto loop;


          return 0;
        }


      #include <stdio.h>

      int main()
      {
          int a = 11;

          while(1)
          {
            printf("输入一个数");
            scanf("%d",&a);
            while(getchar()!='\n');
            switch (a)
            {
              case 10: a++;break;
              case 11: a--;
              case 12: a=11;break;
              default: printf("其他情况\n");break;
            }
            printf("a = %d\n",a);//-----11
            //如果符合条件的那个case没有break,那么会按顺序执行下面case,直到遇到break
          }

        return 0;
      }


      #include <stdio.h>


      int main()
      {
        int a = 4;

        //打印a的地址
        printf("%p\n",&a);
        //0x7ffe43cc7764

        char b = 'S';
        printf("%p\n",b);

        return 0;
      }


      #include <stdio.h>

      int main()
      {

              int a = 2;
              int b = 3;

              int d = a>b?a:b; //d等于3
              a>b?printf("a>b"):printf("a<b");

              return 0;

      }

  练习：

      键盘输入一个数（能持续输入），判断这个数是否能被5整除，如果不能把这个数递增知道变成能被5整除的数。


      #include <stdio.h>

      int main()
      {
        //什么数就可以被5整除 个位数时0或者5的数  11 -->15  17 -->20  14 -->15
        int data;
        while(1)
        {
          printf("请输入一个数");
          scanf("%d",&data);
          while(getchar()!='\n');
          if(data %5 == 0)
          {
            printf("%d能被5整除\n",data);
          }
          else
          {
            printf("%d不能被5整除，需要加%d才可以被整除\n",data,5-(data%5));
          }
        }

        return 0;
      }

      一、单项选择题

      1、表达式 1? (0?3: 2): (10?1: 0)  的值是（A）。

      A)2     B)  3      C)   1     D） 0

      2、以下代码执行完毕后，i的值为（A）。

      A)0      B)  11      C)10     D)出错

      3、定义int a=3,b=6;，则表达式 int c = (a ^ b)<<2中,c的二进制值是（D）。

      A)00011100    B)00000111    C）00000001   D）00010100

        4、在while(!a)中，其中!a与表达式（A）等价。
      A）a==0      B）a==1       C）a!=1       D）a!=0

      5、int a,i=2;  if(a=i++ != 4),该判断语句结果为（A）

      A）真     B）假   C)语法有误  D）其他
        
      编程题：

      1、编写一个程序，此程序要求输入天数，然后将该值转换为星期数和天数。例如：
      输入：18天
      输出：2星期 + 4天
      #include <stdio.h>

      int main(void)
      {
        int day;
        int week;
        int data;
        while(1)
      { 
        printf(“请输入天数:”);
        scanf(“%d”,&day);
        week = day/7;
        data = day%7;
        printf(“%d星期+%d天\n”,week,data);
        break;
      }
      return 0;
      }
        


      2、从键盘输入10个数，统计非负数的个数，并计算非负数的和。
       #include <stdio.h>
       int main(void)
       {
        int num[10];
        int sum=0;
        int a=0;
        while(1)
      {
      printf(“请输入10个数:\n”);
      for(int i=0;i<10;i++)
      {
        scanf(“%d”,&num[i]);
        while(getchar() != ‘\n’);
      if(num[i] >=0)
      {
        a++;
        sum += num[i];
      }
          }
      prinf(“非负数的个数为%d,非负数的和为%d\n”,a,sum);

      return 0;
      }

      计算22左移3位的最后结果
      22(D)=10110(B)  <<3----->10110000(B) = 176(D)


### 自动类型转换
![](/hexo-private-blog-website/images/自动类型转换.png)

### 支付宝付款:
![](/hexo-private-blog-website/images/alipay.jpg)