---
title: Data_structure_01
date: 2020-10-12 18:43:34
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

	曾经沧海难为水，除却巫山不是云。
	取次花丛懒回顾，半缘修道半缘君。

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

# 一、数据结构

---



### 1、概念

---



​			**编写关于数据结构的程序，来控制系统（CPU）来控制组织数据的（读、写、存储）的方式。**

​			**具有较好的运行，存储效率。**

### 2、数据结构基本分类

---

![](/hexo-private-blog-website/images/数据结构.png)



### 3、顺序表

---

![](/hexo-private-blog-website/images/数据结构1.png)


##### 	**1)顺序表的形状，组成的基本类型：**

---


> ```c
> 	由1个结构体组成：
>     #define DATA_NUM  20   //定义顺序表存放元素的最大个数
>     tpyedef int DATA_TYPE; //定义简单的顺序表数据与的类型
> 	typedef struct SQ_List
> 	{
> 		DATA_TYPE DATA[DATA_NUM];//数据域
> 		int num;//整形变量，实际计算元素的个数
> 	
> 	}SQ_S,*SQ_P;
> 	
> ```

​		

##### 	**2)创建一个空的顺序表**

---



​			**`第一种：直接定义结构体变量`**

​			**`第二种：使用malloc等函数给结构体指针变量申请对空间（程序退出才释放，free释放）`**

> ```c
> 
> 第一种：
>     SQ_P Create_Sq_List(SQ_P SQ_L);//传顺序表的结构体指针类型
>     SQ_P Create_Sq_List(SQ_P SQ_L)
>     {
>         //初始化数据域和计算元素个数的变量
>         memset(SQL,0,sizeof(SQ_S));
>         SQ_L->num = 0;
>         return SQ_L;
>     }
> 
> 	int main()
>     {
>         //定义一个没有初始化的顺序表
>         SQ_S SQ_L;
>         SQ_P Tmp_Sq = Create_Sq_List(&SQ_L);
>         if(Tmp_Sq == NULL)
>         {
>             printf(“初始化顺序表失败！\n”);
>             return -1;
>         }
>         
>         return 0;
>     }
> 
> 第二种：
>     SQ_P Create_Sq_List();
> 	SQ_P Create_Sq_List()
>     {
>         SQ_P SQ_L = (SQ_P)malloc(sizeof(SQ_S));
>         if(SQ_L == NULL)
>         {
>             perror("malloc :");
>             return NULL;
>         }
>         
>         memset(SQL,0,sizeof(SQ_S));
>         SQ_L->num = 0;
>         
>         return SQ_L;
>     }
> 
> 	int main()
>     {
>         SQ_P SQ_L = Create_Sq_List();
>         if(SQ_L == NULL)
>         {
>             printf("初始化创建顺序表失败！\n");
>             return -1;
>         }
>         else
>         {
>             printf("初始化创建顺序表成功！\n");
>         }
>         return 0;
>     }
> ```



​	**3)往顺序表添加数据**

---



> ```c
> 添加数据的方式
> 	头插法
> 	尾插法
> 	指定位置加入（在有效数据的长度中指定位置的）
> 	
> 	
> 	指定位置示例：
> 	
> 	int Insert_Sq_Data(SQ_P SQ_L,int where,int add_data)
> 	{
>         //表是否满了
>         if(SQ_L->num == DATA_NUM)
>         {
>             printf("表已经满了！\n");
>             return -1;
>         }
> 
>         if(where<0 || where > SQ_L->num)//指定的位置是否合法
>         {
>             printf("非法添加数据！\n");
>             return -1;	
>         }
> 
>         if(SQ_L->num == 0)//判断是不是第一次添加
>         {
>             printf("第一次添加，只能在头部添加！\n");
>             SQ_L->DATA[0] = add_data;
>         }
>         /*
>         else if(SQ_L->num == DATA_NUM-1)
>         {
>             printf("因为只剩下一个空位，只能在尾部添加！\n");
>             SQ_L->DATA[SQ_L->num] = add_data;
>         }*/
>         else
>         {
>             for(int tmp_num=SQ_L->num-1; tmp_num >= where; tmp_num--)
>                 SQ_L->DATA[tmp_num+1] = SQ_L->DATA[tmp_num];
> 
>             SQ_L->DATA[where] = add_data;
>         }
> 
>         SQ_L->num+=1;
> 
> 		return 0;	
> 	}
> ```

