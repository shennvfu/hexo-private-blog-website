---
title: Data_structure_06
date: 2020-10-18 18:31:18
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

	山外青山楼外楼，西湖歌舞几时休。
	暖风熏得游人醉，直把杭州作汴州。

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





# 一、内核链表

---

##### 	1、从Linux内核源码目录中获取/include/linux/list.h，然后模块化：

```c
list.h：Linux内核链表源码提供的结构
list.c:后面自己在源码的基础上在放封装成自己的api
main.c：调用API的入口
```



##### 2、先保证包含头文件编译没错

```c
①：
	注释list中的头文件包含
②：
	删掉宏定义
		#ifdef __KERNEL__ //判断__KERNEL__有没有定义（后面记得删掉endif）

③、
	现在list.c里面写一个简单的接口在main函数里面调用，然后编译
		
		int Fun()\\记得声明
		{
			printf("My kernel list!\n");
			return 0;
		}
		
	在list.h里面包含#include <stdio.h>
	
④、进行编译：
	出现list.h:611:2: warning: implicit declaration of function ‘smp_wmb’ [-Wimplicit-			function-declaration]
  		
  	方法：注释smp_wmb();
  	
⑤、把哈希表的接口删掉
	从505行	
```



##### 3、分析内核链表中每个结点的组成：

```c
在list.h中不难发现，提供了一个结构体类型：《只有指针域，没有数据域》
	struct list_head {
	struct list_head *next, *prev;
	};
	
所以需要自己再包装一下：
	第一种：
		先定义一个大的结构体类型，把自己的数据域与源码提供的结构体类型包在一起
	
	struct Big_Node
	{
		int data;//数据域
		struct list_head head; //小头
	};
	
	第二种：直接更改struct list_head，在里面直接添加数据域
	struct list_head {
            int data;
            struct list_head *next, *prev;
	};
	
```

![](/hexo-private-blog-website/images/内核链表.png)

##### 4、把包装好的结构体组成类型定义在list.h,然后包装第一个接口：初始化内核链表

```c
内核提供的初始化接口：
    #define LIST_HEAD_INIT(name) { &(name), &(name) }

    #define LIST_HEAD(name) \
        struct list_head name = LIST_HEAD_INIT(name)

    #define INIT_LIST_HEAD(ptr) do { \
        (ptr)->next = (ptr); (ptr)->prev = (ptr); \
    } while (0)
    
    
  自己封装初始化接口：
  
  P_BN Create_Node()
  {
  	P_BN node = (P_BN)malloc(sizeof(BN));
  	if(ndeo == NULL)
  	{
  		perror("malloc big node!");
  		return NULL;
  	}
  	else
  	{
  		/*指针域的操作*/
  		INIT_LIST_HEAD(&(node->head));
  		return node;
  	}
  }
  
  int main()
  {
  	P_BN Head = Create_Node();
  	if(Head == NULL)
  	{
  		printf("初始化大头节点失败！\n");
  		return -1;
  	}
  	else
  	{
  		printf("初始化头节点成功！\n");
  		Fun();
  	}
  
  	return 0;
  }
  	
```

​	

##### 5、添加结点的接口包装

```c
内核提供的添加结点的接口：
	static inline void list_add(struct list_head *new, struct list_head *head) 
	static inline void list_add_tail(struct list_head *new, struct list_head *head)
	
	
list_add:头插法
	new：小头类型（新创建的要添加进链表里面的小头）
	head：小头（大头结点里面的小头）
	
调用实例：
	//先创建要添加的结点
	P_BN add_node = Create_Node();
	/*键盘输入添加的数据*/
	list_add(&(add_node->head),&(Head->head));
```



##### 6、遍历内核链表接口的包装：

​	

