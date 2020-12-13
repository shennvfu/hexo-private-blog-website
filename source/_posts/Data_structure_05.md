---
title: Data_structure_05
date: 2020-10-16 14:31:18
comments: true
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

	应怜屐齿印苍苔，小扣柴扉久不开。
	春色满园关不住，一枝红杏出墙来。

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

---



​	1：使用二级指针变量完善双向链表的销毁

​		

```
定义一个接口用来销毁：  
	int Destory_List(P_DL * head)//这里的形参接受的是最原始存放头结点的指针变量的地址,这里的head存放的是main里面head的地址
	{
		//头结点是否为NULL
		if(*head == NULL)//这里的head是上面的形参，不是main里面的head
		{
			printf("头结点为NULL！\n");
			return -1;
		}
		else
		{
			P_DL * del_node = &((*head)->next);
			P_DL * tmp_node = NULL;//这指针变量是在free del_node的时候先保存del_node的下一个
			
			//你是想先销毁头后面的结点，定义一个临时的变量存放链表的第二个结点，这个变量就是传给free函数
			while(1)
			{
				if((*del_node) == NULL) break;
				tmp_node = &((*del_node)->next);//先备份
				free(*del_node)//在free
				*del_node = NULL;//在把 free的那个结点赋值为NULL
				 del_node  = tmp_node;//使用备份值
			}
			的
		}
	
		return 0;
	}
```

​		

# 二、双向循环链表

---

​	1、

​		双向循环链表的特点：

​				头节点的创建：

​			

```
	typedef struct loop_double_list
	{
		int data;
		struct loop_double_list * prev,*next;
	}LD,*P_LD;
	
	P_LD Create_Node();
	P_LD Create_Node()
	{
		P_LD node = (P_LD)malloc(sizeof(LD));
		if(node == NULL)
		{
			perror("malloc node:");
			return NULL;
		}
		
		node->next = node;
		node->prev = node;
		
		return node;
	}
	
	int main()
	{
		P_LD head = Create_Node();
		if(head == NULL)
		{
			printf("创建头节点失败！\n");
			return -1;
		}
		else
		{
			printf("创建头结点成功！\n");
		}
		
		return 0;
	}
```

#### 2、	实现双向循环链表的头插法



```
注意：
	不需要考虑是否第一次添加结点
	需要考虑哪边先连：
					先连右边，在连左边
					
	int Head_Add_Node(P_LD head);
	int Head_Add_Node(P_LD head)
	{
		if(头是否空)
		{
			return -1;
		}
		else
		{
			//先创建要添加的新结点
			P_LD add_node= Create_Node();
			if(add_node == NULL)
			{
				....
			}
			printf("请输入要添加的数据：");
			scanf("%d",&(add_node->data));
			
			add_node->next = head->next;
			head->next->prev = add_node;
			
			add_node->prev = head;
			head->next = add_node;
		}
	
		return 0;
	}


```



#### 3、实现双向循环链表的尾插法

```
int Tail_Add_Head(P_LD head);
int Tail_Add_Head(P_LD head)
{
	if(head == NULL)
	{
		printf("头节点为空！\n");
		return -1;
	}
	
	P_LD add_node = Create_Node();
	if(add_node == NULL)
	{
		printf("创建添加的新结点失败！\n");
		return -1;
	}
	printf("请输入要添加的数据：");
	scanf("%d",&(add_node->data));
	
	add_node->next = head;
	add_node->prev  = head->prev;
	head->prev->next = add_node;
	head->prev     = add_node;

	return 0;
}
```



#### 4、双向循环链表的结点检索

----

```
思路：
	循环遍历链表，知道tmp_node(临时指针)等于head退出循环，在遍历的过程中判断tmp_node与要搜索的结点
	find_node，如果整条链表没有要检索的find_node,也要跳出。
	
	int Find_Node(P_LD head,P_LD find_node)
	{
		if(head == NULL || find_node == NULL)
		{
			printf("头结点为NULL!,或者find_node 为空 ！\n");
			return -1;
		}
		else if(head->next == head)
		{
			printf("没有可以检索的结点！\n");
			return 0;
		}
		else
		{
			P_LD tmp_node = head->next;
			while(tmp_node != head)
			{
				if(tmp_node == find_node)
				{
					return 1;//返回值为1返回表示找到该结点
				}
				tmp_node = tmp_node->next;
			}
			
		}
		
		return 0; //这里的返回0表示没有找到find_node
	}
	
```