##### 		

##### 4）删除顺序表中的数据

---

```c
int Del_Sq_Data(SQ_P SQ_L,int where)
{
	if(SQ_L->num == 0)
	{
		printf("表空，无法删除！\n");
		return 0;
	}
	else if(where < 0 || where >= SQ_L->num)
	{
		printf("非法操作，无法删除！\n");
		return -1;
	}
	else
	{
     /*
		if(where == 0 && SQ_L->num > 1)
		{
				
			for(int tmp_num = where+1; tmp_num < SQ_L->num; tmp_num++ )
				SQ_L->DATA[tmp_num-1] = SQ_L->DATA[tmp_num];
		}
		else(where == 0 && SQ_L->num == 1)
		{
				SQ_L->num = 0;
		}
	
		else
	*/
		{
			for(int tmp_num = where; tmp_num < SQ_L->num-1; tmp_num++ )
				SQ_L->DATA[tmp_num] = SQ_L->DATA[tmp_num+1];
		}
       
		//0 1 2 4
            
		SQ_L->num--;
	}
	return 0;	
}

```

​	

​	`作业：`

​				**`实现指定数据删除顺序表中的元素`**

​					**`注意：数据是否只有一个`**


	作业：
	实现指定数据删除顺序表中的元素
	注意：数据是否只有一个
	#include "stdio.h"
	#include "string.h"
	#include "stdlib.h"
	#include "errno.h"
	#define SQ_LIST_MAX 10 // 定义顺序表存放元素的最大个数

	typedef int DATA_TYPE; // 定义简单的顺序表数据域的类型
	typedef struct SQ_List_str // 定义结构体存放到顺序表当中
	{
		DATA_TYPE SQ_List_Data[SQ_LIST_MAX]; //数据域
		int num; //整型变量,实际计算顺序表里面元素的个数
	}SQ_LI_S,*SQ_Pointer;

	int Insert_Sq_List_Data(SQ_Pointer SQ_L_PAR,int count,int add_data); //count用来记录数组下标
	int Display_Sq_List_Data(SQ_Pointer SQ_L_PAR);
	int Del_Sq_List_Data(SQ_Pointer SQ_L_PAR,int count);
	int Del_Sq_List_Equ_Data(SQ_Pointer SQ_L_PAR,int data);
	SQ_Pointer My_Sq_List();

	SQ_Pointer My_Sq_List()
	{
		SQ_Pointer SQ_L_FIR=(SQ_Pointer)malloc(sizeof(SQ_LI_S));
		if(SQ_L_FIR == NULL)
		{
			perror("malloc");

			return NULL;
		}

		memset(SQ_L_FIR,0,sizeof(SQ_LI_S));

		//SQ_L_FIR->num = 0;

		return SQ_L_FIR;

	}

	int Insert_Sq_List_Data(SQ_Pointer SQ_L_PAR,int count,int add_data)
	{
		if(SQ_L_PAR->num == SQ_LIST_MAX) // 顺序表是否满了
		{
			printf("顺序表已经满了!!\n");

			return -1;
		}

		if(count < 0 || count > SQ_L_PAR->num) //指定的位置是否合法
		{
			printf("顺序表非法添加数据\n");

			return -1;
		}

		if(SQ_L_PAR->num == 0) //判断是不是第一次添加
		{
			printf("顺序表第一次添加数据,只能在最前面位置添加!\n");
			SQ_L_PAR->SQ_List_Data[0] = add_data;
		}
		/*
		else if(SQ_Pointer->num == SQ_LIST_MAX-1)
		{
			printf("因为前面数据填满了,只剩下一个空位,只能在最后面的位置添加数据!!\n");
			SQ_Pointer->SQ_List_Data[SQ_Pointer->num] = add_data;
		}
		*/
		else
		{
			for(int tmp_num=SQ_L_PAR->num-1; tmp_num >= count; tmp_num--)
			{
				SQ_L_PAR->SQ_List_Data[tmp_num+1] = SQ_L_PAR->SQ_List_Data[tmp_num];
			}

			SQ_L_PAR->SQ_List_Data[count] = add_data;
		}

		SQ_L_PAR->num += 1; //添加了数据之后数据域长度需要加 1

		return 0;
	}

	int Del_Sq_List_Data(SQ_Pointer SQ_L_PAR,int count)
	{
		if(SQ_L_PAR->num == 0)
		{
			printf("顺序表为空,无法删除!!\n");

			return 0;
		}
		else if(count < 0 || count >= SQ_L_PAR->num) //判断count是否在有效范围数据域内
		{
			printf("非法操作顺序表,无法删除!!\n");

			return -1;
		}
		else
		{
			/*
			if(count ==0 && SQ_L_PAR->num > 1)
			{
				for(int tmp_num=count+1; tmp_num < SQ_L_PAR->num; tmp_num++)
				{
					SQ_L_PAR->SQ_List_Data[tmp_num-1] = SQ_L_PAR->SQ_List_Data[tmp_num];
				}
			}
			
			else(count == 0 && SQ_L_PAR->num == 1)
			{
				SQ_L_PAR->num = 0;
			}
			

			else

			*/
			if(SQ_L_PAR->num == 1)
			{
				SQ_L_PAR->SQ_List_Data[0] = 0;
				SQ_L_PAR->num--;
			}
			else if(count == (SQ_L_PAR->num - 1))
			{
				SQ_L_PAR->SQ_List_Data[count] = 0;
				SQ_L_PAR->num--;
			}
			else
			{
				SQ_L_PAR->num--; //表数据个数先减 1,但不影响之前的表内最后一个数据
				for(int tmp_num=count; tmp_num < SQ_L_PAR->num; tmp_num++)
				{
					SQ_L_PAR->SQ_List_Data[tmp_num] = SQ_L_PAR->SQ_List_Data[tmp_num+1];
				}
				SQ_L_PAR->SQ_List_Data[SQ_L_PAR->num] = 0; //移动完成之后把之前的表的最后一个数据清0
			}
			//SQ_L_PAR->num--; 删除数据之后数据域长度需要减 1

			/*
			{
				for(int tmp_num=count; tmp_num< SQ_L_PAR->num; tmp_num++)
				{
					SQ_L_PAR->SQ_List_Data[tmp_num] = SQ_L_PAR->SQ_List_Data[tmp_num+1];
				}

				SQ_L_PAR->num--;
			}
			*/
		}
		
		return 0;
	}

	int Del_Sq_List_Equ_Data(SQ_Pointer SQ_L_PAR,int data) //根据data删除顺序表中全部相同的数据元素
	{
		if(SQ_L_PAR->num == 0)
		{
			printf("顺序表为空,无法删除空!!\n");

			return 0;
		}
		else
		{
			int n=0;
			while(n < SQ_L_PAR-> num) //遍历查询 查完后才退出循环
			{
				if(SQ_L_PAR->SQ_List_Data[n] == data) //查找到顺序表中全部相同的数据元素
				{
					Del_Sq_List_Data(SQ_L_PAR,n); //顺序地删除相同的数据对应的全部下标
					continue; //由于删除后顺序表会向前补充填位,所以需要再对当前数据元素下标进行判断
				}
				n++;
			}
		}

		return 0;
	}

	int Display_Sq_List_Data(SQ_Pointer SQ_L_PAR)
	{
		if(SQ_L_PAR == NULL)
		{
			printf("操作错误!!\n");
			
			return -1;
		}
		else if(SQ_L_PAR->num == 0)
		{
			printf("顺序表为空!!\n");

			return 0;
		}
		else
		{
			for(int tmp_num=0; tmp_num < SQ_L_PAR->num; tmp_num++)
			{
				printf("%d ", SQ_L_PAR->SQ_List_Data[tmp_num]);
			}

			printf("\n");
		}

		return 0;
	}

	int main(int argc, char const *argv[])
	{
		SQ_LI_S SQ_L_RET = My_Sq_List();
		if(SQ_L_RET == NULL)
		{
			printf("初始化创建顺序表失败!!\n");

			return -1;
		}
		else
		{
			Insert_Sq_List_Data(SQ_L_RET,0,1);
			Insert_Sq_List_Data(SQ_L_RET,0,2);
			Insert_Sq_List_Data(SQ_L_RET,0,3);
			Insert_Sq_List_Data(SQ_L_RET,0,4);
			Insert_Sq_List_Data(SQ_L_RET,0,3);
			Insert_Sq_List_Data(SQ_L_RET,0,2);
			Insert_Sq_List_Data(SQ_L_RET,0,1);

			Del_Sq_List_Equ_Data(SQ_L_RET,3);
			Display_Sq_List_Data(SQ_L_RET);
			
		}

		return 0;
	}

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
