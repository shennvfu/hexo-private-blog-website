---
title: Data_structure_03
date: 2020-10-14 09:21:18
comments: true
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

	桃李春风一杯酒，江湖夜雨十年灯。
	想得读书头已白，隔溪猿哭瘴溪藤。

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

# 一、作业

##### 	1）**封装实现单向链表的循环遍历、移动结点的函数。**



​		移动结点：

​			思路：

​						需要有两个目标结点，一个是要移动哪个，另外是移动到哪个结点之后。

​						下图：把p_s1移动到p_s2之后。

![1596589750474](D:\GZ2047\数据结构\003\图示\1596589750474.png)



​		移动结点函数的结构组成：

```
						 ps_1               p_s2
​	int Move_Node(头结点，要移动的结点，要移动到那个结点之后)
​	{

​		//先判断链表是否为空 在判断head是否是空（）

​		//判断两个目标是否为NULL 可以在判断两个目标结点是否在当前要操作的链表里面

​		//*下面就是移动结点的代码步骤*

​		删除p_s1(不能free)

​		指定添加（p_s1添加在p_s2之后）

​	}
```



2）指定位置添加结点：

​	思路：

```
int Add_Where_Node(head,where,add_node)//形参需要头节点，指定加入的结点位置，要添加的新的结点
{
	//判断表是否是可用的
	//两个结点是否可用
	//判断链表中是否有where结点
	/*把add_node加到where的后面*/
	add_node->next = where->next;
	where->next = add_node;
}
```



3）检索链表

​	① 根据结点检索（根据指定的结点，检索表中是否有该结点）

​	思路：

```
int Find_Node(head,find_node) //形参需要头结点，需要搜索的结点
{
	//判断表是否是可用的，表是否为空
	//搜索的结点是否可用
	
	/*下面进行检索*/
	//保证head的稳定性，定义一个临时的结构体指针
	tmp_node = head;
	while(tmp_node != NULL)
	{
		if(tmp_node == find_node)//判断链表中的每一个结点是否和检索的一致，如果一致返回0（0表示找到）		 	return 0;
		else
			 tmp_node = tmp_node->next;
	}
	//如果找不到，肯定跳到这里
	return 1;
}
```



# 二、单向循环链表

#### 1）单向循环链表的特点

​		

```
1：创建的每一个结点，需要数据域和指针域
	int data;
	struct xxx * next; （创建出来的每一个结点还没加到链表之前，他的指针域存放的是他本身）
	
2:尾结点的next存放的是头结点
	假设tmp_node是尾结点，也就是 tmp_node->next 等于 head
	

3：
	怎么创建单向循环链表的头结点
	遍历整条单向循环链表，终端条件是什么？
```

2）创建单向循环链表结点的函数

----



```
typedef struct link_list
{
	int data;
	struct xxx * next; （创建出来的每一个结点还没加到链表之前，他的指针域存放的是他本身）
}LL,*P_L;

P_L Create_Node()
{
	P_L node = (P_L)malloc(sizeof(LL));
	if(...)
	{
		...
		return NULL;
	}
	
	node->next = node;
	
	return node;
}

int main()
{
	P_L head = Create_Node();
	if(...)
	{
		...
	}
	return 0;
}
```

​	



3)单向循环链表添加结点函数（尾插法）

```
int Tail_Add_Node(P_L head)
{
	if(head == NULL)
	{
		return -1;
	}
	else
	{
		//先找尾结点
		P_L tmp_node = head;
		while(1)
		{
			if(tmp_node->next == head)
			{
				break;
			}
			tmp_node = tmp_node->next;
		}
		
		P_L add_node = Create_Node();
		if(add_node == NULL)
		{
			return -1;
		}
		printf("请输入要添加的数据：");
		scanf("%d",&(add_node->data));
		/*不需要考虑那边先连*/
		add_node->next = head;
		tmp_node->next = add_node;
	}
	
	return 0;
}

```



4）单向循环链表添加结点函数（头插法---新的结点时添加在头结点的后面）