####    5、实现指定位置添加和删除		

```
指定位置添加：在指定的结点之后添加新的结点

int Add_Where_Node(P_LD head, P_LD where)
{
	if(head == NULL || where == NULL)
	{
		printf("头节点为NULL，或者指定添加的结点为NULL！\n");
		return -1;
	}
	else
	{	
		/*先调用检索函数，判断where是否在本链表里面*/
		int ret = Find_Node(head,where);
		if(ret != 1)
		{
			printf("添加失败！\n");
			return -1;
		}
		else
		{
			/*进行添加*/
			P_LD add_node = Create_Node();
			if(add_node == NULL)
			{
				printf("创建新添加的结点失败！\n");
				return -1;
			}
			
			printf("请输入要添加的数据：");
			scanf("%d",&(add_node->data));
			
			add_node->prev = where;
			add_node->next = where->next;
			where->next->prev = add_node;
			where->next = add_node;
			
		}
		
	}
	
	return 0;
}
	
	
	
指定位置删除结点：
	
	int Del_Where_Node(P_LD head,P_LD del_node)
	{
		if(head == NULL || del_node == NULL || head->next == head)
        {
            printf("头节点为NULL，或者指定删除的结点为NULL,或者链表为空！\n");
            return -1;
        }
        else
        {
        	int ret = Find(head,del_node);
        	if(ret != 0)
        	{
        		printf("删除失败！\n");
        		return -1;
        	}
            
            del_node->next->prev = del_node->prev;
            del_node->prev->next = del_node->next;
            del_node->prev = NULL;
            del_node->next = NULL;
        }
	
		return 0;
	}
	
```

##### 三、Linux内核链表

----

1）获取内核链表的源码

​		先获取内核源码 --> 在获取内核链表源码 （/inlcude/linux/list.h）



2）先分析源码中创建初始化头节点的结构

```c
#define LIST_HEAD_INIT(name) { &(name), &(name) }

#define LIST_HEAD(name) \
	struct list_head name = LIST_HEAD_INIT(name)
int main()
{
    LIST_HEAD(head);  ==  struct list_head head = LIST_HEAD_INIT(head)
        				  struct list_head head = { &(head), &(head) };
    return 0;
}

#define INIT_LIST_HEAD(ptr) do { \
	(ptr)->next = (ptr); (ptr)->prev = (ptr); \
} while (0)


自己在初始化头结点源码的基础上封装一个属于自己链表的头节点初始化函数。
    
 struct list_head {
	
	struct list_head *next, *prev;
	
};

typedef struct Big_Node//定义大结构体的类型
{
	int data;//数据域
	struct list_head list;//struct list_head * list;
	
}BN,*P_BN;   
 
 P_BN  Create_Node()
{
    //定义大结构体指针变量进行申请空间
    P_BN node = (P_BN)malloc(sizeof(BN));
    if(node == NULL)
    {
        perror("malloc node:");
        return NULL;
    }
    //调用
    INIT_LIST_HEAD(&(node->list));
    
    return node;
}


int main()
{
	
	
	P_BN head = Create_Node();
	if(head == NULL)
	{
		
		printf("创建头结点失败！\n");
	}
	else
	{
		Fun();
	}
	
	return 0;
}
```

---

3)添加结点

​	

```c
                                         
static inline void list_add(struct list_head *new, struct list_head *head)
{
	__list_add(new, head, head->next);
}

static inline void __list_add(struct list_head *new,
			      struct list_head *prev,
			      struct list_head *next)
{
	next->prev = new;
	new->next = next;
	new->prev = prev;
	prev->next = new;
}


struct list_head *new: 新添加的结点里面小结构体的地址  &(add_node->list)
struct list_head *head：你创建的内核链表的头结点的小结构体的地址   &(head->list)
    
该接口的正常调用：
    			list_add(&(add_node->list),&(head->list));
这里的add_node是你要添加的新结点，需要自己创建
    
封装自己的头插法添加结点：

int Add_Node(P_BN head)//形参只需要接受头节点（大结构体）
{
    if(head == NULL)
    {
        printf("链表头为空!\n");
        return -1;
    }
    else
    {
        P_BN add_node = Create_Node();
        if(add_node == NULL)
        {
            printf("添加失败！\n");
            return -1;
        }
        
        list_add(&(add_node->list),&(head->list));
        
        return 0;
    }  
    
}

```

