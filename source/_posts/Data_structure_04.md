---
title: Data_structure_04
date: 2020-10-15 17:21:18
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

	古人学问无遗力，少壮工夫老始成。
	纸上得来终觉浅，绝知此事要躬行。

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


# 一、单向循环链表



1、删除结点（可以独立封装的删除结点方式）

​
![](/hexo-private-blog-website/images/删除特殊结点.bmp)


```
①、
	删除尾结点(需要遍历)
	
②、
	删除头节点后面的结点（第二个结点）

③、 删除第一个结点（删除head结点--需要遍历）
	3、删除head
	tmp_ndeo = head;//A
	frist = head;
   
    
    while(1)
    {
    	if(tmp_node->next == head) //这时候tmp_node 等于 C
    		break;
    	tmp_node = tmp_node->next;
    }
    
    head = head->next ---(A)
    C->next = head;
    free(frist);
    考虑：在没有指针域操作的时候，那些表示方式是head
    	head 
    	C->next
    	
  ④：链表的销毁
  	删除情况考虑：
  			全部连同head一起删掉（free）
  			剩下head
  	
  	1：先删掉，在free -- (把循环遍历到的结点，传递给指定位置删除结点的函数)
  	2: 直接free
  	
  	循环完，在单独free(head)
  	
  	int Destory_List(head)
  	{
  		if(head == NULL)
  		{
  			printf("链表头节点为NULL！\n");
  			retrurn -1;
  		}
  		free_node = head->next;
  	
        while(1)
        {
            tmp_node = free_node->next; 
            free(free_node);//
            free_node = tmp_node;// 3
            if(free_node == head)
            {
                break;
            }
        }
        free(head);
  		
  	}
  	
  	
  	
  	思路：
  		先备份第二个结点（head->next）
  		进入循环，先备份要删除的结点的下一个
  		free
  		把之前备份的赋值给要删除的变量（把之前备份的结点变成现在要删除的结点）
  		备份的结点：tmp_node
  		当前要删除的结点：free_node
  		
  	考虑：
  		如果不删head的情况下，可以把删除剩下的head返回出来，判断和实参的head是否一样
  		
  		2:关于free
  			free(head):
  				本以为free之后，head是变成NULL  XXXX
  				本以为free之后，head里面数据域清空
  				
  			把你传进来的地址head指向的空间（标记）不属于你这份程序中的堆空间。free是不对空间里面的数			据进行操作的。
  			
 		3:关于销毁链表：
 			以后不再用这条链表了，（连head都释放了），但是你还可以通过结点访问已经free的空间。
 			
 			确定不再用了，就要避免第二次误用。（确保没有变量保存他的地址）
 			
 				思路：
 					确保每一个结点存放的指针域为NULL；
 						先删掉，在free
 			比如：
            	int * Fun(int ** a) //a存放的是p的地址
            	{
            		*a = NULL；//*a得到了p存放的地址
            		return a;
            	}
            	
            	int main()
            	{
            		int * p = (int *)malloc(sizeof(int))；
            		
            		 Fun(&p);
            		 printf("%d\n",*p);//段错误
            		return 0;
            	}
 			
 				想通过Fun函数让main函数里面的p存放NULL地址：
 					第一种：让Fun返回NULL赋值给p
 					第二种：改变Fun函数的形参类型为二级指针类型
 					一级指针变量存放的是数据地址
 					二级指针变量存放的是一级指针变量的地址
 					
                
 		4：
 			剩下：

            ​		双向

            ​		内核

            ​		链式队列

            ​		链式栈

            ​		二叉树

```



# 二、双向链表

---

##### 1）双向不循环链表的特点

```
	每一个结点组成
	typedef struct double_list
	{
		int data;//数据域
		//指针域
		前驱：存放上一个结点
		后继：存放下一个结点
		struct xxx * next,*prev;
	} DL,*P_DL;
```

---



##### 2)创建双向链表的结点（第一次创建时头节点）