```
思路：
	head是否可用
	
	直接指针域添加（先连右手，再连左手）
		add_node->next = head->next;
		head->next = add_node;
		
int Head_Add_Node(P_L head)
{
	if(head == NULL)
	{
		return -1;
	}
	else
	{
		P_L add_node = Create_Node();
		if(add_node == NULL)
		{
			return -1;
		}
		printf("请输入要添加的数据：");
		scanf("%d",&(add_node->data));
		/*（先连右手，再连左手）*/
		add_node->next = head->next;
		head->next = add_node;
	}
	
	return 0;
}

```



作业：

​		单向循链表中的遍历，检索（结点检索，数据检索）

​		指定位置添加和删除

	作业：
	单向循链表中的遍历，检索（结点检索，数据检索）
	指定位置添加和删除
	#include "stdio.h"
	#include "stdlib.h"
	#include "string.h"

	typedef struct link_list
	{
		int data; //数据域
		struct link_list *next; // 创建出来的每一个结点还没加到链表之前,他的指针域存放的是他本身
	}LL,*P_L;

	P_L Create_Node(); // 创建头结点
	int Tail_Add_Node(P_L head); // 尾插法
	int Head_Add_Node(P_L head); // 头插法
	int Del_Site_Node(P_L del_node); // 删除指定位置的结点
	int Display_Loop_Link_List_Node(P_L head);// 循环遍历结点中的数据
	int Move_Node(P_L head,P_L before_node, P_L after_node);// 移动结点
	int Site_Add_Node(P_L site_node, P_L add_node);// 指定位置插入结点
	int Find_Node(P_L head, P_L find_node);// 结点检索
	P_L Find_Node_Data(P_L head,int Data);// 数据检索

	P_L Create_Node()
	{
		P_L head = (P_L)malloc(sizeof(LL));
		if(head == NULL)
		{
			perror("malloc");

			return NULL;
		}

		head->next = head;

		return head;
	}

	int Head_Add_Node(P_L head)
	{
		if(head == NULL)
		{
			printf("非法操作!!\n");

			return -1;
		}
		else
		{
			// P_L tmp_node = head;
			// while(1)
			// {
			// 	if(tmp_node->next == head)
			// 	{

			// 	}

			// }
			P_L tmp_node = head;
			P_L add_node = Create_Node();
			if(add_node == NULL)
			{
				printf("添加结点失败!!\n");

				return -1;
			}

			printf("请输入要添加的数据:");
			scanf("%d", &(add_node->data));

			//先连右手再连左手
			add_node->next = tmp_node->next;
			tmp_node->next = add_node;

			return 0;
			
		}

		//return 0;
	}

	int Tail_Add_Node(P_L head)
	{
		if(head == NULL)
		{
			printf("非法操作!!\n");

			return -1;
		}
		else
		{
			//先找尾结点
			P_L tmp_node = head;
			while(1)
			{
				if(tmp_node->next == head)
				{
					break;
				}
				tmp_node = tmp_node->next;
			}

			P_L add_node = Create_Node(); // 创建要添加的新的结点
			if(add_node == NULL)
			{
				printf("添加结点失败!!\n");

				return -1;
			}

			printf("请输入要添加的数据:");
			scanf("%d", &(add_node->data));

			//add_node->next = head; //不需要考虑哪边先连
			//tmp_node->next = add_node;

			tmp_node->next = add_node; //不需要考虑哪边先连
			add_node->next = head;
		}

		return 0;
	}

	int Site_Add_Node(P_L site_node,P_L add_node)
	{
		if(site_node == NULL || add_node == NULL)
		{
			printf("非法操作,无法添加结点!\n");

			return -1;
		}

		// 先连右手,再连左手
		add_node->next = site_node->next;
		site_node->next = add_node;

		return 0;

	}

	int Del_Site_Node(P_L del_node)
	{
		if(del_node == NULL)
		{
			printf("非法操作!!\n");

			return -1;
		}

	P_L tmp_node = del_node;
	while(tmp_node->next != del_node)
	{
		tmp_node = tmp_node->next;
	}

		tmp_node->next = del_node->next;
		del_node->next = NULL;

		return 0;
	}
	/*
	int Del_Site_Node(P_L head,P_L del_node)
	{
		if(head == NULL || del_node == NULL)
		{
			printf("非法操作!!\n");

		return -1;
		}
		else if(head->next == NULL)
		{
			printf("链表为空,无法删除!!\n");

			return 0;
		}

	P_L tmp_node = head;
	while(1)
	{
		if(tmp_node->next == del_node)
			break;
		tmp_node = tmp_node->next;
	}

	tmp_node->next = del_node->next;
	del_node->next = NULL;


	return 0;

}
	*/

	int Find_Node(P_L head,P_L find_node)
	{
		if(head == NULL || find_node == NULL)
		{
			printf("非法操作\n");

			return -1;
		}
		else if(head->next == head)
		{
			printf("链表为空!!\n");

			return 1;
		}
		else
		{
			P_L tmp_node = head;
			while(tmp_node->next != find_node)
			{
				tmp_node = tmp_node->next;
				if(tmp_node == head)
				{
					printf("链表中无该结点!\n");

					return 1;
				}
			}

			return 0;
		}
	}

	P_L Find_Node_Data(P_L head,int str_data) //数据检索
	{
		if(head == NULL)
		{
			printf("非法操作!\n");

			return NULL;
		}
		else if(head->next == head)
		{
			printf("链表为空!\n");

			return NULL;
		}

		P_L tmp_node = head;
		while(tmp_node->data != str_data)
		{
			tmp_node = tmp_node->next;
			if(tmp_node == head)
			{
				printf("链表中无该数据!\n");

				return NULL;
			}
		}

		return tmp_node;
	}


	int Move_Node(P_L head,P_L before_node,P_L after_node)
	{
		if(before_node == NULL || after_node == NULL)
		{
			printf("非法操作的地址!!\n");

			return -1;
		}

		int find_1 = Find_Node(head,before_node);
		int find_2 = Find_Node(head,after_node);

		if(find_1 != 0 || find_2 != 0)
		{
			printf("没有该结点\n");

			return 1;
		}

		int ret_1 = Del_Site_Node(before_node);
		int ret_2 = Site_Add_Node(after_node,before_node);
		if(ret_1 == 0 && ret_2 == 0)
		{
			printf("移动结点成功!!\n");

			return 0;
		}

		//return 0;
	}

	int Display_Loop_Link_List_Node(P_L head)
	{
		if(head == NULL)
		{
			printf("非法操作!!\n");

		return -1;
	}
	else if(head->next == NULL)
	{
		printf("链表为空!!无法遍历!\n");

		return 1;
	}

	P_L tmp_node = head->next;
	if(tmp_node == head)
	{
		printf("该链表中无结点,无法遍历!!\n");

	// 	return 0;
	}
	while(tmp_node != head)
	{
		printf("%d ", tmp_node->data);

		tmp_node = tmp_node->next;
	}
	/*
	P_L tmp_node = head;
	if(tmp_node->next == head)
	{
		printf("该链表中无结点,无法遍历!!\n");

	// 	return 0;
	}
	while(tmp_node->next != head)
	{
		printf("%d ", tmp_node->next->data);

		tmp_node = tmp_node->next;
	}
	*/

	printf("\n");

	return 0;
}

	int main(int argc, char const *argv[])
	{
		int data;
		P_L head_ret = Create_Node();
		if(head_ret == NULL)
		{
			printf("创建头结点失败!!\n");

			return -1;
		}

	int ret = Head_Add_Node(head_ret);
	if(ret == 0)
	{
		printf("添加结点成功!!\n");
	}
	ret = Head_Add_Node(head_ret);
	if(ret == 0)
	{
		printf("添加结点成功!!\n");
	}
	ret = Head_Add_Node(head_ret);
	if(ret == 0)
	{
		printf("添加结点成功!!\n");
	}

	Display_Loop_Link_List_Node(head_ret);

	//Del_Site_Node(head_ret->next);

	Move_Node(head_ret,head_ret->next,head_ret->next->next->next);

	Display_Loop_Link_List_Node(head_ret);

	printf("请输入想在链表内检索的数据:");
	scanf("%d", &data);
	P_L find_ret = Find_Node_Data(head_ret,data);
	if(find_ret != NULL)
	{
		printf("数据已找到:%d\n", find_ret->data);
	}

	//printf("%d---%d\n", head_ret->next->data,head_ret->next->next->data);

	free(head_ret);

	return 0;
}

### 省钱利器：
![](/hexo-private-blog-website/images/淘宝客13.jpg)
![](/hexo-private-blog-website/images/淘宝客14.jpg)
![](/hexo-private-blog-website/images/淘宝客15.jpg)

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