​	

4）遍历内核链表的接口：



```
	
#define list_for_each_entry(pos, head, member)	
	for (pos = list_entry((head)->next, typeof(*pos), member),	\
		     prefetch(pos->member.next);			\
	     &pos->member != (head); 					\
	     pos = list_entry(pos->member.next, typeof(*pos), member),	\
		     prefetch(pos->member.next))
		     
pos:大结构体的名字
head:小结构体的地址
member：大结构体里面成员的名字
	
	P_BN tmp_node;
	
封装遍历链表的函数
	
	int Dispaly_Node();
	int Display_Node()
	{
		if(head->list.next == head.list)
		{
			printf("链表没数据！\n");
			return -1;
		}
		list_for_each_entry(tmp_node, &(head->list), list)
		{
			printf("%d\n",tmp_node->data);
		}
	
	}

	list_for_each_entry(tmp_node, &(head->list), list)	


```


	作业： 
	1、完成双向循环链表的结点移动 
	2、找typeof函数源码 
	3、一周总结
	①
	#include "stdio.h"
	#include "stdlib.h"
	#include "errno.h"

	typedef struct Double_Link_List
	{
		int data; //数据域
		struct Double_Link_List *next,*prev; // 指针域
	}D_LL,*P_D_LL;

	P_D_LL Create_Node();
	int Head_Add_Link_List_Node(P_D_LL head);
	int Tail_Add_Link_List_Node(P_D_LL head);
	int Site_Add_Link_List_Node(P_D_LL head,P_D_LL site);
	int Del_Site_Link_List_Node(P_D_LL head,P_D_LL del_node);
	int Move_Link_List_Node(P_D_LL head,P_D_LL move_node,P_D_LL move_site);
	int Find_Node(P_D_LL head,P_D_LL find_node);
	P_D_LL Find_Node_Data(P_D_LL head,int data);
	int Display_Link_List_Data(P_D_LL head);
	int Destroy_Link_List_Node(P_D_LL head);
	int Destroy_Link_List_Pointer_Node(P_D_LL *head);


	P_D_LL Create_Node()
	{
		P_D_LL head = (P_D_LL)malloc(sizeof(D_LL));
		if(head == NULL)
		{
			perror("malloc head:");

			return NULL;
		}

		head->next = head;
		head->prev = head;

		return head;
	}

	int Head_Add_Link_List_Node(P_D_LL head)
	{
		if(head == NULL)
		{
			printf("头结点的地址为空地址!!\n");

			return -1;
		}
		else
		{
			//P_D_LL tmp_node = head;
			P_D_LL add_node = Create_Node(); //先创建要添加的新结点
			if(add_node == NULL)
			{
				printf("新添加的结点地址为空!!添加结点失败!!\n");

				return -1;
			}

			printf("请输入要添加的数据:");
			scanf("%d", &(add_node->data));

			/*
			if(head->next == NULL) //判断是否是第一次添加
			{
				head->next = add_node;
				add->prev = head;
			}
			*/

				add_node->next = head->next; //先连右手
				head->next->prev = add_node;

				add_node->prev = head; //在连左手
				head->next = add_node;

				//书上写法
				// add_node->next = head->next;
				// add_node->prev = head;

				// head->next = add_node;
				// add_node->next->prev = add_node;

				return 0;


		}
	}

	int Tail_Add_Link_List_Node(P_D_LL head)
	{
		if(head == NULL)
		{
			printf("链表的头结点地址为空!!\n");

			return -1;
		}

		P_D_LL add_node = Create_Node();
		if(add_node == NULL)
		{
			printf("新添加的结点地址为空!!添加结点失败\n");

			return -1;
		}

		printf("请输入要添加的数据:");
		scanf("%d", &(add_node->data));

		//书上写法
		add_node->prev = head->prev;
		add_node->next = head;

		//add_node->prev->next = add_node;
		head->prev->next = add_node;
		head->prev = add_node;

		// 先连右手 再连左手
		// add_node->next = head;
		// head->next->prev = head->prev;

		// //add_node->prev = head;
		// add_node->prev = head->prev;
		// head->prev->next = add_node;


		return 0;

	}

	int Site_Add_Link_List_Node(P_D_LL head,P_D_LL site)
	{
		if(head == NULL || site == NULL)
		{
			printf("链表的头结点地址为空!!或者要添加到的结点位置的地址为空!!\n");

			return -1;
		}
		else if(head->next == head)
		{
			printf("链表中只有一个头结点!!无法添加!!\n");

			return 0;
		}
		//检索链表中是否有要添加到指定位置的结点
		//先调用检索函数 判断要添加到指定位置的结点是否在链表里面
		int ret = Find_Node(head,site);
		if(ret != 0)
		{
			printf("链表中未检索到该结点!!添加结点失败!!\n");

			return -1;
		}
		else
		{
			// 进行添加
			P_D_LL add_node = Create_Node();
			if(add_node == NULL)
			{
				printf("创建新添加的结点地址为空!!结点添加失败!!\n");

				return -1;
			}

			printf("请输入要添加的数据:");
			scanf("%d", &(add_node->data));

			add_node->next = site->next;
			add_node->prev = site;
			add_node->next->prev = add_node;
			//site->next->prev = add_node;
			site->next = add_node;

			return 0;
		}

		//return 0;
	}

	int Del_Site_Link_List_Node(P_D_LL head,P_D_LL del_node)
	{
		if(head == NULL || del_node == NULL)
		{
			printf("链表的头结点地址为空!!或者要删除的结点的地址为空!!\n");

			return -1;
		}
		else if(head->next == head || del_node == head)
		{
			printf("链表为空或者删除的结点是头结点!!\n");

			return 0;
		}

		int ret = Find_Node(head,del_node);
		if(ret != 0)
		{
			printf("删除结点失败!!\n");

			return -1;
		}

		del_node->prev->next = del_node->next;
		del_node->next->prev = del_node->prev;

		del_node->prev = NULL;
		del_node->next = NULL;

		//free(del_node);

		return 0;
	}

	int Find_Node(P_D_LL head, P_D_LL find_node)
	{
		if(head == NULL || find_node == NULL)
		{
			printf("链表头结点的地址为空!!或者要检索的结点地址为空!!\n");

			return -1;
		}
		else if(head->next == head)
		{
			printf("链表为空!!没有其它可以检索的结点!!\n");

			return 0;
		}
		
		P_D_LL tmp_node = head;
		while(tmp_node->next != find_node)
		{
			tmp_node = tmp_node->next;
			if(tmp_node == head)
			{
				printf("链表中无该结点!!\n");

				return 1;
			}

		}

		//printf("链表中有该结点!!\n");

		return 0;
	}


		//P_D_LL tmp_node = head;
		// while(1)
		// {
		// 	if(tmp_node == find_node) 
		// 	{
		// 		printf("检索结点成功!!链表里面有该结点!\n");

		// 		return 1; //返回值为1返回表示已找到该结点
		// 	}

		// 	tmp_node = tmp_node->next;
		// 	if(tmp_node == head)
		// 		break;
		// }

		// printf("检索结点失败!!链表里面找不到该结点!\n");

		// return 0; //这里的返回 0表示没有找到该结点

	int Move_Link_List_Node(P_D_LL head,P_D_LL move_node,P_D_LL move_site)
	{
		if(head == NULL || move_node == NULL || move_site == NULL)
		{
			printf("链表的头结点地址为空!!或者要移动的结点和位置为空!!\n");

			return -1;
		}
		else if(head->next == head)
		{
			printf("链表为空!!无法移动\n");

			return 0;
		}
		
			int find_1 = Find_Node(head,move_node);
			int find_2 = Find_Node(head,move_site);

			if(find_1 != 0 || find_2 != 0)
			{
				printf("检索结点失败,链表中无该结点!!\n");
				//printf("检索结点成功,链表中有该结点!!\n");

				return 1; // 1表示没有找到该结点
			}
			else
			{
				printf("检索结点成功,链表中有该结点!!\n");
			}

			int ret_1 = Del_Site_Link_List_Node(head,move_node);
			int ret_2 = Site_Add_Link_List_Node(head,move_site);

			if(ret_1 == 0 && ret_2 == 0)
			{
				printf("移动结点成功!!\n");

				return 0;
			}

		//return 0;
	}

	int Destroy_Link_List_Node(P_D_LL head)
	{
		if(head == NULL)
		{
			printf("链表的头结点地址为空!!\n");

			return -1;
		}
		else if(head->next == head)
		{
			free(head);

			return 0;
		}

		P_D_LL tmp_node = NULL;
		P_D_LL free_node = head->next;
		while(free_node != head)
		{
			tmp_node = free_node->next;
			free(free_node);
			free_node = tmp_node;
			// if(free_node == head)
			// 	break;
		}

		// P_D_LL tmp_node = NULL;
		// P_D_LL free_node = head->next;
		// while(1)
		// {
		// 	tmp_node = free_node->next;
		// 	free(free_node);
		// 	free_node = tmp_node;
		// 	if(free_node == head)
		// 		break;
		// }

		free(head);

		return 0;

	}

	int Display_Link_List_Data(P_D_LL head)
	{
		if(head == NULL)
		{
			printf("链表头结点的地址为空!!无法打印!!\n");

			return -1;
		}
		else if(head->next == head)
		{
			printf("链表当中只有一个头结点!!\n");

			return 0;
		}

		P_D_LL tmp_node = head->next;
		while(tmp_node != head) //先判断
		{
			printf("%d ", tmp_node->data); //再打印

			tmp_node = tmp_node->next;
		}

		printf("\n");

		return 0;
	}

	// 	P_D_LL tmp_node = head;
	// 	while(tmp_node->next != head) // 先判断
	// 	{`
	// 		printf("%d ", tmp_node->next->data); //再打印

	// 		tmp_node = tmp_node->next;
	// 	}

	//	printf("\n");
	// }

	int main(int argc, char const *argv[])
	{
		P_D_LL head_ret = Create_Node();
		if(head_ret == NULL)
		{
			printf("创建链表头结点失败!!\n");

			return -1;
		}
		else
		{
			printf("创建链表头结点成功!!\n");
		}

		int ret = Tail_Add_Link_List_Node(head_ret);
		if(ret != 0)
		{
			printf("添加结点失败!!\n");

			return -1;
		}
		else
		{
			printf("添加结点成功!!\n");
		}

		ret = Tail_Add_Link_List_Node(head_ret);
		if(ret != 0)
		{
			printf("添加结点失败!!\n");

			return -1;
		}
		else
		{
			printf("添加结点成功!!\n");
		}

		ret = Tail_Add_Link_List_Node(head_ret);
		if(ret != 0)
		{
			printf("添加结点失败!!\n");

			return -1;
		}
		else
		{
			printf("添加结点成功!!\n");
		}

		Display_Link_List_Data(head_ret);

		//Site_Add_Link_List_Node(head_ret,head_ret); //在head的下一个位置添加新结点
		//Site_Add_Link_List_Node(head_ret,head_ret); //在head的下一个位置添加新结点

		//Display_Link_List_Data(head_ret);

		//Del_Site_Link_List_Node(head_ret,head_ret->next);
		Move_Link_List_Node(head_ret,head_ret->next,head_ret->next->next);

		Display_Link_List_Data(head_ret);

		//ret = Destroy_Link_List_Node(&head_ret);
		ret = Destroy_Link_List_Node(head_ret);
		if(ret == 0)
		{
			printf("链表销毁成功!!\n");
		}
		else
		{
			printf("链表销毁失败!!\n");
		}

		Display_Link_List_Data(head_ret);

		printf("%p---------%p\n", head_ret,&head_ret);
		printf("%p---------%p\n", head_ret+1,&head_ret+1);
		printf("%p---------%p\n", head_ret+2,&head_ret+2);
		printf("%p---------%p\n", head_ret+3,&head_ret+3);

		// Find_Node(head_ret,head->next);
		// Find_Node(head_ret,NULL);


		free(head_ret);

		return 0;
	}

	②Linux内核中typeof是个关键字  它大概是返回变量的类型


### 省钱利器：
![](/hexo-private-blog-website/images/淘宝客18.jpg)
![](/hexo-private-blog-website/images/淘宝客19.jpg)
![](/hexo-private-blog-website/images/淘宝客20.jpg)

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