```c
内核提供的遍历结点的接口：
	1）只遍历小头
	for (pos = (head)->next, prefetch(pos->next); pos != (head); \
        pos = pos->next, prefetch(pos->next))
	
	pos:每次遍历到的小头结构体指针
	head：大头结点里面的小头
	
	2）遍历大头（能获取大头结点）
		#define list_for_each_entry(pos, head, member)
		for (pos = list_entry((head)->next, typeof(*pos), member)	\
		;			\
	     &pos->member != (head); 					\
	     pos = list_entry(pos->member.next, typeof(*pos), member)	\
		    )
		    
		    
	#define list_entry(ptr, type, member) \
	container_of(ptr, type, member)
	pos:每次遍历到的大头结构体指针
	typeof(*pos)：获取大结构体的类型
	head：大头结点里面的小头
	member：大结构体里面任意的成员们名字（通过结构体里面的成员，找到该结构体的地址）
	
	list_entry： 找大头结构体的位置的接口
	
	#define list_entry(ptr, type, member) \
		container_of(ptr, type, member)
		
	ptr:第二个大头里面的小头结构体
	type：获取大结构体的类型
	member：小头的名字
	
	
	封装遍历大头结点的函数时，编译报错：
	
		container_of：没这个函数 （找出来---/include/linux/kernel.h里面）
		
		#define container_of(ptr, type, member) ({			\
        const typeof( ((type *)0)->member ) *__mptr = (ptr);	\
        (type *)( (char *)__mptr - offsetof(type,member) );})
        
        (char *)__mptr：新存放的小结构体的地址
        ( (char *)__mptr - offsetof(type,member) ) 把小结构体的地址减去偏移量（）
        
       关于((type *)0)->member的分析：把0地址强制变成结构体的地址
       struct A()
       {
      	 int a;
      	 int b;
      }
      (type) = struct A
      (type *) = struct A *
        
      再次编译的时候，报错 没有offsetof（找出来---/include/linux/stddef.h里面)
```



##### 7、删除结点的封装

```c
提供的删除接口：
	static inline void list_del(struct list_head *entry) 指定结点删除
	
	entry：指定要删除的结点的小头

int Del_Node(P_BN Head)
{
	if(Head == NULL)
	{
		printf("Head 为 NULL,无法删除！\n");
		return -1;
	
	}
	else if(Head->head.next == &(Head->head)) //判断链表是否为空
	{
		printf("链表为空！\n");
	}
	else
	{
		//再判断删除的结点是否再链表里面
	}

	return 0;
}
```



##### 8、先封装检索链表的函数

```c
只需要再调用遍历链表的接口之前，判断头结点和要检索的结点是否可以用，再调用list_for_each_entry

int Find_Node(P_BN Head,P_BN find_node)
{
	if(Head == NULL || find_node == NULL)
	{
		printf("结点无效！检索失败！\n");
		return -1;
	}
	else if((Head->head.next == &(Head->head))
	{
		printf("链表为空！\n");
		return 1;
	}
	else
	{
		P_BN tmp_node = NULL;
		list_for_each_entry(tmp_node,&(Head->head),head)
		{
			if(tmp_node == find_node)
				return 0;
		}
	}
	return -1;//检索了，但是找不到要找的结点
}
```

##### 9、移动结点

```c
指定结点移动到头结点的后面
	static inline void list_move(struct list_head *list, struct list_head *head)

指定结点移动到头结点的前面
static inline void list_move_tail(struct list_head *list,
				  struct list_head *head)
				  
list: 要移动的结点（小结构体指针类型）
head: （第一个小结构体）

int List_Move_Head(P_BN Head，P_BN move_node)//形参需要两个：第一个大头结点 要移动的大头结点
{
	if(Head == NULL || move_node == NULL)
	{
		printf("Head 为NULL，移动失败！\n");
		return -1;
	}
	else if(Head == NULL)
	{
		printf("表为空！\n");
		return 1;
	}
	else
	{
		//先检表中是否有move_node
		int ret = Find_Node(Head,move_node);
		if(ret != 0)
		{
			printf("表中没有这个结点可以移动，移动失败！\n");
			return -1;
		}
		else
		{
			list_move(&(move_node->head),&(Head->head));
			return 0;
		}
	}
}
```



##### 练习：

​	封装检索小结构体结点的接口函数

​	再封装删除的函数



##### 作业：

​		实现内核链表数据检索的接口

​		实现内核链表结点的删除

​		举例队列和栈的应用场景

​	

---

# 二、栈和队列

##### 	1）特点

```
主要分为两种：
			每个数据点的地址是否是连续的来进行分类：
			顺序： 顺序栈、顺序队列
			链式： 链式栈，链式队列
			
存储数据的特点：
			队列 ：先进先出
			栈   ：先进后出
 			链式栈：受限制的单向链表 （压栈  出栈 初始栈 遍历栈）
 			压栈：头插法
 			出栈：头删法
```