```
	注意：
		不循环：指针域指向空（NULL）
		循环：指针域指向自己
		
	P_DL Create_Node();
	P_DL Create_Node()
	{
		P_DL node = (P_DL)malloc(sizeof(DL));
		if(node == NULL)
		{
			perror("malloc:");
			return NULL;
		}
		/*指针域操作*/
		node->next = NULL;
		node->prev = NULL;
		
		return node;
	}
	
	int main()
	{
		P_DL head = Create_Node();
		if(head == NULL)
		{
			printf("创建链表头失败！\n");
			return -1;
		}
		else
		{
			printf("创建链表头成功！\n");
		}
	
		return 0;
	}
```

---

##### 3）双向不循环链表添加结点

​		三种方式添加：

​					头插法：（添加在头节点的后面）			

```
注：
	需要判断是否是第一次添加结点： head->next 是否等于 NULL

int Head_List_Node(P_DL head)
{
	if(head == NULL)
	{
		printf("头节点为NULL！\n");
		return -1;
	}
	else
	{
		P_DL add_node = Create_Node();
		if(add_node == NULL)
		{
			printf("add_node为NULL！\n");
			return -1;
		}
		printf("请输入要添加的数据：");
		scanf("%d",&(add_node->data));
		
		if(head->next == NULL)//判断是否是第一次添加
		{
			head->next     = add_node;
			add_node->prev = head;
		}
		else
		{
			//右手
			add_node->next   = head->next;
			head->next->prev = add_node;
			//左手
			head->next     = add_node;
			add_node->prev = head;
		}
	}
	
	return 0;
}
```

​	尾插法：(练习)

```
int Tail_Add_Node(P_DL head)
{
	if(head == NULL)
	if(...)
	{
		...
	}
	else
	{
		找到最后一个结点
		tmp_node = head;
		while(tmp_node ->next != NULL)
		{
			tmp_node = tmp_node->next;
		}
		
		P_DL add_node = Create_Node();
		if(...)
		{
			...
		}
		tmp_node->next = add_node;
		add_node->prev = tmp_node;
	}
	return 0;
}



```







指定位置添加：(指定的结点后面添加)+ 检索

---

```
int Find_Node(P_DL head,P_DL find_node);
int Find_Node(P_DL head,P_DL find_node)
{
	if(head == NULL)
	{
		printf("head为NULL！\n");
		return -1;
	}
	else if(head->next == NULL)   
	{
		printf("表空！\n");
		return 0;
	}
	else
	{
		P_DL tmp_node = head;
		while(1)
		{
			if(tmp_node == find_node || tmp_node == NULL) break;
			tmp_node = tmp_node->next;
		
		}
		
		if(tmp_node == NULL)
		{
			printf("没有该结点！\n");
			return 2;//2表示链表中没有此结点
		}
		else
		{
			printf("已找到该结点！\n");
			return 0;
		}
	}
}

int Add_where_Node(P_DL head,P_DL where)
{
	if(head == NULL || where == NULL)
	{
		printf("head或者where为NULL！\n");
		return -1;
	}
	if(head->next == NULL || where->next == NULL)
	{
		printf("只有一个头结点或者where是尾结点，无法添加\n");
		return 0;
	}
	//检索链表中是否有where结点
	int ret = Find_Node(head,where);
	if(ret == 0)
	{
		printf("可以添加！\n");
		P_DL add_node = Create_Node();
		printf("请输入要添加的数据：");
		scanf("%d",&(add_node->data));
		
		add_node->next    = where->next;
		where->next->prev = add_node;
		add_node->prev    = where;
		where->next       = add_node;
		return 0;
	}
	
	return -1;
}





```



---

4)遍历双向链表结点

```
int Display_List_Data(P_DL head)
{
	if(head == NULL)
	{
		printf("链表头为NULL\n");
		return -1;
	}
	else if(head->next == NULL)
	{
		printf("链表为空！\n");
	}
	else
	{
		P_DL tmp_node = head->next;
		while(tmp_node != NULL)
		{	
			printf("%d\n",tmp_node->data);
			tmp_node = tmp_node->next;
		}
	}
	return 0;
}

```



##### 5)指定位置删除结点