![](/hexo-private-blog-website/images/栈.png)

##### 2）初始化链式栈

```c
根据以上结构图，初始化链式栈：
	
	第一步：先设计结构体类型（有两个结构体类型）
	
	结点结构体：
		typedef struct stack_node
		{
			int data;
			struct stack_node * next;
		}SN,*P_SN;
	栈结构体：
		typedef struct Stack_Ctrl
		{
			struct stack_node * top,*bottom;
			int height;
		}SC,*P_SC;
		
	
	第二步：对结构体里面的数据域、指针域初始化
	P_SC Init_Stack()
	{
		P_SC stack = (P_SC)malloc(sizeof(SC));
		if(stack == NULL)
		{
			perror("malloc stack...");
			return NULL;
		}
		stack->top = stack->bottom = NULL;
		stack->height = 0;

		return stack;
	}

```

##### 3）压栈

```c
对于栈结构体的指针域操作：
	第一次压栈：
				top 和 bottom指向同一个结点
	
	不是第一次压栈：
				top指向第一个
				bottom指向最后一个
	height++;
	
int Stack_Push(P_SC stack)
{
	if(stack == NULL)
	{
		printf("stack为NULL，压栈失败！\n");
		return -1;
	}
	else
	{
		//创建栈结点
		P_SN new_node = (P_SN)malloc(sizeof(SN));
        if(new_node == NULL)
        {
            printf("创建栈结点失败,压栈失败！\n");
            return -1;
        }
        printf("请输入要压栈的数据：");
        scanf("%d",&(new_node->data));
		if(stack->top == NULL)//判断是不是第一次压栈
		{
			stack->top = stack->bottom = new_node;
			new_node->next = NULL;
			
		}
		else
		{
			new_node->next =top;
			top = new_node;
		}
		
		stack->height++;
		return 0;
	}
}
```

	作业：
	​	实现内核链表数据检索的接口
	​	实现内核链表结点的删除
	​	举例队列和栈的应用场景
	①
	#include "list.h"

	int Fun_Kernel_List()
	{
		printf("my kernel list!!\n");
		
		return 0;
	}

	P_BN Create_Node()
	{
		P_BN node = (P_BN)malloc(sizeof(BN)); //定义大结构体指针变量进行申请内存空间
		if(node == NULL)
		{
			perror("malloc Big node:");

			return NULL;
		}
		else
		{
			INIT_LIST_HEAD(&(node->head)); //因为大结构体里面的小结构体不是指针类型  因此需要加上取地址符&

			return node;
		}
	}
		// 调用初始化结点函数接口
		//INIT_LIST_HEAD(&(head->list)); //该函数接口需要传一个结构体指针变量
		//LIST_HEAD(&(head->list));

		/*
		#define LIST_HEAD_INIT(name) { &(name), &(name) }

		#define LIST_HEAD(name) \
			struct list_head name = LIST_HEAD_INIT(name)

		#define INIT_LIST_HEAD(ptr) do { \
			(ptr)->next = (ptr); (ptr)->prev = (ptr); \
		} while (0)
		*/
	//}

	int Head_Add_Link_List_Node(P_BN Head) //头插法 (看内核链表源码里的list_add)
	{
		if(Head == NULL)
		{
			printf("大结构体的头结点地址为空!!添加结点失败!!\n");

			return -1;
		}
		else
		{
			//先创建要添加的新的结点
			P_BN add_node = Create_Node();
			if(add_node == NULL)
			{
				printf("创建的新的结点添加失败!!\n");

				return -1;
			}
			else
			{
				printf("请输入要添加到大头里面的数据:");
				scanf("%d", &(add_node->data));
				//printf("请输入要添加到小头里面的数据:");
				//scanf("%d", &(add_node->head.data));
			}

			//再调用内核链表提供的添加结点的接口  就是帮你操作指针域的接口
			//list_add_tail(struct list_head *new,struct list_head *head);
			list_add_tail(&(add_node->head),&(Head->head)); //要添加的结点的小头和大结构体里的小头  因为操作的是指针域所以需要取地址  （看内核链表源码，它调用了__list_add）
			return 0;
		}
	}

	int Tail_Add_Link_List_Node(P_BN Head)  //尾插法（看内核链表源码里的list_add_tail）
	{
		if(Head == NULL)
		{
			printf("大结构体的头结点地址为空!!添加结点失败!!\n");

			return -1;
		}
		else
		{
			//先创建要添加的新的结点
			P_BN add_node = Create_Node();
			if(add_node == NULL)
			{
				printf("创建的新的结点添加失败!!\n");

				return -1;
			}
			else
			{
				printf("请输入要添加到大头里面的数据:");
				scanf("%d", &(add_node->data));
				//printf("请输入要添加到小头里面的数据:");
				//scanf("%d", &(add_node->head.data));
			}

			//再调用内核链表提供的添加结点的接口
			//list_add_tail(struct list_head *new,struct list_head *head)
			list_add_tail(&(add_node->head),&(Head->head)); //要添加的结点的小头和大结构体里的小头  因为操作的是指针域所以需要取地址 //（看内核链表源码，它调用了__list_add）

			return 0;
		}
	}

	int Move_Link_List_Head_Node(P_BN Head,P_BN move_node) //形参需要两个:第一个大头结点 要移动的大头结点
	{
		if(Head == NULL || move_node == NULL)
		{
			printf("大结构体的头结点地址为空!!或者要移动的结点的地址为空!!移动结点失败\n");

			return -1;
		}
		else if(Head->head.next == &(Head->head))
		{
			printf("链表为空!!移动结点失败!!\n");

			return 1;
		}
		else
		{
			//先检索链表当中是否有要移动的大头结点
			int ret = Find_Link_List_Node(Head,move_node);
			if(ret != 0)
			{
				printf("链表当中无该结点可以移动!!移动结点失败!!\n");

				return -1;
			}
			else
			{
				list_move(&(move_node->head),&(Head->head));

				printf("移动结点成功!!\n");

				return 0;
			}
		}
	}

	int Del_Link_List_Little_Head_Node(P_BN Head,struct list_head *del_node)
	{
		if(Head == NULL || del_node == NULL)
		{
			printf("大结构体的头结点地址为空!!或者要删除的结点的地址为空!!删除结点失败!!\n");

			return -1;
		}
		else if(Head->head.next == &(Head->head)) //判断链表是否为空
		{
			printf("链表为空!!删除结点失败!!\n");

			return 1;
		}
		else
		{
			//再判断删除的结点是否在链表里面
			int ret = Find_Link_List_Little_Node(Head,del_node);
			if(ret != 0)
			{
				printf("链表当中无该结点可以删除!!删除结点失败!!\n");

				return -1;
			}
			else
			{
				list_del(del_node);

				return 0;
			}
		}
	}

	int Del_Link_List_Head_Node(P_BN Head,P_BN del_node)
	{
		if(Head == NULL || del_node == NULL)
		{
			printf("大结构体的头结点地址为空!!或者要删除的结点的地址为空!!删除结点失败\n");

			return -1;
		}
		else if(Head->head.next == &(Head->head)) //判断链表是否为空
		{
			printf("链表为空!!删除结点失败!!\n");

			return 1;
		}
		else
		{
			//再判断删除的结点是否在链表里面
			int ret = Find_Link_List_Node(Head,del_node);
			if(ret != 0)
			{
				printf("链表当中无该结点可以删除!!删除结点失败\n");

				return -1;
			}
			else
			{
				list_del(&(del_node->head));

				return 0;
			}
		}
	}

	int Find_Link_List_Node_Data(P_BN Head) //根据大头里的数据寻找节点
	{
		int find_data;
		if(Head == NULL)
		{
			printf("大结构体的头结点地址为空!!\n");

			return -1;
		}
		else if(Head->head.next == &(Head->head))
		{
			printf("链表为空!!检索数据失败!!\n");

			return 1;
		}
		else
		{
			printf("请输入想要在链表中检索的数据:");
			scanf("%d", &find_data);
			P_BN tmp_node = NULL;
			//list_for_each(tmp_node,&(Head->head))
			list_for_each_entry(tmp_node,&(Head->head),head)
			{
				if(tmp_node->data == find_data)
				{
					printf("链表中有该数据!!数据已找到%d\n", tmp_node->data);

					return 0;
				}
			}
		}

		printf("链表内无该数据!!\n");

		return 2;
	}

	int Find_Link_List_Node(P_BN Head,P_BN find_node)
	{
		if(Head == NULL || find_node == NULL)
		{
			printf("大结构体的头结点地址为空!!或者要检索的结点的地址为空!!检索结点失败!!\n");

			return -1;
		}
		else if(Head->head.next == &(Head->head))
		{
			printf("链表为空!!检索结点失败!!\n");

			return 1;
		}
		else
		{
			P_BN tmp_node = NULL;
			//list_for_each(tmp_node,&(Head->head))
			list_for_each_entry(tmp_node,&(Head->head),head)
			{
				if(tmp_node == find_node)
					return 0;
			}
		}

		return -1; //检索了,但是找不到要找的结点
	}

	int Find_Link_List_Little_Node(P_BN Head,struct list_head * find_little_node)
	{
		if(Head == NULL || find_little_node == NULL)
		{
			printf("大结构体的头结点地址为空!!或者要检索的小头结点的地址为空!!检索结点失败\n");

			return -1;
		}
		else if(Head->head.next == &(Head->head))
		{
			printf("链表为空!!检索结点失败!!\n");

			return -1;
		}
		else
		{
			struct list_head * tmp_node = NULL;
			list_for_each(tmp_node,&(Head->head))
			//list_for_each_entry(tmp_node,&(Head->head),head)
			{
				if(tmp_node == find_little_node)
					return 0;
			}
		}

		return -1; //检索了,但是找不到要找的结点
	}

	int Display_Little_Head_Data(P_BN Head)
	{
		if(Head == NULL)
		{
			printf("大结构体的头结点地址为空!!遍历结点数据失败!!\n");

			return -1;
		}
		else if(Head->head.next == &(Head->head)) //判断是否是只有一个头结点
		{
			printf("链表为空!!没有可以遍历的数据!!\n");

			return 0;
		}
		else
		{
			struct list_head * tmp_node = NULL;
			list_for_each(tmp_node,&(Head->head))
			{
				printf("%d ", tmp_node->data);
			}

			printf("\n");
		}

		return 0;
	}

	int Display_Big_Head_Data(P_BN Head)
	{
		if(Head == NULL)
		{
			printf("大结构体的头结点地址为空!!遍历结点数据失败\n");

			return -1;
		}
		else if(Head->head.next == &(Head->head)) // 判断是否是只有一个头结点
		{
			printf("链表为空!!没有可以遍历的数据\n");

			return 0;
		}
		else
		{
			P_BN tmp_node = NULL;
			list_for_each_entry(tmp_node,&(Head->head),head)
			{
				printf("%d ", tmp_node->data);
			}

			printf("\n");
		}

		return 0;
	}
	执行：

	②
	#include "list.h"

	int Fun_Kernel_List()
	{
		printf("my kernel list!!\n");
		
		return 0;
	}

	P_BN Create_Node()
	{
		P_BN node = (P_BN)malloc(sizeof(BN)); //定义大结构体指针变量进行申请内存空间
		if(node == NULL)
		{
			perror("malloc Big node:");

			return NULL;
		}
		else
		{
			INIT_LIST_HEAD(&(node->head)); //因为大结构体里面的小结构体不是指针类型  因此需要加上取地址符&

			return node;
		}
	}
		// 调用初始化结点函数接口
		//INIT_LIST_HEAD(&(head->list)); //该函数接口需要传一个结构体指针变量
		//LIST_HEAD(&(head->list));

		/*
		#define LIST_HEAD_INIT(name) { &(name), &(name) }

		#define LIST_HEAD(name) \
			struct list_head name = LIST_HEAD_INIT(name)

		#define INIT_LIST_HEAD(ptr) do { \
			(ptr)->next = (ptr); (ptr)->prev = (ptr); \
		} while (0)
		*/
	//}

	int Head_Add_Link_List_Node(P_BN Head) //头插法 (看内核链表源码里的list_add)
	{
		if(Head == NULL)
		{
			printf("大结构体的头结点地址为空!!添加结点失败!!\n");

			return -1;
		}
		else
		{
			//先创建要添加的新的结点
			P_BN add_node = Create_Node();
			if(add_node == NULL)
			{
				printf("创建的新的结点添加失败!!\n");

				return -1;
			}
			else
			{
				printf("请输入要添加到大头里面的数据:");
				scanf("%d", &(add_node->data));
				//printf("请输入要添加到小头里面的数据:");
				//scanf("%d", &(add_node->head.data));
			}

			//再调用内核链表提供的添加结点的接口  就是帮你操作指针域的接口
			//list_add_tail(struct list_head *new,struct list_head *head);
			list_add_tail(&(add_node->head),&(Head->head)); //要添加的结点的小头和大结构体里的小头  因为操作的是指针域所以需要取地址  （看内核链表源码，它调用了__list_add）
			return 0;
		}
	}

	int Tail_Add_Link_List_Node(P_BN Head)  //尾插法（看内核链表源码里的list_add_tail）
	{
		if(Head == NULL)
		{
			printf("大结构体的头结点地址为空!!添加结点失败!!\n");

			return -1;
		}
		else
		{
			//先创建要添加的新的结点
			P_BN add_node = Create_Node();
			if(add_node == NULL)
			{
				printf("创建的新的结点添加失败!!\n");

				return -1;
			}
			else
			{
				printf("请输入要添加到大头里面的数据:");
				scanf("%d", &(add_node->data));
				//printf("请输入要添加到小头里面的数据:");
				//scanf("%d", &(add_node->head.data));
			}

			//再调用内核链表提供的添加结点的接口
			//list_add_tail(struct list_head *new,struct list_head *head)
			list_add_tail(&(add_node->head),&(Head->head)); //要添加的结点的小头和大结构体里的小头  因为操作的是指针域所以需要取地址 //（看内核链表源码，它调用了__list_add）

			return 0;
		}
	}

	int Move_Link_List_Head_Node(P_BN Head,P_BN move_node) //形参需要两个:第一个大头结点 要移动的大头结点
	{
		if(Head == NULL || move_node == NULL)
		{
			printf("大结构体的头结点地址为空!!或者要移动的结点的地址为空!!移动结点失败\n");

			return -1;
		}
		else if(Head->head.next == &(Head->head))
		{
			printf("链表为空!!移动结点失败!!\n");

			return 1;
		}
		else
		{
			//先检索链表当中是否有要移动的大头结点
			int ret = Find_Link_List_Node(Head,move_node);
			if(ret != 0)
			{
				printf("链表当中无该结点可以移动!!移动结点失败!!\n");

				return -1;
			}
			else
			{
				list_move(&(move_node->head),&(Head->head));

				printf("移动结点成功!!\n");

				return 0;
			}
		}
	}

	int Del_Link_List_Little_Head_Node(P_BN Head,struct list_head *del_node)
	{
		if(Head == NULL || del_node == NULL)
		{
			printf("大结构体的头结点地址为空!!或者要删除的结点的地址为空!!删除结点失败!!\n");

			return -1;
		}
		else if(Head->head.next == &(Head->head)) //判断链表是否为空
		{
			printf("链表为空!!删除结点失败!!\n");

			return 1;
		}
		else
		{
			//再判断删除的结点是否在链表里面
			int ret = Find_Link_List_Little_Node(Head,del_node);
			if(ret != 0)
			{
				printf("链表当中无该结点可以删除!!删除结点失败!!\n");

				return -1;
			}
			else
			{
				list_del(del_node);

				return 0;
			}
		}
	}

	int Del_Link_List_Head_Node(P_BN Head,P_BN del_node)
	{
		if(Head == NULL || del_node == NULL)
		{
			printf("大结构体的头结点地址为空!!或者要删除的结点的地址为空!!删除结点失败\n");

			return -1;
		}
		else if(Head->head.next == &(Head->head)) //判断链表是否为空
		{
			printf("链表为空!!删除结点失败!!\n");

			return 1;
		}
		else
		{
			//再判断删除的结点是否在链表里面
			int ret = Find_Link_List_Node(Head,del_node);
			if(ret != 0)
			{
				printf("链表当中无该结点可以删除!!删除结点失败\n");

				return -1;
			}
			else
			{
				list_del(&(del_node->head));

				return 0;
			}
		}
	}

	int Find_Link_List_Node_Data(P_BN Head) //根据大头里的数据寻找节点
	{
		int find_data;
		if(Head == NULL)
		{
			printf("大结构体的头结点地址为空!!\n");

			return -1;
		}
		else if(Head->head.next == &(Head->head))
		{
			printf("链表为空!!检索数据失败!!\n");

			return 1;
		}
		else
		{
			printf("请输入想要在链表中检索的数据:");
			scanf("%d", &find_data);
			P_BN tmp_node = NULL;
			//list_for_each(tmp_node,&(Head->head))
			list_for_each_entry(tmp_node,&(Head->head),head)
			{
				if(tmp_node->data == find_data)
				{
					printf("链表中有该数据!!数据已找到%d\n", tmp_node->data);

					return 0;
				}
			}
		}

		printf("链表内无该数据!!\n");

		return 2;
	}

	int Find_Link_List_Node(P_BN Head,P_BN find_node)
	{
		if(Head == NULL || find_node == NULL)
		{
			printf("大结构体的头结点地址为空!!或者要检索的结点的地址为空!!检索结点失败!!\n");

			return -1;
		}
		else if(Head->head.next == &(Head->head))
		{
			printf("链表为空!!检索结点失败!!\n");

			return 1;
		}
		else
		{
			P_BN tmp_node = NULL;
			//list_for_each(tmp_node,&(Head->head))
			list_for_each_entry(tmp_node,&(Head->head),head)
			{
				if(tmp_node == find_node)
					return 0;
			}
		}

		return -1; //检索了,但是找不到要找的结点
	}

	int Find_Link_List_Little_Node(P_BN Head,struct list_head * find_little_node)
	{
		if(Head == NULL || find_little_node == NULL)
		{
			printf("大结构体的头结点地址为空!!或者要检索的小头结点的地址为空!!检索结点失败\n");

			return -1;
		}
		else if(Head->head.next == &(Head->head))
		{
			printf("链表为空!!检索结点失败!!\n");

			return -1;
		}
		else
		{
			struct list_head * tmp_node = NULL;
			list_for_each(tmp_node,&(Head->head))
			//list_for_each_entry(tmp_node,&(Head->head),head)
			{
				if(tmp_node == find_little_node)
					return 0;
			}
		}

		return -1; //检索了,但是找不到要找的结点
	}

	int Display_Little_Head_Data(P_BN Head)
	{
		if(Head == NULL)
		{
			printf("大结构体的头结点地址为空!!遍历结点数据失败!!\n");

			return -1;
		}
		else if(Head->head.next == &(Head->head)) //判断是否是只有一个头结点
		{
			printf("链表为空!!没有可以遍历的数据!!\n");

			return 0;
		}
		else
		{
			struct list_head * tmp_node = NULL;
			list_for_each(tmp_node,&(Head->head))
			{
				printf("%d ", tmp_node->data);
			}

			printf("\n");
		}

		return 0;
	}

	int Display_Big_Head_Data(P_BN Head)
	{
		if(Head == NULL)
		{
			printf("大结构体的头结点地址为空!!遍历结点数据失败\n");

			return -1;
		}
		else if(Head->head.next == &(Head->head)) // 判断是否是只有一个头结点
		{
			printf("链表为空!!没有可以遍历的数据\n");

			return 0;
		}
		else
		{
			P_BN tmp_node = NULL;
			list_for_each_entry(tmp_node,&(Head->head),head)
			{
				printf("%d ", tmp_node->data);
			}

			printf("\n");
		}

		return 0;
	}
	执行：

	③
	栈：括号匹配问题、四则运算、迷宫
	栈和队列的特性不一样，队列的特性是先进先出，栈是先进后出
	栈的出口和入口只有一个，都是从一个地方进入或者出去，所以进去的时候，就不能出来，而且出来的时候，只有最后进入的能够出来
	比较适合于先进来等着，等到满足某种条件的时候，才按照后来的先处理。
	队列：交互式程序、生产消费队列、消息队列
	队列是一种常见的数据结构，主要的特性先进来的元素会先出去，也就是First In First Out:FIFO ，所以适合具有这个特性的算法来使用，比如图的广度优先搜索、二叉树的层次遍历等。
	所以队列和生活中的排队非常相似，先来的先服务，后来的后服务。


### 省钱利器：
![](/hexo-private-blog-website/images/淘宝客21.jpg)
![](/hexo-private-blog-website/images/淘宝客22.jpg)
![](/hexo-private-blog-website/images/淘宝客23.jpg)

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