```
注意：
	不需要遍历找删除结点的上一个（因为有prev）
	
	int Del_Where_Node(P_DL head, P_DL del_node)
{
	//判断head是否为NULL ，链表空不空，where有没有在链表里面
	if(head == NULL)
	{
		printf("链表头为NULL\n");
		return -1;
	}
	else if(head->next == NULL)
	{
		printf("链表为空！\n");
	}
	else if(Find_Node(head,del_node))
	{
		printf("表中没有这个结点！\n");	
	}
	else
	{
		
		if(head->next == del_node && del_node->next == NULL)//是否链表中只有一个数据结点，只能是除了head之外只有一个结点
		{
			head->next = NULL;
			del_node->prev = NULL;
		}
		else if(del_node->next == NULL)
		{
			del_node->prev->next = NULL;
			del_node->prev = NULL;
		}
		else//结点数大于2
		{
			del_node->next->prev = del_node->prev;
			del_node->prev->next = del_node->next;
			
			del_node->next = NULL;
			del_node->prev = NULL;	
		}
		free(del_node);
		return 0;
	}
		
	
}
	
```



​	作业：

​		使用二级指针变量，完善你的单向链表的销毁

​		完成双向链表的销毁

	作业：
	使用二级指针变量，完善你的单向链表的销毁
	完成双向链表的销毁
	#include "stdio.h"
	#include "stdlib.h"
	#include "string.h"

	typedef struct link_list
	{
		int data; //数据域
		struct link_list *next; // 创建出来的每一个结点还没加到链表之前,他的指针域存放的是他本身
	}LL,*P_L;

	P_L Create_Node(); // 创建头结点
	int Tail_Add_Node(P_L head); //尾插法
	int Head_Add_Node(P_L head); // 头插法
	int Del_Site_Node(P_L del_node); // 删除指定位置的结点
	int Display_Loop_Link_List_Node(P_L head); //循环遍历结点内的数据
	int Move_Node(P_L head,P_L before_node, P_L after_node); // 移动结点
	int Site_Add_Node(P_L site_node, P_L add_node); // 指定位置插入
	int Find_Node(P_L head, P_L find_node); // 结点检索
	P_L Find_Node_Data(P_L head,int Data); //数据检索
	//int Destroy_Link_List_Node(P_L head); // 销毁链表
	int Destroy_Link_List_Node(P_L *head);

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

	int Destroy_Link_List_Node(P_L *head)
	{
		if((*head) == NULL)
		{
			printf("链表头结点的地址为空!!\n");

			return -1;
		}
		else if((*head)->next == *head)
		{
			free(*head);
			// *head=NULL;
			//printf("链表销毁成功!!\n");
			return 0;
		}

		P_L *tmp_node = NULL;
		P_L *free_node = &((*head)->next);

		while((*free_node) != (*head))
		{
			tmp_node = &((*free_node)->next);
			//tmp_node = (*free_node)->next;
			free(*free_node);
			//*free_node = tmp_node;
			free_node = tmp_node;
		}
		free(*head);

		return 0;
		//*head = NULL;
		//printf("全部销毁成功!\n");
	}
	// int Destroy_Link_List_Node(P_L head)
	// {
	// 	if(head == NULL)
	// 	{
	// 		printf("链表头结点地址为空!!\n");

	// 		return -1;
	// 	}
	// 	else if(head->next == head)
	// 	{
	// 		free(head);

	// 		return 0;
	// 	}

	// 	P_L tmp_node = NULL;
	// 	P_L free_node = head->next;

	// 	while(free_node != head)
	// 	{
	// 		tmp_node = free_node->next;
	// 		free(free_node);
	// 		free_node = tmp_node;
	// 		// if(free_node == head)
	// 		// {
	// 		// 	break;
	// 		// }
	// 	}

	// 	/*
	// 	while(1)
	// 	{
	// 		tmp_node = free_node->next;
	// 		free(free_node);
	// 		free_node = tmp_node;
	// 		if(free_node == head)
	// 		{
	// 			break;
	// 		}
	// 	}
	// 	*/

	// 	free(head);

	 	//return 0;
	// }


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

		P_L tmp_node = head;
		if(tmp_node->next == head)
		{
			printf("该链表中无结点,无法遍历!!\n");

		// 	return 0;
		}

		while(1)
		{
			//printf("%d ", tmp_node->data);
			if(tmp_node->next == head)
				break;
			tmp_node = tmp_node->next;

			printf("%d ", tmp_node->data);
		}
		// while(tmp_node != head)
		// {
		// 	printf("%d ", tmp_node->data);

		// 	tmp_node = tmp_node->next;
		// }
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

		ret = Destroy_Link_List_Node(&head_ret);
		if(ret == 0)
		{
			printf("链表销毁成功!!\n");

		}
		else
		{
			printf("链表销毁失败!!\n");
		}

		Display_Loop_Link_List_Node(head_ret);

		printf("%p---%p\n", head_ret,&head_ret);
		printf("%p---%p\n", head_ret+1,&head_ret+1);
		printf("%p---%p\n", head_ret+2,&head_ret+2);

		//printf("%d---%d\n", head_ret->next->data,head_ret->next->next->data);

		//free(head_ret);

		return 0;
	}





	#include "stdio.h"
	#include "stdlib.h"
	#include "errno.h"

	typedef struct Double_Link_List
	{
		int data; //数据域
		struct Double_Link_List *next,*prev; // 指针域 前驱:存放上一个结点  后继 存放下一个结点
	}D_LL,*P_D_LL;

	P_D_LL Create_Node();
	int Head_Link_List_Node(P_D_LL head);
	int Tail_Link_List_Node(P_D_LL head);
	int Site_Add_Link_List_Node(P_D_LL head,P_D_LL site);
	int Del_Site_Link_List_Node(P_D_LL head,P_D_LL del_node);
	int Move_Node(P_D_LL head,P_D_LL before_node,P_D_LL after_node);
	int Display_Link_List_Data(P_D_LL head);
	int Find_Link_List_Node(P_D_LL head, P_D_LL find_node);
	int Destroy_Link_List_Node(P_D_LL *head);

	P_D_LL Create_Node()
	{
		P_D_LL head = (P_D_LL)malloc(sizeof(D_LL));
		if(head == NULL)
		{
			perror("malloc:");

			return NULL;
		}

		head->next = NULL;
		head->prev = NULL;

		return head;
	}

	int Head_Link_List_Node(P_D_LL head)
	{
		if(head == NULL)
		{
			printf("头结点为空地址!!\n");

			return -1;
		}
		else
		{
			P_D_LL add_node = Create_Node(); //创建要添加的结点
			if(add_node == NULL)
			{
				printf("新添加的结点地址为空!!\n");

				return -1;
			}
			printf("请输入要添加的数据:");
			scanf("%d", &(add_node->data));

			if(head->next == NULL) //判断是否是第一次添加
			{
				head->next = add_node; //这里是第一次添加的时候
				add_node->prev = head;
			}
			else
			{
				// 右手
				add_node->next = head->next; //这里是添加多次的时候
				head->next->prev = add_node;
				//左手
				head->next = add_node;
				add_node->prev = head;
			}
		}

		return 0;
	}

	int Tail_Link_List_Node(P_D_LL head)
	{
		if(head == NULL)
		{
			printf("头结点为空地址!!\n");

			return -1;
		}

		// 找到最后一个结点
		P_D_LL tmp_node = head;
		while(1)
		{
			if(tmp_node->next == NULL)
				break;
			tmp_node = tmp_node->next;
		}

		// P_D_LL tmp_node = head;
		// while(tmp_node->next != NULL)
		// {
		// 	//if(tmp_node->next == NULL)
		// 		//break;
		// 	tmp_node = tmp_node->next;
		// }

		P_D_LL add_node = Create_Node();
		if(add_node == NULL)
		{
			printf("新添加的结点地址为空!!\n");

			return -1;
		}
		printf("请输入要添加的数据:");
		scanf("%d", &(add_node->data));

		tmp_node->next = add_node; //只需要考虑左手
		add_node->prev = tmp_node;

		return 0;
	}

	int Find_Link_List_Node(P_D_LL head,P_D_LL find_node)
	{
		if(head == NULL || find_node == NULL)
		{
			printf("头结点为空地址或者要检索的结点为空地址!!\n");

			return -1;
		}
		else if(head->next == NULL)
		{
			printf("链表为空!!\n");

			return 0;
		}
		else
		{
			P_D_LL tmp_node = head;

			while(tmp_node != NULL)
			{
				if(tmp_node == find_node)
					return 0;
				else tmp_node = tmp_node->next;
			}
			return 1; //1 表示链表中没有此结点

			/*
			while(1)
			{
				if(tmp_node == find_node || tmp_node == NULL)
					break;
				tmp_node = tmp_node->next;
			}
			// while(tmp_node != NULL)
			// {
			// 	if(tmp_node == find_node)
			// 		return 0;
			// 	else tmp_node = tmp_node->next;
			// }
			// return 1; //1 表示链表中没有此结点
			if(tmp_node == NULL)
			{
				printf("没有该结点!!\n");

				return 2; // 2表示链表中没有此结点
			}
			else
			{
				printf("已找到该结点!!\n");

				return 0;
			}
			*/
		}
	}

	int Site_Add_Link_List_Node(P_D_LL head, P_D_LL site)
	{
		if(head == NULL || site == NULL)
		{
			printf("头结点为空地址!!或者要插入的结点位置为空地址!!\n");

			return -1;
		}
		else if(head->next == NULL || site->next == NULL)
		{
			printf("只有一个头结点或者要插入的结点位置是尾结点!!无法添加!\n");

			return 0;
		}

		/*
		//检索链表中是否有要插入的位置的结点
		int ret = Find_Link_List_Node(head,site);
		if(ret == 0)
		{
			printf("可以添加!\n");
			P_D_LL add_node = Create_Node();
			if(add_node == NULL)
			{
				printf("新添加的结点地址为空!!\n");

				return -1;
			}

			printf("请输入要添加的数据:");
			scanf("%d\n", &(add_node->data));

			add_node->next = site->next; //先连右手
			sit->next->prev = add_node;

			add_node->prev = site;
			site->next = add_node;

			return 0;
		}

		return -1;

		*/

		
		//检索链表中是否有要插入的位置的结点
		int ret = Find_Link_List_Node(head,site);
		if(ret != 0)
		{
			printf("检索不到该结点,无法添加\n");

			return -1;
		}

		P_D_LL add_node = Create_Node();
		if(add_node == NULL)
		{
			printf("新添加的结点地址为空!!\n");

			return -1;
		}
		printf("请输入要添加的数据:");
		scanf("%d", &(add_node->data));

		add_node->next = site->next; //先连右手
		site->next->prev = add_node;

		add_node->prev = site;
		site->next = add_node;


		return 0;
		
	}

	int Del_Site_Link_List_Node(P_D_LL head,P_D_LL del_node)
	{
		//判断head是否为NULL,链表空不空,要删除的结点是否在链表里面
		if(head == NULL || del_node == NULL)
		{
			printf("头结点为空地址!!或者要删除的结点为空地址\n");

			return -1;
		}
		else if(head->next == NULL)
		{
			printf("链表为空!!无法删除!!\n");

			return 1;
		}
		else if(Find_Link_List_Node(head,del_node)) //如果返回 0 则不执行该语句
		{
			printf("链表中没有该结点!!\n");
		}
		else
		{
			if(head->next == del_node && del_node->next == NULL) //是否链表中只有一个数据结点,只能是除了head之外只有一个结点  要删除的结点为头结点时
			{
				head->next = NULL;
				del_node->prev = NULL;
			}
			else if(del_node->next == NULL) //要删除的结点为尾结点时
			{
				del_node->prev->next = NULL;
				del_node->prev = NULL;
			}
			else
			{
				del_node->next->prev = del_node->prev; //先连右手
				del_node->prev->next = del_node->next;

				del_node->next = NULL;
				del_node->prev = NULL;
			}

			free(del_node);

			return 0;
		}
	}

	int Move_Node(P_D_LL head,P_D_LL before_node,P_D_LL after_node)
	{
		if(head == NULL || before_node == NULL || after_node == NULL)
		{
			printf("头结点为空地址或者要移动的结点为空地址!!\n");

			return -1;
		}
		else if(head->next == NULL)
		{
			printf("链表为空!!无法移动结点!!\n");

			return 1;
		}
		else
		{
			int find_1 = Find_Link_List_Node(head,before_node);
			int find_2 = Find_Link_List_Node(head,after_node);
			if(find_1 !=0 || find_2 != 0)
			{
				printf("链表没有找到该结点\n");

				return 2;
			}

			int ret_1 = Del_Site_Link_List_Node(head,before_node);
			int ret_2 = Site_Add_Link_List_Node(head,after_node);
			if(ret_1 == 0 && ret_2 == 0)
			{
				printf("移动结点成功!\n");

				return 0;
			}
		}
	}

	int Destroy_Link_List_Node(P_D_LL *head)
	{
		if((*head) == NULL)
		{
			printf("头结点为空地址!!\n");

			return -1;
		}
		else if((*head)->next == NULL)
		{
			free(*head);
			//*head=NULL;
			//printf("销毁成功!!\n");

			return 0;
		}

		P_D_LL *tmp_node = NULL;
		P_D_LL *free_node = &((*head)->next);

		while((*free_node) != NULL)
		{
			tmp_node = &((*free_node)->next);
			//tmp_node = (*free_node)->next;
			free(*free_node);
			//*free_node = tmp_node;
			//*free_node = NULL
			free_node = tmp_node;
		}

		free(*head);
		//*head = NULL;
		//printf("全部销毁成功\n");

		return 0;
	}

	int Display_Link_List_Data(P_D_LL head)
	{
		if(head == NULL)
		{
			printf("头结点的地址为空!!无法遍历!\n");

			return -1;
		}
		else if(head->next == NULL)
		{
			printf("链表为空!!无法遍历!!\n");

			return 0;
		}

		P_D_LL tmp_node = head;
		while(tmp_node->next != NULL)
		{
			printf("%d ", tmp_node->next->data);
			tmp_node = tmp_node->next;
			//printf("%d ", tmp_node->next->data); //这样写会段错误 如果指向的是尾结点那么打印的就是尾结点的下一个结点的数据了
		}

		printf("\n");

		return 0;
	}

	int main(int argc, char const *argv[])
	{
		P_D_LL head_ret = Create_Node();
		if(head_ret == NULL)
		{
			printf("创建链表头失败!!\n");

			return -1;
		}
		else
		{
			printf("创建链表头成功!!\n");
		}

		int ret = Tail_Link_List_Node(head_ret);
		if(ret != 0)
		{
			printf("添加结点失败!!\n");

			return -1;
		}
		else
		{
			printf("添加结点成功!!\n");
		}

		ret = Tail_Link_List_Node(head_ret);
		if(ret != 0)
		{
			printf("添加结点失败!!\n");

			return -1;
		}
		else
		{
			printf("添加结点成功!!\n");
		}

		ret = Tail_Link_List_Node(head_ret);
		if(ret != 0)
		{
			printf("添加结点失败!!\n");

			return -1;
		}
		else
		{
			printf("添加结点成功!!\n");
		}

		ret = Display_Link_List_Data(head_ret);
		if(ret != 0)
		{
			printf("打印数据失败!\n");

			return -1;
		}

		Move_Node(head_ret,head_ret->next,head_ret->next->next);

		Display_Link_List_Data(head_ret);

		ret = Destroy_Link_List_Node(&head_ret);
		if(ret == 0)
		{
			printf("链表销毁成功!!\n");
		}
		else
		{
			printf("链表销毁失败!!\n");
		}


		//Site_Add_Link_List_Node(head_ret,head_ret);

		//Display_Link_List_Data(head_ret);

		// ret = Del_Site_Link_List_Node(head_ret,head_ret->next);
		// if(ret == 0)
		// {
		// 	printf("删除结点成功!!\n");
		// }

		//Move_Node(head_ret,head_ret->next,head_ret->next->next);

		Display_Link_List_Data(head_ret);

		printf("%p------%p\n", head_ret,&head_ret);
		printf("%p------%p\n", head_ret+1,&head_ret+1);
		printf("%p------%p\n", head_ret+2,&head_ret+2);
		printf("%p------%p\n", head_ret+3,&head_ret+3);

		//printf("%d\n", head->next->data);

		free(head_ret);


		return 0;
	}

### 省钱利器：
![](/hexo-private-blog-website/images/淘宝客15.jpg)
![](/hexo-private-blog-website/images/淘宝客16.jpg)
![](/hexo-private-blog-website/images/淘宝客17.jpg)

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